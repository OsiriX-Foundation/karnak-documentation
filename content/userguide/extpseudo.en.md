---
title: External Pseudonym
weight: 40
description: Imports pseudonyms from a CSV file or manually
---

In this page, pseudonyms can be created or imported, and will then be used during de-identification by Karnak. The de-identification process and how the pseudonym is used is detailed in the [Pseudonym chapter](../../profiles/rules/#pseudonym).

The activation of de-identification is done in the [Destination configuration](../gateway/destinations/#6-de-identification).

The created or imported pseudonyms in this page are stored in a cache that will be cleared after a maximum of 7 days. The pseudonyms saved will also be lost in case Karnak is restarted.

![External pseudonym cache](/userguide/external_pseudonym_cache.png)

##### 1. Choose a project

External pseudonyms are linked to a project. It allows Karnak to handle properly the case where a patient is participating in multiple clinical studies, as well as potential collisions.

##### 2. Upload a CSV file

A CSV file containing external pseudonyms can be uploaded by clicking the "Upload File" button or drag and dropped there. It launches the import process.

A pop-up is displayed asking for the separator of the CSV file. By default, the value is set to ",". 

![External pseudonym pop-up](/userguide/external_pseudonym_popup.png)

After clicking on the "Open CSV" button, a grid is displayed containing the CSV file data.  

![External pseudonym CSV Dialog](/userguide/external_pseudonym_csvdialog.png)

###### 2.1 From line

This field defines the index of the first line of the CSV file to import. It allows to skip a number of lines at the beginning of the file, especially if it contains headers. 

![External pseudonym CSV Dialog 2](/userguide/external_pseudonym_csvdialog2.gif)

###### 2.2 Columns assignments 

In this view, the CSV columns are associated with the corresponding pseudonym attributes. All the fields are mandatory, except the Issuer of patient ID.

![External pseudonym CSV Dialog 3](/userguide/external_pseudonym_csvdialog3.gif)

###### 2.3 Upload CSV

The "Upload CSV" button will start importing the CSV data. It performs data validation on the data, such as Patient ID or Pseudonym duplication.

##### 3. Add a new patient

It's also possible to add an external pseudonym manually. All the fields are mandatory, except the Issuer of patient ID.

Clicking on "Add patient" will add the entered data into the external pseudonyms table.

##### 4. Delete all patients

This button will delete all the external pseudonyms stored in the cache but linked to the selected project. The other pseudonyms linked to other projects won't be impacted.

A popup will be displayed before the deletion is effectively done, asking for the confirmation of the user.

##### 5. Pseudonym edition

Once stored in the cache, it is possible to edit the patient fields by clicking on the "Edit" button on the external pseudonym row that should be modified. The external pseudonym can also be deleted by clicking in the "Delete" button on the external pseudonym row that should be removed.

Multiple rows can be selected by checking the boxes on the left of each row. These rows can be deleted by clicking on the "Delete selected patients" at the top of the external pseudonyms table. Again, a confirmation is asked before the deletion is performed. 

![External pseudonym CSV Dialog 2](/userguide/external_pseudonym_csvdialog4.gif)