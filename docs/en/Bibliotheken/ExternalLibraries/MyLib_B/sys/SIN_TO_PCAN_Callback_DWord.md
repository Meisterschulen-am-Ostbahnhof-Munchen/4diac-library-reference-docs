# SIN_TO_PCAN_Callback_DWord


![SIN_TO_PCAN_Callback_DWord_network](./SIN_TO_PCAN_Callback_DWord_network.svg)

![SIN_TO_PCAN_Callback_DWord](./SIN_TO_PCAN_Callback_DWord.svg)

* * * * * * * * * *

## Introduction

The **SIN_TO_PCAN_Callback_DWord** subapplication is a diagnostic and debugging oriented function block that generates a sinusoidal signal and transmits it as a CAN message through a PCAN callback adapter. The generated signal value is packed into the CAN data field with full DWORD (32-bit) precision, using a reversed byte order for proper little-endian interpretation on the receiving side. The primary purpose of this block is to provide a real-time, easily plottable signal stream for tools such as PCAN Explorer, allowing engineers to visualize and verify the sine wave directly on the CAN bus.

## Interface Structure

The subapplication exposes a single adapter interface and no direct event or data ports. All interaction with the outside world is performed through this adapter.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Adapter Name | Type                    | Direction | Description                                                      |
|--------------|-------------------------|-----------|------------------------------------------------------------------|
| PLUG1        | `isobus::pgn::tx::Callback` | Plug      | Callback-based CAN transmission interface. Provides the mechanism to send CAN messages to the connected PCAN channel via the callback system. |

## Functionality

The subapplication implements a self-contained signal-generation and CAN-forwarding pipeline. The functional flow is initiated when the callback adapter receives a request from the connected partner (typically the PCAN callback driver). Upon this trigger, the following processing chain is executed:

1. **Trigger generation** – The `CallbackFB` receives the adapter request and forwards it as an event to the sine generator.
2. **Sine wave synthesis** – The `GEN_SIN` function block produces a sinusoidal output value with the configured parameters: a period of 10 seconds, an amplitude of 10.0, an offset of 5.0, and no dead time.
3. **Data type conversion** – The floating-point sine value is converted to a DWORD (32-bit unsigned integer) using the `F_REAL_TO_DWORD` conversion block, preserving the full bit pattern of the REAL value.
4. **Byte array transformation** – The DWORD is converted into an 8-byte array with reversed byte order (`DWORDS_TO_ARR08B`). This ensures that the DWORD is placed in the least significant four bytes in a little-endian (reversed) arrangement, while the upper four bytes are padded with zero.
5. **Message structure assembly** – The byte array is packed into a `CAN_MSG` structured type using `STRUCT_MUX`. The message is configured with priority 7 and a data size field set to 0, which represents a standard CAN frame with an 8-byte data payload.
6. **Transmission** – The assembled CAN message is handed back to the `CallbackFB`, which sends it over the adapter to the PCAN bus.

## Technical Features

- **Full DWORD precision**: The sine value is transported as a raw 32-bit IEEE 754 representation, preserving all decimal places and enabling high-resolution plotting on the receiver side.
- **Reversed byte order**: The DWORD is placed into the first four data bytes in reversed order (byte 3, 2, 1, 0), which matches the little-endian memory layout used by many CAN visualization tools.
- **Parameterizable sine wave**: The generator allows configuration of period (`PT`), amplitude (`AM`), offset (`OS`), and dead time (`DL`), making the block adaptable to various test and debug scenarios.
- **Deterministic event chain**: The functional flow is strictly event-driven, ensuring that each CAN message is only generated when the callback adapter requests a new transmission.
- **Standard CAN message layout**: The output adheres to a common CAN frame structure with priority, data size, and an 8-byte payload, making it compatible with standard CAN analysis tools.

## State Overview

The subapplication is essentially an event-driven pipeline with no internal persistent state beyond the standard function block request/confirmation handshakes. The operational sequence follows a linear state progression:

1. **Idle** – Waiting for a request from the callback adapter (`CallbackFB` idle).
2. **Signal Generation** – On request, `GEN_SIN` is activated and produces the sine output.
3. **Conversion** – The REAL value is converted to DWORD.
4. **Byte Packing** – The DWORD is transformed into the reversed byte array and subsequently wrapped into the CAN message structure.
5. **Transmission** – The completed message is forwarded to the callback adapter for transmission on the PCAN bus, after which the block returns to the idle state.

There are no error or exceptional states because the conversion and packing operations are deterministic and do not depend on external conditions other than the availability of the callback adapter.

## Application Scenarios

- **PCAN Explorer visualization**: The block provides a continuous, high-precision sine stream that can be directly plotted in PCAN Explorer, allowing engineers to monitor signal quality and timing without additional hardware.
- **CAN bus testing and validation**: As a diagnostic block, it can inject a known sinusoidal signal into a CAN network to verify the correct operation of receivers, data logging, or filter configurations.
- **Signal generator for simulation**: When developing or debugging CAN-based control systems, this block serves as a stable and repeatable signal source, replacing manual or external signal generators.
- **End-to-end data integrity checks**: By comparing the received sine values with the expected parameters (period, amplitude, offset), users can confirm that the CAN transmission chain preserves data integrity.

## Comparison with Similar Blocks

| Feature                                   | SIN_TO_PCAN_Callback_DWord               | Direct GEN_SIN to CAN (without conversion) | SIN_TO_PCAN with 16-bit precision                 |
|-------------------------------------------|------------------------------------------|--------------------------------------------|---------------------------------------------------|
| Signal precision                          | Full DWORD (32-bit IEEE 754)             | Not directly possible (type mismatch)      | Reduced to 16-bit, loses precision                |
| Byte ordering handled internally          | Yes (reversed, little-endian)            | Manual or external handling required       | Manual handling required                           |
| Configuration effort                      | Low (parameters preset in block)         | High (multiple conversion steps needed)    | Moderate (additional scaling required)            |
| Suitability for high-resolution plotting  | Excellent                                | Not applicable                              | Limited for values with many decimal places       |
| Dependencies on external functions        | Only the callback adapter                | Requires additional custom conversion FB   | Requires scaling and type conversion FB           |

## Conclusion

The **SIN_TO_PCAN_Callback_DWord** subapplication is a robust and practical solution for generating and transmitting sinusoidal CAN signals with full numerical precision. By integrating the sine generator, REAL-to-DWORD conversion, byte-order reversal, and message assembly into a single encapsulated block, it significantly reduces development effort and improves reliability. Its event-driven design ensures deterministic message generation in response to callback requests, while the parameterized sine configuration offers flexibility across a wide range of diagnostic and testing applications. This block is particularly well suited for engineers working with PCAN Explorer and similar CAN analysis tools who require a dependable, high-resolution signal source.