# Numeric vs. Bitwise: The Conversion Trap in FORTE

* * * * * * * * * *

## Introduction

All the building blocks in this folder (`AB_TO_AR`, `AD_TO_ADI`, `AI_TO_AR`, etc.) and the underlying standard function blocks `iec61131::conversion::F_X_TO_Y` (part of the 4diac IDE standard library, not vendored in this repository) convert a value from one IEC 61131 data type to another. For some of these combinations, this is a **true numeric value conversion**; for others, it is a **pure bit reinterpretation**, where the numeric value is deliberately ignored and the raw bit pattern is used instead. Those who don't know the difference can easily create a silent, hard-to-find bug—see `AD_TO_AR_TODO.md` in the source repository for the specific case that triggered this page.

## The Four Type Categories

IEC 61131-3 distinguishes four relevant categories:

| Category | Types | Meaning |
| -------------------------------- | ---------------------------------------- | ----------------------------------------------- |
| **ANY_BIT** | `BOOL`, `BYTE`, `WORD`, `DWORD`, `LWORD` | pure bit patterns without their own numerical semantics |
| **ANY_INT** (signed) | `SINT`, `INT`, `DINT`, `LINT` | Signed Integers |
| **ANY_INT** (Unsigned) | `USINT`, `UINT`, `UDINT`, `ULINT` | Unsigned Integers |
| **ANY_REAL** | `REAL`, `LREAL` | IEEE 754 Floating-Point Numbers |


The adapter prefixes in this folder correspond to: `AB`=BYTE, `AW`=WORD, `AD`=DWORD, `AL`=LWORD, `AX`=BOOL, `AS`=SINT, `AI`=INT, `ADI`=DINT, `ALI`=LINT, `AUS`=USINT, `AUI`=UINT, `AUDI`=UDINT, `AULI`=ULINT, `AR`=REAL, `ALR`=LREAL.

## The Conversion Matrix

Verified in the FORTE core (`core/include/forte/datatypes/forte_any.h`, `CIEC_ANY::cast<U,T>`, as well as `forte_real.cpp`/`forte_lreal.cpp`, `CIEC_REAL::castRealData`) — the same logic behind every `F_X_TO_Y` block and every adapter wrapper in this folder:

| Source < Destination | → ANY_BIT | → ANY_INT | → ANY_REAL |
| ------------------------ | ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **ANY_BIT** (except BOOL) | Bit copy (structural, no numeric value) | Bit reinterpretation — **value-preserving** if target is the same width or wider; otherwise truncates | ⚠️ **Bit reinterpretation — NO numeric value!** IEEE 754 misinterpretation |
| **BOOL** | Parity/LSB test | numeric (0/1) | numeric (0.0/1.0) — special case, see below |
| **ANY_INT** | stores the bit pattern (expected behavior for a bit string target) | numeric (sign expansion/zero expansion safe, narrowing can truncate) | **numeric** (correct cast) |
| **ANY_REAL** | Bit extraction (intentional, e.g., serialization via `F_REAL_TO_DWORD`) | numeric (rounding, `llrint`) | numeric (rounding up/down precision) |

**The only real pitfall** is therefore the cell **ANY_BIT (except BOOL) → ANY_REAL** (highlighted in red): `BYTE`/`WORD`/`DWORD`/`LWORD` as the source of a conversion to `REAL`/`LREAL`. In this library, this specifically affects two building blocks:

- [`AD_TO_AR`](./AD_AR/AD_TO_AR.md) (DWORD→REAL) — safe replacement: [`AD_TO_AR_NUM`](./AD_AR/AD_TO_AR_NUM.md)

- [`AL_TO_ALR`](./AL_ALR/AL_TO_ALR.md) (LWORD→LREAL) — safe replacement: `AL_TO_AULI` + `AULI_TO_ALR`

(There are no `AB_TO_AR`/`AB_TO_ALR`/`AW_TO_AR`/`AW_TO_ALR` pairs in this library — only width-matched bit↔real combinations were offered as adapters.)

**Why is BOOL→REAL safe even though BOOL belongs to ANY_BIT?** FORTE treats BOOL as an explicit special case when casting to REAL (`case e_BOOL: setTFLOAT(...)` instead of the generic bit string copy) — the only exception in the matrix.

## Why is ANY_BIT→ANY_REAL implemented this way at all?

Not arbitrarily: For bit string destinations (`ANY_BIT`→`ANY_BIT`) and for the return path `ANY_REAL`→`ANY_BIT`, bit reinterpretation is precisely the desired, documented behavior — e.g., For example, to pack the IEEE 754 bit pattern of a REAL into a DWORD for transmission (`F_REAL_TO_DWORD`) and later unpack it again using `AD_TO_AR`. The problem is the **risk of confusion**: the same mechanism is incorrectly used when a raw counter or analog value (not a serialized bit pattern) is actually intended to be numerically transferred to REAL.

## Practical Rule of Thumb

- **Does the value come from a `F_REAL_TO_X`/`F_X_TO_REAL` round trip or a fieldbus/protocol deserialization where bit patterns are explicitly transmitted?** → Bit reinterpretation is correct (`AD_TO_AR`, `AL_TO_ALR`).


- **Is the value a raw counter, analog, or other integer value that should be represented as the same numeric value in REAL?** → Use the numeric variant (`AD_TO_AR_NUM`, or more generally: a two-stage conversion via the appropriate intermediate type `ANY_INT`, e.g., `AD_TO_AUDI` → `AUDI_TO_AR`).

- **All other cells in the matrix** are either uniquely numeric or uniquely structural (bit operation) — without the hidden risk of misinterpretation inherent in the ANY_BIT→ANY_REAL cell. Each component in this folder includes a brief note about this in its "Technical Specifications" section.


## Also applies to the standard conversion blocks

The adapter blocks documented here (`AD_TO_AR` etc.) are thin wrappers around the standard 4diac function blocks `iec61131::conversion::F_X_TO_Y` (e.g., `F_DWORD_TO_REAL`).These standard building blocks are part of the 4diac IDE distribution itself (not vendored in this repository) and are subject to the exact same matrix—`F_DWORD_TO_REAL`, `F_WORD_TO_REAL`, `F_BYTE_TO_LREAL`, etc., are bit reinterpretations, while `F_DINT_TO_REAL`, `F_UDINT_TO_REAL`, etc., are numerically correct.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de ](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
