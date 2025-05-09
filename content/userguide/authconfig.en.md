---
title: Authentication Configuration
weight: 80
description: Authentication Configuration for API calls
---

In this page, the Configuration for Authenticating API calls are defined. This information being sensitive, it is stored in a specific table where the columns are encrypted. 

![authconfig page](/userguide/authconfig_main.png)

##### 1. Create a Authentication Config

After clicking on the Create Authentication Config button, a field asking for the identifier of the new Authentication Configuration will be displayed. This identifier must be unique, otherwise an error will appear if a duplicate exists. We recommend that it does not contain spaces for an easier reference in profiles.

![authconfig create](/userguide/authconfig_create.png)

Clicking on the button Create will display the form. The Authentication Configuration will be added to the list on the left and its details displayed in the right panel.

Currently, the only Authentication Configuration type implemented is OAuth 2.0.

###### OAuth 2.0

For the OAuth 2.0 Configuration, the required fields are the access token URL, the scope (separated by a whitespace if there are multiple scopes), the client secret and the client ID.

![authconfig create form](/userguide/authconfig_createform.png)

This information will be used when an API call is made with a reference to the identifier as the authConfig parameter. The API call will be intercepted, if a valid token is available, it will be added to the request for authentication. Otherwise, a new token will be requested using the Authentication Configuration details to obtain a token and authenticate the API call.  

{{% notice note %}} The Authentication Configuration cannot be modified once created.{{% /notice %}}

##### 2. Authentication Config list

All the Authentication Configurations available are listed here. By selecting an element in the list, its details are displayed on the right.

##### 3. Authentication Config details

In the details view, the Authentication Configuration details are displayed. They cannot be modified. In case a modification is required, the element must be deleted and recreated.

##### 4. Delete Button

The selected configuration can be deleted by clicking on the red bin button next to its identifier. A confirmation message is displayed before the deletion.

![authconfig delete](/userguide/authconfig_delete.png)

This operation may lead to errors in the behavior of the application. There are no checks that are executed to ensure that a configuration is not used. The deleted configuration might be used by a profile or a deidentification process, and will result in errors during the next transfer. Please ensure that the configuration is not used anywhere before deleting it.
