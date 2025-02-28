---
layout: default
title: How does de-identification work?
weight: 70
description: Technical explanations about the de-identification process
---

Karnak is a gateway for sending DICOM files to one or multiple Application Entity Title (AET). Karnak offers the possibility to configure multiple destinations for an AET.
These destinations can communicate using the DICOM or DICOM WEB protocol.

A destination contains several configurations for the DICOM endpoint, the credentials, and is also linked to a project.

A project defines the de-idenfication or tag morphing method and a secret that will be used to generate deterministic random values like UIDs or shift date arguments.

## Basic Profile

The reference profile for de-identifying DICOM objects is provided by the [DICOM standard](http://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html). This profile defines an exhaustive list of DICOM tags and their related action to allow the de-identification of the instance.

In order to properly de-identify the sensitive data, five different actions are defined in the standard:

* D – Replace with a dummy value
* Z – Set to null
* X – Remove
* K – Keep
* U – Replace with a new UID

The Basic Profile defines one or more actions to be applied to a list of tags. 

The [DICOM's type](http://dicom.nema.org/dicom/2013/output/chtml/part05/sect_7.4.html) is often dependent on the [Information Object Definition (IOD)](http://dicom.nema.org/medical/dicom/current/output/chtml/part04/chapter_6.html) of the instance. To avoid DICOM corruption, multiple actions can be defined for a tag, ensuring that a destructive action like REMOVE won't be applied on a Type 1 or Type 2 attribute.

* Z/D – Z unless D is required to maintain IOD conformance (Type 2 versus Type 1)
* X/Z – X unless Z is required to maintain IOD conformance (Type 3 versus Type 2)
* X/D – X unless Z is required to maintain IOD conformance (Type 3 versus Type 1)
* X/Z/D – X unless Z or D is required to maintain IOD conformance (Type 3 versus Type 2 versus Type 1)
* X/Z/U* – X unless Z or replacement of contained instance UIDs (U) is required to maintain IOD conformance (Type 3 versus Type 2 versus Type 1 sequences containing UID references)

Karnak loads the SOPs and attributes as specified in the DICOM Standard. Based on the tag's type in the current instance, the proper action is set and applied. If the tag cannot be identified in the SOP or its type cannot be inferred, the strictest action will be applied (U/D > Z > X). 

Below is a concrete illustration of the action applied in case of multiple actions defined in the Basic Profile:

* Z/D, X/D, X/Z/D → apply action D
* X/Z → apply action Z
* X/Z/U, X/Z/U* → apply action U

## Action D, without a dummy value

The action D replaces the tag value with a dummy one. This value must be consistent with the [Value Representation](http://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html) (VR) of the tag.

Karnak will use a default value based on the VR in this case, as defined below:

* AE, CS, LO, LT, PN, SH, ST, UN, UT, UC, UR → “UNKNOWN”
* DS, IS → “0”
* AS, DA, DT, TM → a date is generated using shiftRange(), as explained in the [Shift Date](#shift-date-generate-a-random-date) section
* UI → a new UID is generated using the [Action U](#action-u-generate-a-new-uid)

The *shiftRange()* action will return a random value between a given maximum days and seconds. By default, maximum days is set to **365** and maximum seconds is set to **86400**.

The following VRs FL, FD, SL, SS, UL, US are of type Binary. By default, Karnak will set to null the value of this VR.

## Action U, Generate a new UID

For each U action, Karnak will hash the input value. A one-way function is created to ensure that it is not possible to revert to the original UID. This function will hash the input UID and generate a new UID from the hashed input.

### Context

It’s possible for a DICOM study to be de-identified several times and in different ways, potentially implying the use of multiple hashing and one-way functions. Karnak ensures deterministic generation of UIDs in order to maintain the quality and usability of the data.

To achieve that behavior, a project must be created and associated to the destination that requires de-identification. A project defines a de-idenfication method and a secret, either generated randomly or imported by the user. **The project's secret will be used as key for the HMAC.**

#### Project secret

**The secret is 16 bytes long** and randomly defined when the project is created.

A user can upload his own secret, but it must be 16 bytes long. It can be uploaded as a String in hexadecimal format.

### Hash function

The algorithm used for hashing is the “Message Authentication Code” (MAC). Karnak uses the MAC, not as message authentication, but as a one-way function. Below is a definition from the [JAVA Mac class](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/javax/crypto/Mac.html) used in Karnak:

« *A MAC provides a way to check the integrity of information transmitted over or stored in an unreliable medium, based on a secret key. Typically, message authentication codes are used between two parties that share a secret key in order to validate information transmitted between these parties.*

*A MAC mechanism that is based on cryptographic hash functions is referred to as HMAC. HMAC can be used with any cryptographic hash function, e.g., SHA256 or SHA384, in combination with a secret shared key. HMAC is specified in RFC 2104.* »

For each use of the HMAC, it uses the **SHA256** hash function **combined with the project's secret**.

### Generate UID

« *What DICOM calls a "UID" is referred to in the ISO OSI world as an Object Identifier (OID)* » [[1]] 

To generate a new DICOM UID, Karnak will create an OID beginning with “2.25”, that is an OID encoded UUID. 

« *The value after "2.25." is the straight decimal encoding of the UUID as an integer. It MUST be a direct decimal encoding of the single integer, all 128 bits.* » [[2]]

[1]: <https://www.dclunie.com/medical-image-faq/html/part2.html>
[2]: https://wiki.ihe.net/index.php/Creating_Unique_IDs_-_OID_and_UUID "How do you create an OID ?"

The generated UUID will use the first 16 bytes (128 bits) from the hash value. The UUID is a type 4 with a variant 1. See the pseudocode below to ensure the type and the variant are correct in the UUID:

```
// Version
uuid[6] &= 0x0F
uuid[6] |= 0x40

// Variant
uuid[8] &= 0x3F
uuid[8] != 0x80
```

The hashed value will be converted in a positive decimal number and appended to the OID root separated by a dot. See the example below:

```
OID_ROOT = “2.25”
uuid = OID_ROOT + “.” + HashedValue[0:16].toPositiveDecimal()
```

## Shift Date, Generate a random date

Karnak implements a randomized date shifting action. This shift must be identical based on the project and **the patient** for data consistency. For example, if a random shift is made for the birthdate of the patient "José Santos", it must be the same for each instance associated to "José Santos", even if the instance is loaded later.

The random shift date action will use the HMAC defined above and a range of days or seconds defined by the user. If the minimum isn't specified, it defaults to 0.

The patientID, along with the project's secret, will be passed to the HMAC, ensuring data consistency by patient.

The code below illustrates how a random value is generated within a given minimum (inclusive) and maximum (exclusive) range.

```
scaleHash(PatientID, scaledMin, scaledMax):
    patientHashed = hmac.hash(PatientID)
    scale = scaledMax - scaledMin

    shift = (patientHashed[0:6].toPositiveDecimal() * scale) + scaledMin
```

## Pseudonym

This chapter details the problems linked to the PatientID generation for different de-identification methods. 

A patient can participate in several studies using different de-identification methods. Depending on the project or the clinical research, the de-identification profile can keep some of the patient's metadata. A pseudonym and a patientID are generated and affected to the patient in order to identify him in the context of the project.

Most of the patient's identifying information is contained in the [Patient Module](http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_C.2.html). 

Below is illustrated a case where some patient's data could be leaked. During the de-identification, the patient is associated with a pseudonym provided by an external service or mapping table.
In this example, a patient's study falls in the scope of two different projects in Karnak. The first de-identification removes the patient birthdate (*3. Apply Project 1*) and the second keeps it (*5. Apply Project 2*). If the patient pseudonym is used as patient identification in the Patient Module, and the data is reconciled between the two study, the birthdate will be leaked. 

![Pseudonymization](/profiles/pseudonymization_karnak.png)

Below is illustrated an alternative way of handling pseudonyms during the de-identification.

In this example, the patient is still associated with a pseudonym provided by an external service or mapping table. But Karnak will generate a PatientID based on this pseudonym and other characteristics linked to the project specifically. This ID will be used to identify the patient in the context of the project. In case of different de-identification methods in different projects, the patients cannot be reconciled and data won't be leaked.  

![Generate PatientID](/profiles/pseudonymization_patientIDGenerated_karnak.png)

### PatientID generation

Karnak generates a PatientID to solve the problem explained previously. The PatientID is generated using the HMAC function defined in the project, see chapter [Action U, Generate a new UID](#action-u-generate-a-new-uid) for more details.

The de-identified patientID is generated as follows :

* the patient's pseudonym is retrieved from an external service or a mapping table 
* the pseudonym is hashed using the hmac function and the project's secret, making it unique and deterministic in the context of the project
* the patientID is set to the first 16 bytes of the hashed pseudonym

The pseudonym will be used as the patient's name if no other action has been defined during de-identification. 

### Keep the correspondence between pseudonym and patient

It is possible to retrieve the patient information once de-identified.

The patientID is generated from the pseudonym and the project's secret using the hmac function, making it irreversible. The pseudonym is stored in the attribute **Clinical Trial Subject ID** **(0012,0040)**. Using the mapping table or the service that provided the pseudonym, it is possible to retrieve the original patient identity.

« *The Clinical Trial Subject ID (0012,0040) identifies the subject within the investigational protocol specified by Clinical Trial Protocol ID (0012,0020).* » [[3]]

[3]: http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_C.7.html#sect_C.7.1.3.1.6
## Attributes added by Karnak

Some attributes are automatically set by Karnak during de-identification in the following modules.

### SOP Common

The following attributes are set in the [*SOP Common module*](http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_C.12.html#sect_C.12.1) during de-identification:

* **Instance Creation Time (0008,0013)**, is set to the time the SOP instance was created. The value representation used is TM (HHMMSS.FFFFFF).
* **Instance Creation Date (0008,0012)**, is set to the date the SOP instance was created. The value representation used is DA (YYYYMMDD).

### Patient Module

The following attributes are set in the [*SOP Common module*](http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_C.12.html#sect_C.12.1) during de-identification:

* **Patient ID (0010,0020)**, is set to the hashed pseudonym, see [PatientID Generation](#patientid-generation) for more details
* **Patient Name (0010,0010)** is set to the pseudonym if no other action is applied to that tag during de-identification
* **Patient Identity Removed (0012,0062)** is set to **YES**
* **De-identification Method (0012,0063)** is set to the concatenated profile element codenames applied to the instance in order of application

The profile element codenames are concatenated and separated by **-**. 
For example, a profile composed of the profile elements *action.on.specific.tags* and *basic.dicom.profile* will appear as *action.on.specific.tags-basic.dicom.profile*.

### Clinical Trial Subject Module

The following attributes are set in the [*Clinical Trial Subject Module*](http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_C.7.html#sect_C.7.1.3) during de-identification:

* **Clinical Trial Sponsor Name (0012,0010)** is set to the project name
* **Clinical Trial Protocol ID (0012,0020)** is set to the profile codename (concatenated profile elements' codename)
* **Clinical Trial Protocol Name (0012,0021** is set to null
* **Clinical Trial Site ID (0012,0030)** is set to null
* **Clinical Trial Site Name (0012,0031)** is set to null
* **Clinical Trial Subject ID (0012,0040)** is set to the patient's pseudonym