# AR_MIN

## Introduction

The function block `AR_MIN` is used to determine the minimum value of two analog input signals transmitted via adapters (`AR`). It compares the data values of the two input sockets `IN0` and `IN1` and passes the smaller value to the output plug `OUT`.

By consistently using adapters instead of traditional discrete data and event pins, the complexity of the wiring in the higher-level IEC 61499 application diagram is significantly reduced.

## Interface Structure

Since this function block relies entirely on adapter-based communication, it has no direct, traditional event or data interfaces at the top level. All communication is handled via the declared adapters.

### **Event Inputs**

*No direct event inputs available (events are received via the adapter interfaces).*

### **Event Outputs**

*No direct event outputs available (events are sent via the adapter interfaces).*

### **Data Inputs**

*No direct data inputs available.*

### **Data Outputs**

*No direct data outputs available.*

### **Adapters**

#### **Sockets (Input Interfaces)**

- **IN0** (Type: `adapter::types::unidirectional::AR`):
  The first analog input signal adapter.
- **IN1** (Type: `adapter::types::unidirectional::AR`):
  The second analog input signal adapter.

#### **Plugs (Output Interfaces)**

- **OUT** (Type: `adapter::types::unidirectional::AR`):
  The output adapter. It provides the minimum value $\min(\text{IN0.D1}, \text{IN1.D1})$ including the corresponding update event.

---

## Functionality

As soon as an event `E1` arrives at either input adapter (`IN0` or `IN1`), the function block compares the current data values `IN0.D1` and `IN1.D1`:

$$\text{OUT.D1} = \min(\text{IN0.D1}, \text{IN1.D1})$$

The computed minimum value is set on `OUT.D1` and emitted along with output event `OUT.E1`.

## Technical Features

- **Unidirectional Adapter Structure**: Uses standardized `AR` adapter channels for clear signal direction.
- **Event-Driven**: Recalculation and output occur immediately upon arrival of a new measurement at `IN0` or `IN1`.
- **Type Consistency**: Operates internally using the floating-point data type `REAL`.

## Conclusion

The `AR_MIN` is a useful selection block for calculating extreme values (minimum selection) in adapter-based IEC 61499 applications.
