# SPLIT_LWORD_INTO_WORDS

![SPLIT_LWORD_INTO_WORDS](./SPLIT_LWORD_INTO_WORDS.svg)

* * * * * * * * * *

## Introduction

The function `SPLIT_LWORD_INTO_WORDS` extracts the four `WORD` values from a `LWORD`. It is useful when a 64-bit value (e.g. from a communication protocol) has to be split into 16-bit parts.

## Interface Structure

### **Event Inputs**

- **REQ**: starts the processing (associated with `IN`)

### **Event Outputs**

- **CNF**: signals completion (associated with `WORD_00`..`WORD_03`)

### **Data Inputs**

- **IN** (LWORD): 64-bit value to be split

### **Data Outputs**

- **WORD_00** (WORD): first word, bits 0-15 (`IN.%W0`)
- **WORD_01** (WORD): second word, bits 16-31 (`IN.%W1`)
- **WORD_02** (WORD): third word, bits 32-47 (`IN.%W2`)
- **WORD_03** (WORD): fourth word, bits 48-63 (`IN.%W3`)

### **Adapters**

No adapters are used.

## Functionality

The function assigns the four words by partial access: `WORD_00 := IN.%W0;`, `WORD_01 := IN.%W1;`, `WORD_02 := IN.%W2;`, `WORD_03 := IN.%W3;`.

## Technical Details

- Pure bit accesses without calculation effort; no states
- Package `eclipse4diac::utils::splitting`, version 1.0 (2026-05-16)

## Application Scenarios

- Splitting 64-bit values (e.g. from OPC-UA or CAN data) into 16-bit registers

## ⚖️ Comparison with Similar Blocks

Analogous blocks split smaller values, such as `SPLIT_BYTE_INTO_QUARTERS` (byte into four quarter bytes) or `SPLIT_DWORD_INTO_BOOLS`.

## Conclusion

`SPLIT_LWORD_INTO_WORDS` splits a `LWORD` into four `WORD` values and delivers them with a single `CNF` event.
