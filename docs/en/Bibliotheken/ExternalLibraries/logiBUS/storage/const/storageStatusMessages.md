# storageStatusMessages

![storageStatusMessages](./storageStatusMessages.svg)

* * * * * * * * * *

## Introduction

`storageStatusMessages` is a **GlobalConstants** element from the `logiBUS::storage::const` package. It is not an executable function block, adapter, or subapplication. Instead, it provides a centralized collection of `STRING` constants used by storage-related function blocks to report status and error information.

The constant set is designed for storage modules that work with initialization (INI) and non-volatile storage (NVS) systems. It contains human-readable messages for successful operations, missing or invalid data, initialization state, and common NVS error cases.

## Interface Structure

Because `storageStatusMessages` is a GlobalConstants definition, it does not provide an IEC 61499 event or data interface. It has no inputs, no outputs, and no adapters. The declared constants are globally visible to other elements in the same library or project.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The main purpose of `storageStatusMessages` is to define a single source of truth for storage status strings. Instead of using hard-coded literals in multiple function blocks, storage-related FBs can reference these global constants. This reduces typing errors, simplifies translations or adjustments, and makes the behavior of different storage blocks more consistent.

The following table lists all constants exposed by this element.

| Constant Name | Type | Value |
| --- | --- | --- |
| `OK` | `STRING` | `OK` |
| `NO_CHANGE` | `STRING` | `Value was not stored. Old is new` |
| `INITIALISED` | `STRING` | `initialized` |
| `NOT_INITIALISED` | `STRING` | `Not initialized` |
| `ERR` | `STRING` | `ERROR` |
| `ERR_NVS_NOT_FOUND` | `STRING` | `ESP_ERR_NVS_NOT_FOUND` |
| `ERR_NVS_DEFAULT_SET` | `STRING` | `Default Value was used` |
| `ERR_SECTION_EMPTY` | `STRING` | `SECTION empty` |
| `ERR_KEY_EMPTY` | `STRING` | `KEY empty` |
| `ERR_SECTION_WHITESPACE` | `STRING` | `SECTION contains whitespace` |
| `ERR_KEY_WHITESPACE` | `STRING` | `KEY contains whitespace` |
| `ERR_READ_ONLY` | `STRING` | `Key is read-only` |

The constants cover the following categories:

- General success and error states: `OK`, `ERR`.
- Initialization states: `INITIALISED`, `NOT_INITIALISED`.
- No-operation result: `NO_CHANGE`.
- NVS-related errors: `ERR_NVS_NOT_FOUND`, `ERR_NVS_DEFAULT_SET`.
- Validation errors for storage sections and keys: empty or whitespace.
- Access restriction error: `ERR_READ_ONLY`.

## Technical Features

- Defined as `GLOBALCONSTANTS` in the package `logiBUS::storage::const`.
- Contains 12 `STRING` constants.
- All constants are read-only and cannot be modified at runtime.
- The values are globally accessible, avoiding local duplication of status strings.
- The names follow a consistent prefix style: `ERR_` for error constants.
- The message set is versioned, allowing controlled evolution of the status vocabulary.
- Supports both human-readable logging and comparison in application logic.

## State Overview

`storageStatusMessages` is not an FB and therefore has no state machine. It does not process events, maintain runtime variables, or transition between states. The element has exactly one static condition: all declared constants are defined and available as soon as the containing library is loaded.

## Application Scenarios

This GlobalConstants element is useful wherever storage services need to report their result in a standardized way.

Typical scenarios include:

- Function blocks that access NVS storage and need to return `ERR_NVS_NOT_FOUND` when a stored key does not exist.
- Initialization routines that distinguish between `INITIALISED` and `NOT_INITIALISED`.
- Write operations that detect that the new value is identical to the old one and return `NO_CHANGE`.
- Validation logic that checks section and key names for empty strings or whitespace.
- Error handling for read-only keys using `ERR_READ_ONLY`.
- Logging or HMI displays that show status messages from storage operations.

## Comparison with Similar Blocks

Since `storageStatusMessages` is a passive constant container rather than a function block, it cannot be compared directly to executable FBs. However, it can be compared with common alternatives:

- Compared to hard-coded literals, it provides a central, reusable message catalog and avoids inconsistent wording.
- Compared to local constants inside individual FBs, it makes the same messages available across an entire library or project.
- Compared to integer error codes, it delivers readable text directly, but it does not provide numeric encoding for binary protocols.
- Compared to a function block that generates status strings, it requires no runtime processing and has no execution overhead.

## Conclusion

`storageStatusMessages` is a compact global constant set that standardizes status and error reporting for storage-related function blocks. It is especially suited for INI and NVS handling on platforms such as ESP32, where storage errors need to be communicated consistently. By using these global constants, applications become more readable, maintainable, and robust against inconsistent status strings.
