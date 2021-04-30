---
layout: default
title: External Pseudonym
parent: User guide
nav_order: 4
permalink: /docs/userguide/extpseudo

---

# External pseudonym

KARNAK will always use a given pseudonym when de-identifying. In this page, it is possible to import your data manually or from a CSV file your existing pseudonyms. So that KARNAK can de-identify your studies while using your pseudonyms.

The pseudonyms are stored for a short period of time. Your data is stored in the cache for maximum 7 days. If KARNAK is restarted, the uploaded data will be lost.

![External pseudonym cache](resources/external_pseudonym_cache.png)

### 1. Choose a project

Each external pseudonym is linked to a project. In this way, there is no risk of collision between different external pseudonyms, if they are two similar pseudonym but patient information is different.

### 2. Upload a CSV file

To upload a CSV file containing external pseudonyms, you must click in the "Upload File" button. 

A pop-up will appear and offer to enter the separator character of the CSV file. In the exemple bellow, we use the "," separator. 

![External pseudonym pop-up](resources/external_pseudonym_popup.png)

After click "Open CSV" button, a grid will be appear with the CSV file content.  

![External pseudonym CSV Dialog](resources/external_pseudonym_csvdialog.png)

### 2.1 From line

This input field allows you to indicate from which line you want to start reading the CSV file. Your CSV file may contain headers. If these headers are in line 1 like the above example (EXTERNALID, PATIENTID, LASTNAME, FIRSTNAME, ISSUER_OF_PATIENTID), you can specify the entry "From line" to the value 2. 

![External pseudonym CSV Dialog 2](resources/external_pseudonym_csvdialog2.gif)

### 2.2 Columns assignements 

Fill in the correct fields with the corresponding columns. All the fields are mandatory, excepted the Issuer of patient ID.

![External pseudonym CSV Dialog 3](resources/external_pseudonym_csvdialog3.gif)

### 2.3 Upload CSV

Click to the "Upload CSV" button to validate the fields with the correct columns to store the externals pseudonyms in KARNAK.

### 3. Add a new patient

It's also possible to add the external pseudonym manually. All the fields are mandatory, excepted the Issuer of patient ID.

### 4. Pseudonym kept temporarily

As explained in the introduction, the pseudonyms are stored for a short period of time in KARNAK. It's possible to edit the patient fields by clicking "Edit"button  or delete a patient by clicking "Delete" button.

![External pseudonym CSV Dialog 2](resources/external_pseudonym_csvdialog4.gif)