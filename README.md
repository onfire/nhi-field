# nhi-field
A Silverstipe module to provide a NHI db field and NHI Form field to track NZ's "National Health Index" number.

[![Build Status](https://travis-ci.org/webtorque/silverstripe-nhi-field.svg?branch=dev)](https://travis-ci.org/webtorque/silverstripe-nhi-field)

## Features
* Custom DB field type
* Custom form field type with basic pattern validation and checksum validation.
* Consistently save NHI in all caps.

## Requirements
* PHP 5.4 or greater (tested with up to PHP 7.1)
* `silverstripe/framework:^3.0`
* `silverstripe/cms:^3.0`

## Installation
```bash
composer require webtorque/silverstripe-nhi-field:^0.0
```

## Usage
To specify a NHI db field on a DataObject:
```php
class Patient extends DataObject {
    private static $db = [
        'NationalHealthIndex' => 'NHI',
    ];
}
```

The `NHI` field type is equivalent to `Varchar(7)`. When scaffolding a form, any NHI db field will autmatically use the
`NHIField` form field instead of `TextField`.

## Validating an NHI
Just use the `NHIField` in your form and validate your Form normally.

```php

$nhiField = NHIField::create(
    $name = 'nhi',                      // required
    $title = 'National Health Index',   // optional
    $value = 'CGC2720',                 // optional
    $form = null,                       // optional
    $html5pattern = false               // optional, output an `html5` pattern attribute
);

```

`NHIField` is a simple extension of `TextField` with the following alteration:
* The `maxlength` is automatically set to 7.
* When setting the value of the field, it will automatically be converted to uppercase.
* The value is validated with a basic regex and with a [checksum as specified by the NHI standard](https://en.wikipedia.org/wiki/NHI_Number#Format).
* You can ouput an html5 `pattern` attribute with `$nhiField->setHtml5Pattern(true);`.

For testing purposes, checksum validation can be disabled by setting the `disable_checksum_validation` flag on the `NHIField` config.
```php
if (Director::isDev()) {
    NHIField::config()->disable_checksum_validation = true;
}
```
