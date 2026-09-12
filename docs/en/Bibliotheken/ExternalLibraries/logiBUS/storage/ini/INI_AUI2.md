# INI_AUI2

![INI_AUI2](./INI_AUI2.svg)

* * * * * * * * * *

## Introduction

The INI_AUI2 function block is used to read and write UINT data from a settings.ini file via an AUI2 adapter. It combines the INI block with a bidirectional AUI2 adapter to read values from or write them back to a configuration file. Initialization is performed via the INIT event, where the section and key are specified. The read value is offered via the adapter, and a write operation is triggered via the adapter event.

## Interface Structure

### **Event Inputs**

| Name | Description |
| ---- | ----------- |
| INIT | Initialization event to start the read operation. Expects the parameters QI, SECTION, KEY, and DEFAULT_VALUE. |

### **Event Outputs**

| Name | Description |
| ---- | ----------- |
| INITO | Initialization confirmation. Sent after the read operation is complete. Status information is available via QO and STATUS. |

### **Data Inputs**

| Name | Type | Description |
| ---- | ---- | ----------- |
| QI | BOOL | Input qualifier for controlling processing. |
| SETM | BOOL | Enables mirroring/confirmation of the write value after a successful write. |
| SECTION | STRING | Name of the section in the settings.ini file. |
| KEY | STRING | Name of the key within the section. |
| DEFAULT_VALUE | UINT | Default value read if the key is not present in the file. Default: 0. |

### **Data Outputs**

| Name | Type | Description |
| ---- | ---- | ----------- |
| QO | BOOL | Output qualifier, indicates successful processing. |
| STATUS | STRING | Service status message (e.g., error messages). |

### **Adapters**

| Name | Type | Description |
| ---- | ---- | ----------- |
| VAL | adapter::types::bidirectional::AUI2 | Bidirectional adapter for exchanging values. This adapter provides the read value as an output (DI1) and receives a write value as an input (DO1). |

## Functionality

The INI_AUI2 function block contains an internal INI function block (eclipse4diac::storage::INI) that performs the actual file operation. The network connections implement the following processes:

- Upon arrival of INIT, QI, SECTION, KEY, and DEFAULT_VALUE are forwarded to the INI function block.
- The INI function block performs a read operation and passes the result (via VALUEO) to the adapter output (VAL.DI1).
- Simultaneously, after a successful read, the GET event is triggered, which activates the adapter input (VAL.EI1) to transmit the value.
- A write operation is initiated as soon as the adapter sends an EO1 event. This event triggers the SET event on the INI function block, setting the value received via VAL.DO1 as the new VALUE.
- The INI block confirms the write operation with SETO and sends this back to the adapter (VAL.EI1).
- The QO and STATUS outputs are directly taken from the INI block.

Thus, read and write access to the settings.ini file can be controlled via the AUI2 adapter.

## Technical Features

- The block uses a bidirectional AUI2 adapter that can both send and receive values.
- A UINT value is used as the default value.
