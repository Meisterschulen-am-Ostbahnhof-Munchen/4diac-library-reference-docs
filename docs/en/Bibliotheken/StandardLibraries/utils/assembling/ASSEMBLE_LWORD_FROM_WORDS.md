# ASSEMBLE_LWORD_FROM_WORDS

![ASSEMBLE_LWORD_FROM_WORDS](ASSEMBLE_LWORD_FROM_WORDS.svg)

* * * * * * * * * *

## Introduction

The function `ASSEMBLE_LWORD_FROM_WORDS` combines four 16-bit words (`WORD`) into one 64-bit long word (`LWORD`).

## Interface Structure

### **Event Inputs**

- `REQ`: Starts the processing. Associated data: `WORD_00`, `WORD_01`, `WORD_02`, `WORD_03`

### **Event Outputs**

- `CNF`: Signals the processing and the output of the combined `LWORD`.

### **Data Inputs**

- `WORD_00`: Word 0 (`WORD`), lowest 16 bits of the result
- `WORD_01`: Word 1 (`WORD`), bits 16 to 31
- `WORD_02`: Word 2 (`WORD`), bits 32 to 47
- `WORD_03`: Word 3 (`WORD`), highest 16 bits (bits 48 to 63)

### **Data Outputs**

- (no name): The combined 64-bit long word (`LWORD`), also the return value of the function

### **Adapters**

This function uses no adapters.

## How it works

The function assigns the four input words to the return value via the partial access `.%W0` to `.%W3`:

```text
ASSEMBLE_LWORD_FROM_WORDS.%W0 := WORD_00;
ASSEMBLE_LWORD_FROM_WORDS.%W1 := WORD_01;
ASSEMBLE_LWORD_FROM_WORDS.%W2 := WORD_02;
ASSEMBLE_LWORD_FROM_WORDS.%W3 := WORD_03;
```

`WORD_00` thus forms the least significant and `WORD_03` the most significant word of the `LWORD`.

## Technical Details

- Implemented in Structured Text (ST) as a `FUNCTION` without internal state
- Part of the package `eclipse4diac::utils::assembling`
- The inverse is [`SPLIT_LWORD_INTO_DWORDS`](../splitting/SPLIT_LWORD_INTO_DWORDS.md) or the respective `SPLIT_*` functions in the package `splitting`

## Application Scenarios

- Assembling a 64-bit value from four 16-bit registers (e.g. fieldbus/Modbus data)
- Creating 64-bit bit patterns from individual words

## Summary

`ASSEMBLE_LWORD_FROM_WORDS` assembles four `WORD` values, starting with the least significant, into an `LWORD`.
