# FB_AR_RANDOM

![FB_AR_RANDOM](./FB_AR_RANDOM.svg)

* * * * * * * * * *

## Introduction

FB_AR_RANDOM is a composite function block wrapper around `eclipse4diac::utils::FB_RANDOM`. It exposes a random value through a unidirectional AR adapter output, while keeping the external interface minimal. The block provides an initialization handshake via `INIT`/`INITO` and uses the `REQ` event to trigger the underlying random number generator.

## Interface Structure

### **Event Inputs**

- `INIT` (EInit): Initialization request. This event is associated with the `SEED` input and is directly confirmed by `INITO`.
- `REQ` (Event): Normal execution request. This event triggers the internal random number generation and causes the result to be sent through the adapter output. It is also associated with the `SEED` input.

### **Event Outputs**

- `INITO` (EInit): Initialization confirm. This event acknowledges the `INIT` request.

### **Data Inputs**

- `SEED` (UINT): Seed value used by the underlying random number generator. The default value is `0`.

### **Data Outputs**

- None. The generated random value is not provided as a direct data output. It is transmitted through the adapter output instead.

### **Adapters**

- `OUT` (plug, `adapter::types::unidirectional::AR`): Unidirectional AR adapter output. The generated value is sent via the adapter event `E1` with the corresponding data on `D1`.

## Functionality

When an `INIT` event is received, the FB passes this request through and immediately acknowledges it with `INITO`. No random generation is performed during initialization.

When a `REQ` event is received, the `SEED` value is forwarded to the internal `FB_RANDOM` block. The internal block generates a random value and, upon completion, triggers the adapter event `E1` on the `OUT` plug. The generated value is made available on the adapter data port `D1`.

## Technical Features

- Composite FB containing one instance of `eclipse4diac::utils::FB_RANDOM`.
- Unidirectional AR adapter output for decoupled, adapter-based communication.
- `UINT` seed input with a default value of `0`.
- Simple initialization handshake through `INIT` and `INITO`.
- No direct output variables; results are delivered exclusively through the adapter.
- No internal ECC; behavior is defined by the contained FBNetwork.

## State Overview

The FB itself does not define an explicit state machine. Its behavior is event-driven and is determined by the internal FBNetwork. The initialization event is forwarded directly to the initialization confirm output without affecting the random generator. The `REQ` event can be used whenever a new random value is required.

## Application Scenarios

- Integration of a random number generator into IEC 61499 applications through a standard AR adapter interface.
- Use in testbeds, simulations, or control applications that require pseudo-random values.
- Modular system designs where data exchange should be encapsulated in adapters instead of direct data connections.
- Supplying random setpoints, seeds, or scheduling parameters to other function blocks in a decoupled manner.

## Comparison with Similar Blocks

Compared to a plain `FB_RANDOM`, this wrapper hides the `CNF` event and the `VAL` data output and instead provides them through a unidirectional AR adapter. This makes the block more suitable for adapter-based architectures and plug-and-socket connections. It also adds a distinct initialization handshake that a direct random generator does not provide. Unlike blocks with ordinary output variables, `FB_AR_RANDOM` allows the random source to be easily replaced or reused in different contexts.

## Conclusion

`FB_AR_RANDOM` is a compact and reusable wrapper that makes a pseudo-random number generator available through a unidirectional AR adapter. It simplifies integration, introduces a clear initialization flow, and fits well into adapter-oriented IEC 61499 system designs.
