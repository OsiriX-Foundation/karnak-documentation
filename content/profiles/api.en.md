---
title: API Actions
weight: 45
description: Definition of the actions related to API calls
---


## Actions with API calls

This profile element uses a call to an API to replace the value of a tag or a group of tags defined by the user. Its codename is `action.replace.api`.

This profile element requires the following parameters:

* `name`: description of the action applied
* `codename`: `action.replace.api`
* `arguments`: definition of the expression
* `tags`: list of tags the action should be applied to

This profile can have these optional parameters:

* `condition`: optional, defines a condition to evaluate if this profile element should be applied to this DICOM instance
* `excludedTags`: list of tags that will be ignored by this action

The mandatory arguments for this profile are the following:

* `url`: url of the api to query, it can contain parameters evaluated at runtime, see below for an example
* `responsePath`: JSON path of the value used as a replacement, using the [JSON Pointer](https://datatracker.ietf.org/doc/html/rfc6901) syntax. An empty string means that the entire value returned by the call will be used.

The following arguments are optional:

* `method`: GET or POST HTTP method, defaults to GET if not specified
* `body`: optional body for a POST request in the JSON format, it can contain parameters evaluated at runtime, see below for an example
* `authConfig`: identifier of an existing [Authentication Configuration](../../userguide/authconfig) to be used to authenticate the call. If not specified, no authentication will be used for the call.
* `defaultValue`: default value used for replacement in case an error happens during the API call (Unauthorized, value not found in the JSON response, authConfig identifier not found...). If not specified, the transfer will be aborted and a reason will be displayed in the monitoring view, otherwise the default value is used for the replacement does not block the transfer

### URL and Body Arguments

The content of the body and the URL may require a value directly linked to the instance being transferred, like the Patient ID for example.

A specific syntax has been implemented to allow this kind of runtime injection. The parts evaluated at runtime must be enclosed in **double brackets**. The syntax used is the [Spring Expression Language (SpEL)](https://docs.spring.io/spring-framework/reference/core/expressions.html), with functions and constants defined similarly to the [Expressions](../expressions).

```yaml
http://example.com/{{getString(#Tag.PatientID)}}/endpoint
http://example.com/{{getString(#Tag.ClinicalTrialSponsorName)}}/patient/{{getString(#Tag.PatientID)}}/endpoint

{ 
  "patientId": "{{getString(#Tag.PatientID)}}", 
  "project": "{{getString(#Tag.ClinicalTrialSponsorName)}}" 
}
```

In the body, hence the JSON format, the expression must be enclosed in double quotes if the expected value is a String.

## Examples

The profile element below replaces the value of the Patient ID tag with the value retrieved from the url, it extracts the String value from the response at the specified path. Since it is not specified, the HTTP method is GET.

The authConfig argument contains the identifier of the Authentication Configuration used to authenticate the API call.

If an error occurs during the call, this transfer will be aborted. If the defaultValue was specified, it would have been used as the replacement value and the transfer would not have been blocked.

```yaml
  - name: "Replace Patient ID from API call"
    codename: "action.replace.api"
    arguments:
      url: "http://example.com/{{getString(#Tag.PatientID)}}/endpoint"
      responsePath: "/_embedded/0/value"
      authConfig: "endpoint-auth-config"
    tags: 
      - "(0010,0020)"
```
