# Q_LockUnlockMask

![Q_LockUnlockMask](https://user-images.githubusercontent.com/116869307/214148004-903a6233-7e3e-43eb-a611-03d82d451bf4.png)

* * * * * * * * * *

## Introduction

The **Q_LockUnlockMask** is a standards-compliant function block for controlling the locking state of masks in virtual terminals, developed under the EPL-2.0 license. Version 1.0 implements the ISO 11783-6 (Part 6 - F.46) specification for VT systems from version 4 onwards.
![Q_LockUnlockMask](Q_LockUnlockMask.svg)

## Interface Structure

### **Event Inputs**

- `INIT`: Initialization Request (with mask object ID `u16MaskId`)
- `REQ`: Lock/Unlock Request (with lock command and timeout)

### **Event Outputs**

- `INITO`: Initialization Acknowledgement
- `CNF`: Operation Acknowledgement

### **Data Inputs**

- `u16MaskId` (UINT): Mask Object ID (provided at `INIT`)
- `u8LockCmd` (USINT): Lock Command (0=Unlock, 1=Lock)
- `u16LockTimeoutMs` (UINT): Timeout in ms (0 = no timeout)

### **Data Outputs**

- `STATUS` (STRING): Operational status message
- `u8OldLockCmd` (USINT): Previous lock state
- `u16OldLockTimeoutMs` (UINT): Previous timeout
- `s16result` (INT): ISO-compliant result code

## Instance Uniqueness

This block requires instance uniqueness regarding **u16MaskId**. Only one instance of `Q_LockUnlockMask` may exist in the entire program for the same mask ID. `u16MaskId` is read at `INIT` — a second instance with the same mask ID is deactivated during `INIT` (STATUS = "This objID is already in use"). See also [Instance Uniqueness](./INSTANCE_UNIQUENESS.md).

## Valid Object IDs

`u16MaskId` addresses the mask to lock/unlock (F.46). Valid are:

**Data Mask (1000–1999)** and **Window Mask / User-Layout Data Mask (34000–34999)**.

ID_NULL (65535) is not a valid command target — the command is answered by the VT with an error code (the mask must match the currently visible mask).

## Functionality

1. **Initialization**:
   - `INIT` with `u16MaskId`
   - `INITO` confirms operational readiness

2. **Mask Locking**:
   - `REQ` with lock command and timeout
   - Controls the screen refresh of the mask
   - `CNF` provides operating status and previous values

3. **Timeout Behavior**:
   - Automatic unlock upon expiration
