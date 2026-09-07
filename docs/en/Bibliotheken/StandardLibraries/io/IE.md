# IE

## 🎧 Podcast

- [4diac IDE: Your "Hello World" of Automation – The Blinking Tutorial Local](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/4diac-IDE-Dein-Hello-World-der-Automatisierung--Das-Blinking-Tutorial-Lokal-e36971r)
- [4diac IDE: Your Open-Source Toolkit for Distributed Industrial Automation according to IEC 61499](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/4diac-IDE-Dein-Open-Source-Werkzeugkasten-fr-verteilte-Industrieautomatisierung-nach-IEC-61499-e36821e)
- [4diac IDE: How the IEC 61499 Standard is Revolutionizing Industrial Automation](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/4diac-IDE-Wie-der-IEC-61499-Standard-die-Industrieautomatisierung-revolutioniert-e36756a)
- [4diac Presentation: Target Groups, Structure, and Unique Selling Proposition Sharpening](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/4diac-Prsentation-Zielgruppen--Struktur-und-Alleinstellungsmerkmal-Schrfen-e38ckbo)
- [Taming the Software Dragon: Industrial Automation and the Future of Production](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/Den-Software-Drachen-zhmen-Industrielle-Automatisierung-und-die-Zukunft-der-Produktion-e372eg1)

## 📺 Video

- [The Great Migration](https://www.youtube.com/watch?v=XcBu7y6ch4E)
- [The Art of Soldering (](https://www.youtube.com/watch?v=I6Srdxx6fzU))
- The World of Standardization (](https://www.youtube.com/watch?v=9phDmkJVaGM))
- Soldering Like a Pro (](https://www.youtube.com/watch?v=8ulMWcxaB-c))
- The Secret of the Field (](https://www.youtube.com/watch?v=MmMrEXum4w4))

## Introduction

The IE (Input Event) function block is a service interface function block for processing event input data. It serves as an interface between the control logic and external input devices or signals and enables the initialization, querying, and indication of input events.

![IE](IE.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Service Initialization - Initializes the function block with parameters
- **REQ**: Service Request - Triggers a query of the input state

### **Event Outputs**

- **INITO**: Initialization Acknowledgement - Confirms successful initialization
- **CNF**: Service Acknowledgement - Confirms the processed service request
- **IND**: Resource Indication - Signals incoming events from the hardware

### **Data Inputs**

- **QI** (BOOL): Event Input Qualifier - Enables/disables event processing
- **PARAMS** (STRING): Service Parameters - Configuration parameters for the service

### **Data Outputs**

- **QO** (BOOL): Event Output Qualifier - Status of the Event Processing
- **STATUS** (STRING): Service status - Feedback on the current operating state

### **Adapter**

No adapter interfaces are available.

## Functionality

The IE function block operates as a bidirectional interface for input events. During initialization (INIT), the service parameters are configured. Targeted queries of the input state can be performed via REQ events. Simultaneously, the block can asynchronously generate IND events when input events are spontaneously detected by the hardware.

## Technical Features

- Supports both poll-based (REQ/CNF) and interrupt-based (IND) operating modes
- String-based parameter and status transmission for flexible configuration
- Separate qualifiers for input and output events (QI/QO)
- Robust error handling through STATUS feedback

## State Overview

The function block goes through the following main states:

1. **Not Initialized**: Block waits for an INIT event
2. **Initialized**: Block is ready for operation and can process REQ and IND events
3. **Query Active**: Processing a REQ request
4. **Indication Active**: Processing a spontaneous input event

## Application Scenarios

- Querying digital inputs (e.g., pushbuttons, switches)
- Monitoring sensor signals
- Interface to external input devices
- Event-driven control applications
- Hardware-level I/O management in automation systems

## ⚖️ Comparison with Similar building blocks

Compared to simpler input blocks, IE offers extended functionality:

- Compared to pure I/O blocks: Supports both poll-based and event-based queries
- Compared to static input blocks: Dynamic parameterization at runtime
- Enhanced status feedback for improved error diagnosis

## 🛠️ Related Exercises

- [Exercise_004a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a/)
- [Exercise_004a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2/)
- [Exercise_004a2_2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2_2/)
- [Exercise_004a2_3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2_3/)
- [Exercise_004a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a2_AX/)
- [Uebung_004a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a3/)
- [Uebung_004a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a3_AX/)
- [Uebung_004a4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a4/)
- [Uebung_004a4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a4_AX/)
- [Uebung_004a5](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a5/)
- [Uebung_004a5_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a5_AX/)
- [Uebung_004a6](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a6/)
- [Uebung_004a6_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a6_AX/)
- [Uebung_004a7](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a7/)
- [Uebung_004a7_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a7_AX/)
- [Uebung_004a8](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a8/)
- [Uebung_004a8_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a8_AX/)
- [Uebung_004a9](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a9/)
- [Uebung_004a9_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a9_AX/)
- [Uebung_004a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a_AX/)
- [Uebung_004b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b/)
- [Uebung_004b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b2/)
- [Uebung_004b3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b3/)
- [Uebung_004b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX/)
- [Uebung_004b_AX_ASR](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR/)
- [Uebung_004b_AX_ASR_X](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR_X/)
- [Uebung_004c1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c1/)
- [Uebung_004c1_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c1_AX/)
- [Uebung_004c2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c2/)
- [Uebung_004c2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c2_AX/)
- [Uebung_004c3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c3/)
- [Uebung_004c3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c3_AX/)
- [Uebung_004c4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c4/)
- [Uebung_004c4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c4_AX/)
- [Uebung_004c5](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c5/)
- [Uebung_004c5_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c5_AX/)
- [Uebung_006](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006/)
- [Uebung_006_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006_AX/)
- [Uebung_006a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a/)
- [Uebung_006a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a2/)
- [Uebung_006a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a2_AX/)
- [Uebung_006a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a3/)
- [Uebung_006a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a3_AX/)
- [Uebung_006a4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a4/)
- [Uebung_006a4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a4_AX/)
- [Uebung_006a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a_AX/)
- [Uebung_006b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006b/)
- [Uebung_006b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006b_AX/)
- [Uebung_006d](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006d/)
- [Uebung_006d_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006d_AX/)
- [Uebung_007a1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a1/)
- [Uebung_007a1_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a1_AX/)
- [Uebung_007a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a2/)
- [Uebung_007a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a2_AX/)
- [Uebung_007a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a3/)
- [Uebung_007a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a3_AX/)
- [Uebung_009a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_009a/)
- [Uebung_010b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b2/)
- [Uebung_010b2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b2_AX/)
- [Uebung_010b3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b3/)
- [Uebung_010b3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b3_AX/)
- [Uebung_010b6](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b6/)
- [Uebung_010b6_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b6_AX/)
- [Uebung_010b7](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b7/)
- [Uebung_010b7_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b7_AX/)
- [Uebung_010b8](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b8/)
- [Uebung_010b8_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b8_AX/)
- [Uebung_010b9](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b9/)
- [Uebung_010b9_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b9_AX/)
- [Uebung_010bA](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA/)
- [Uebung_010bA2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA2/)
- [Uebung_010bA2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA2_AX/)
- [Uebung_010bA3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA3/)
- [Uebung_010bA3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA3_AX/)
- [Uebung_010bA4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA4/)
- [Uebung_010bA4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA4_AX/)
- [Uebung_010bA_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA_AX/)
- [Uebung_013](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_013/)
- [Uebung_013_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_013_AX/)
- [Uebung_014](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_014/)
- [Uebung_015](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_015/)
- [Uebung_015a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_015a/)
- [Uebung_016](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_016/)
- [Uebung_016a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_016a/)
- [Uebung_017](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_017/)
- [Uebung_018](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_018/)
- [Uebung_018a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_018a/)
- [Uebung_019](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019/)
- [Uebung_019a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019a/)
- [Uebung_019b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019b/)
- [Uebung_019c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019c/)
- [Uebung_020f3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f3/)
- [Uebung_020f3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f3_AX/)
- [Uebung_020h](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020h/)
- [Uebung_020h_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020h_AX/)
- [Uebung_020i](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020i/)
- [Uebung_020i_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)
- [Uebung_021](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_021/)
- [Uebung_022](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_022/)
- [Uebung_023](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_023/)
- [Uebung_024](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_024/)
- [Uebung_025](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_025/)
- [Uebung_026](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_026/)
- [Uebung_031](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_031/)
- [Uebung_034b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034b/)
- [Uebung_035](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035/)
- [Uebung_035a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a/)
- [Uebung_035a1_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1_AX/)
- [Uebung_035a1b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1b_AX/)
- [Uebung_035a2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a2/)
- [Uebung_035a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a2_AX/)
- [Uebung_035a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a3/)
- [Uebung_035a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a3_AX/)
- [Uebung_035b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035b/)
- [Uebung_035c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035c/)
- [Uebung_036](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_036/)
- [Uebung_037](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_037/)
- [Uebung_038](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_038/)
- [Uebung_038_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_038_AX/)
- [Uebung_039](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039/)
- [Uebung_039a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a/)
- [Uebung_039a_sub_Outputs](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a_sub_Outputs/)
- [Uebung_040](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040/)
- [Uebung_040_2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040_2/)
- [Uebung_040_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_040_AX/)
- [Uebung_041](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_041/)
- [Uebung_042](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_042/)
- [Uebung_043](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_043/)
- [Uebung_080](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080/)
- [Uebung_080b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080b/)
- [Uebung_080c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080c/)
- [Uebung_081](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_081/)
- [Uebung_082](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_082/)
- [Uebung_083](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_083/)
- [Uebung_083_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_083_AX/)
- [Uebung_084](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_084/)
- [Uebung_085](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_085/)
- [Uebung_087](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087/)
- [Uebung_087a1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a1/)
- [Uebung_091](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_091/)
- [Uebung_093](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_093/)
- [Uebung_093b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_093b/)
- [Uebung_094](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094/)
- [Uebung_094a](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094a/)
- [Uebung_094a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094a_AX/)
- [Uebung_095](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_095/)
- [Uebung_095_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_095_AX/)
- [Uebung_110](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_110/)
- [Uebung_111](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_111/)
- [Uebung_124](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_124/)
- [Uebung_127](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_127/)
- [Uebung_128](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128/)
- [Uebung_128b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128b/)
- [Uebung_132](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_132/)
- [Uebung_150_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_150_AX/)
- [Uebung_151_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_151_AX/)
- [Uebung_152](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_152/)
- [Exercise_153](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_153/)
- [Exercise_160b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160b/)
- [Exercise_160b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160b_AX/)
- [Exercise_171_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_171_AX/)
- [Exercise_179](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_179/)
- [Exercise_180](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_180/)

## Conclusion

The IE function block provides a flexible and robust solution for handling input events in 4diac-based control systems. Its ability to process both synchronous queries and asynchronous indications makes it particularly suitable for applications requiring both reactive and proactive input processing.

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)

]
