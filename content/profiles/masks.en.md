---
title: Image modifications
weight: 50
description: Definition of the masks applied to Pixel Data
---

This page describes the profile elements available in Karnak for modifying DICOM image data to remove identifying information.

## Overview

Karnak provides two main approaches to protect patient identity in DICOM images:

1. **Defacing**: Automated removal of facial features from CT images
2. **Pixel Data Cleaning**: User-defined masks to remove burned-in annotations and identifying information

Both methods ensure that visual identifying information is removed from images while preserving the diagnostic value of the data.

---

## Defacing

Defacing automatically removes facial features from CT images to protect patient identity.

### Supported Images

This profile can only be applied to images with **Axial orientation** in the following SOP Classes:

* `1.2.840.10008.5.1.4.1.1.2` - CT Image Storage
* `1.2.840.10008.5.1.4.1.1.2.1` - Enhanced CT Image Storage

> [!INFO]
> Images with non-axial orientation will be skipped.
> Currently, the CT images that do not represent the head are not automatically excluded. It is recommended to use [conditions](../conditions) to restrict defacing to head CT images only.

### Configuration

This profile element requires the following parameters:

| Parameter | Description | Required |
|-----------|-------------|----------|
| `name` | Description of the action applied | Yes |
| `codename` | Must be `clean.recognizable.visual.features` | Yes |
| `condition` | Condition to evaluate if this profile element should be applied | No |

## Pixel Data Cleaning

Pixel data cleaning applies user-defined masks to DICOM images to remove identifying information burned into the pixels, such as patient or institution information.

### Condition for Automatic Application

This profile is automatically applied to the following SOP Classes:

* `1.2.840.10008.5.1.4.1.1.6.1` - Ultrasound Image Storage
* `1.2.840.10008.5.1.4.1.1.7.1` - Multiframe Single Bit Secondary Capture Image Storage
* `1.2.840.10008.5.1.4.1.1.7.2` - Multiframe Grayscale Byte Secondary Capture Image Storage
* `1.2.840.10008.5.1.4.1.1.7.3` - Multiframe Grayscale Word Secondary Capture Image Storage
* `1.2.840.10008.5.1.4.1.1.7.4` - Multiframe True Color Secondary Capture Image Storage
* `1.2.840.10008.5.1.4.1.1.3.1` - Ultrasound Multiframe Image Storage
* `1.2.840.10008.5.1.4.1.1.77.1.1` - VL Endoscopic Image Storage

**Or** if the tag **Burned In Annotation (0028,0301)** is set to `"YES"`

### Configuration

This profile element requires the following parameters:

| Parameter | Description | Required |
|-----------|-------------|----------|
| `name` | Description of the action applied | Yes |
| `codename` | Must be `clean.pixel.data` | Yes |
| `condition` | Condition to evaluate if this profile element should be applied | No |

### Using Conditions

The `condition` parameter allows you to apply cleaning selectively. For example, to exclude images from a specific machine that does not add burned-in annotations, you can use the following configuration:

```yaml
profileElements:
  - name: "Clean pixel data"
    codename: "clean.pixel.data"
    condition: "!tagValueContains(#Tag.StationName,'ICT256')"
```

## Masks Definition

Masks define rectangular regions to be filled with a solid color, removing any identifying information in those areas.

### Mask Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| `stationName` | Station name to match against DICOM tag (0008,1010). Use `*` to match any station | Yes |
| `color` | Fill color in hexadecimal format (e.g., `ffff00` for yellow) | Yes |
| `rectangles` | List of rectangles defining the areas to mask | Yes |

### Optional Parameters

| Parameter | Description |
|-----------|-------------|
| `imageWidth` | Apply mask only to images with this width in pixels (matches Columns tag 0028,0011) |
| `imageHeight` | Apply mask only to images with this height in pixels (matches Rows tag 0028,0010) |

> [!INFO]
> When specifying image dimensions, **both** `imageWidth` and `imageHeight` must be set. Defining only one parameter is not supported.

### Rectangle Definition

Each rectangle is defined by four values:

| Parameter | Description |
|-----------|-------------|
| `x` | X coordinate of the upper-left corner |
| `y` | Y coordinate of the upper-left corner |
| `width` | Width of the rectangle in pixels |
| `height` | Height of the rectangle in pixels |

The coordinate system starts at `(0, 0)` in the **upper-left corner** of the image.

The diagram below illustrates a rectangle with parameters `(25, 75, 150, 50)`:

![Rectangle definition example](/profiles/CleanPixel_rectangle.png)

## Mask Selection Process

When processing a DICOM instance, Karnak selects the appropriate mask using the following algorithm:

1. **Extract image attributes**: Retrieve the Rows (0028,0010), Columns (0028,0011), and Station Name (0008,1010) values from the instance
2. **Exact match**: Search for a mask matching all three values (width, height, and station name)
3. **Station match**: If no exact match is found, ignore image dimensions and match only on station name
4. **Default mask**: If no station match is found, use the mask with `stationName: "*"`

### Example Configuration


```yaml
masks:
  - stationName: "*"
    color: "ffff00"
    rectangles:
      - "25 75 150 50"
  - stationName: "R2D2"
    color: "00ff00"
    rectangles:
      - "25 25 150 50"
      - "350 15 150 50"
  - stationName: "R2D2"
    imageWidth: 1024
    imageHeight: 1024
    color: "00ffff"
    rectangles:
      - "50 25 100 100"
```

## Complete Example: Equipment-Specific Cleaning

This example demonstrates a profile that handles specific pixel data cleaning for equipment that burns patient information into the images and does not match the conditions for automatic application.

This profile below performs the following actions for images from a machine with Station Name containing "ICT256":

1. Ensures the Burned In Annotation tag is present and set to "YES"
2. Applies pixel data cleaning with a station-specific mask
3. Applies the basic DICOM de-identification profile

```yaml
name: "Clean pixel data"
version: "1.0"
minimumKarnakVersion: "0.9.2"
defaultIssuerOfPatientID:
profileElements:
  - name: "Add tag BurnedInAnnotation if does not exist"
    codename: "action.add.tag"
    condition: "tagValueContains(#Tag.StationName, 'ICT256') && !tagIsPresent(#Tag.BurnedInAnnotation)"
    arguments:
      value: "YES"
      vr: "CS"
    tags:
      - "(0028,0301)"

  - name: "Set BurnedInAnnotation to YES"
    codename: "expression.on.tags"
    condition: "tagValueContains(#Tag.StationName, 'ICT256')"
    arguments:
      expr: "Replace('YES')"
    tags:
      - "(0028,0301)"

  - name: "Clean pixel data"
    codename: "clean.pixel.data"

  - name: "DICOM basic profile"
    codename: "basic.dicom.profile"

masks:
  - stationName: "*"
    color: "ffff00"
    rectangles:
      - "25 75 150 50"
  - stationName: "ICT256"
    color: "00ff00"
    rectangles:
      - "25 25 150 50"
      - "350 15 150 50"
```
