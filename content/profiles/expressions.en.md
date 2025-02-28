---
title: Expressions
weight: 40
description: Definition of the expressions applied to specific tags
---


## Actions on specific tags

This profile element applies an expression to a tag or a group of tags defined by the user. An expression is based on [Spring Expression Language (SpEL)](https://docs.spring.io/spring-framework/reference/core/expressions.html) and returns a value or an action according to a certain condition.
Its codename is `expression.on.tags`.

This profile element requires the following parameters:

* `name`: description of the action applied
* `codename`: `expression.on.tags`
* `arguments`: definition of the expression
* `tags`: list of tags the action should be applied to

This profile can have these optional parameters:

* `condition`: optional, defines a condition to evaluate if this profile element should be applied to this DICOM instance
* `excludedTags`: list of tags that will be ignored by this action

The expression is contained in the `expr` argument. The expression will be executed and can either return an action that will be executed, or a null value that will do nothing and move on the next profile element.

Some custom variables are defined in the context of the expression, they contain information relative to the current attribute the profile element is being applied to:

- `tag` contains the current attribute tag, for example (0010,0010)
- `vr` contains the current attribute VR, for example PN
- `stringValue` contains the current element value, for example 'John^Doe' //TOCHECK

Some constants are also defined to improve readability and reduce errors. 

- `#Tag` is a constant that can be used to retrieve any tag integer value in the DICOM standard. For example, `#Tag.PatientBirthDate` corresponds to (0010,0030).
- `#VR` is a constant that can be used to retrieve any VR value in the DICOM standard. For example, `#VR.LO` corresponds to the Long String type.

Some utility functions are defined to work with the tags:

- `getString(int tag)` returns the value of the given tag in the DICOM instance, returns null if the tag is not present

- `tagIsPresent(int tag)` returns true if the tag is present in the DICOM instance, false otherwise

The actions are defined as functions, they are used to set the action and its parameters for the profile element.

The possible actions are:

- `ReplaceNull()`: Sets to empty the current tag value
- `Replace(String dummyValue)`: Replaces the current tag value
- `Remove()`: Removes the tag
- `Keep()`: Keeps the tag unchanged
- `UID()`: Replaces the current tag value with a newly generated UID and sets the tag's VR to UI
- `Add(int tagToAdd, int vr, String value)`: Adds a tag to the DICOM instance
- `ComputePatientAge()`: Replaces the current tag value with a computed value of the patient's age at the time of the exam

//TOCHECK calls api + add ?

## Examples

If the current tag corresponds to the Patient Name attribute and its value is 'John', its content will be replaced by the value of the Institution Name attribute, otherwise the tag is kept unchanged. This example is applied to all the attributes of the DICOM instance, and returns either a Keep or Replace action for every tag, implying that the following profile elements, if any, will be ignored since all the attributes already had an action applied to them. 

```yaml
  - name: "Expression"
    codename: "expression.on.tags"
    arguments:
      expr: "stringValue == 'John' and tag == #Tag.PatientName? Replace(getString(#Tag.InstitutionName)) : Keep()"
    tags: 
      - "(xxxx,xxxx)"
```

If the current tag value is UNDEFINED, it will be kept as is (and it will implicitly block any further potential modifications applied by other profile elements), otherwise the tag is removed. This expression is applied to 2 tags, (0010,0010) and (0010,0212).

```yaml
  - name: "Expression"
    codename: "expression.on.tags"
    arguments:
      expr: "stringValue == 'UNDEFINED'? Keep() : Remove()"
    tags: 
      - "(0010,0010)" #PatientName
      - "(0010,0212)" #StrainDescription
```

Replacement of the Study Description tag value with a concatenation of the Institution Name and the Station Name of the DICOM instance.

```yaml
- name: "Expression"
  codename: "expression.on.tags"
  arguments:
    expr: "Replace(getString(#Tag.InstitutionName) + '-' + getString(#Tag.StationName))"
  tags: 
    - "(0008,1030)" #StudyDescription
```

Computation of the patient's age at the time of the exam.

```yaml
- name: "Expression"
  codename: "expression.on.tags"
  arguments:
    expr: "ComputePatientAge()"
  tags: 
    - "(0010,1010)" #PatientAge
```