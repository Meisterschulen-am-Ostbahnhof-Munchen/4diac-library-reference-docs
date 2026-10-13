# Callback

![Callback](Callback.svg)

**Plug (Adapter-Ausgang)**

![Callback_plug](./Callback_plug.svg)

**Socket (Adapter-Eingang)**

![Callback_socket](./Callback_socket.svg)

Message Callback

## Interface

### Event Inputs

| Name | Comment             | With |
| :--- | :------------------ | :--- |
| REQ  | Request from Socket |      |

### Event Outputs

| Name | Comment                | With |
| :--- | :--------------------- | :--- |
| CNF  | Confirmation from Plug | DO1  |

### Output Vars

| Name | Type                 | Comment |
| :--- | :------------------- | :------ |
| DO1  | isobus::pgn::CAN_MSG |         |

## 🛠️ Zugehörige Übungen

- [SIN_TO_PCAN_Callback_DWord](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-library-reference-docs/de/latest/Bibliotheken/ExternalLibraries/MyLib_B/sys/SIN_TO_PCAN_Callback_DWord/)
- [SIN_TO_PCAN_Callback_Byte](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-library-reference-docs/de/latest/Bibliotheken/ExternalLibraries/MyLib_B/sys/SIN_TO_PCAN_Callback_Byte/)
- [Uebung_12x_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_12x_sub/)
