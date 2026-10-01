# F_DT_TO_DATE

* * * * * * * * * *
The function block `F_DT_TO_DATE` converts a date-time value (DT) into a date (DATE). This functionality is particularly useful when control applications only require the date without time information.
![F_DT_TO_DATE](F_DT_TO_DATE.svg)

- `REQ`: Starts the conversion process. Connected to the data input `IN`.
- `CNF`: Signals the completion of the conversion. Connected to the data output `OUT`.
- `IN` (DT): The input for the date-time value to be converted.
- `OUT` (DATE): The output for the converted date.

## Data Outputs

### Data Inputs

### Event Outputs

### Event Inputs

## Interface Structure

## Introduction

### **Adapters**

This function block does not use any adapters.

Upon receiving a `REQ` event, the DT value at input `IN` is converted to a DATE value and output at `OUT`. Simultaneously, the `CNF` event is triggered to signal the completion of the operation.

- The function block implements the conversion using the function `DT_TO_DATE`.
- The conversion is atomic and occurs in a single execution step.

The function block has no internal states. Each `REQ` request is processed independently.

- Extracting the date from timestamps in logging systems
- Preprocessing date information for reports or visualizations
- Interfaces to systems that can only process DATE data types
- Compared to `F_DT_TO_TIME`, this function block only converts the date portion
- Similar to `F_DT_TO_TOD`, but focusing on the date instead of the time of day

The `F_DT_TO_DATE` function block offers a simple and efficient way to convert DT values into pure dates. Its lean implementation makes it particularly suitable for real-time applications where performance is critical.

## Functionality

## Technical Features

## State Overview

## Application Scenarios

## ⚖️ Vergleich mit ähnlichen Bausteinen

## Conclusion
