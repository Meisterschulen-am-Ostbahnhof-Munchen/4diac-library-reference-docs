# IG2_NAME_Functions

![IG2_NAME_Functions](./IG2_NAME_Functions.svg)

* * * * * * * * * *
## Introduction
The `IG2_NAME_Functions` global constant set defines Industry Group 2 (Agricultural Equipment) specific function codes used in ISO 11783 (ISOBUS) networks. These constants represent the `function` part of the NAME (Name field) of a control function, as specified in ISO 11783-7. The values are used to identify the role of a device or software component on the bus, such as "Spray Rate Control", "Tillage Depth Control", or "Product Flow". This constant set covers a wide range of agricultural applications, from tractors and tillage implements to harvesters, sensor systems, and specialised devices.

The constants are grouped by device class (e.g., Tractor, Tillage, Sprayers, Harvesters) and provide a consistent, human‑readable naming scheme for developers working with ISOBUS protocols. Each constant is of type `BYTE` and has a predefined default value that is unique within its device‑class context.

## Interface Structure
### **Event Inputs**
None – This is a global constant set and does not contain any event inputs.

### **Event Outputs**
None – This is a global constant set and does not contain any event outputs.

### **Data Inputs**
None – This is a global constant set and does not contain any data inputs.

### **Data Outputs**
The set provides a total of **85** constants, each representing a specific function code. They are listed in the following table (grouped by device class, as in the XML).

| Constant Name | Value | Short Description (excerpt) |
|---------------|-------|-----------------------------|
| **Non-specific System** | | |
| `F_NON_VIRTUAL_TERMINAL_DISPLAY` | 128 | An operator display that cannot perform as a Virtual Terminal |
| `F_OPERATOR_CONTROLS_MACHINE_SPECIFIC` | 129 | Operator interface controls (auxiliary or proprietary) |
| `F_TASK_CONTROLLER_MAPPING_COMPUTER` | 130 | Task Controller / Mapping Computer |
| `F_POSITION_CONTROL` | 131 | Multiple axis position control of application boom |
| `F_MACHINE_CONTROL` | 132 | General machine control (outputs, ancillary functions) |
| `F_FOREIGN_OBJECT_DETECTION` | 133 | Detection of undesired objects in product flow |
| `F_TRACTOR_ECU` | 134 | Tractor interface unit on the implement bus |
| `F_SEQUENCE_CONTROL_MASTER` | 135 | Master controller in ISO11783‑14 sequence control |
| `F_PRODUCT_DOSING` | 136 | Adds active ingredient to liquid carrier |
| `F_PRODUCT_TREATMENT` | 137 | Mixes treatment to dry product |
| `F_RESERVED` | 138 | Reserved for future “all operations stop” function |
| `F_DATA_LOGGER` | 139 | Non‑task related data logger |
| `F_DECISION_SUPPORT` | 140 | Configures operation for optimal performance |
| `F_LIGHTING_CONTROLLER` | 141 | Controls electrical lights and status |
| `F_TIM_SERVER` | 142 | Tractor Implement Management (TIM) Server |
| `F_NOT_AVAILABLE` | 255 | Not available / unassigned |
| **Tractor** | | |
| `F_TRACTOR_AUXILIARY_VALVE_CONTROL` | 129 | Control of tractor‑mounted auxiliary valves |
| `F_TRACTOR_REAR_HITCH_CONTROL` | 130 | Rear hitch control |
| `F_TRACTOR_FRONT_HITCH_CONTROL` | 131 | Front hitch control |
| `F_TRACTOR_TRACTOR_MACHINE_CONTROL` | 132 | Machine control (outputs, ancillary functions) |
| `F_TRACTOR_CENTER_HITCH_CONTROL` | 134 | Center hitch control |
| `F_TRACTOR_NOT_AVAILABLE` | 255 | Not available |
| **Tillage** | | |
| `F_TILLAGE_TILLAGE_MACHINE_CONTROL` | 132 | Control of outputs, ancillary functions |
| `F_TILLAGE_TILLAGE_DEPTH_CONTROL` | 135 | Depth of tillage tool in soil |
| `F_TILLAGE_FRAME_CONTROL` | 136 | Folding/unfolding of frame (transport vs field) |
| `F_TILLAGE_NOT_AVAILABLE` | 255 | Not available |
| **Secondary Tillage** | | |
| `F_SECONDARY_TILLAGE_SECONDARY_TILLAGE_MACHINE_CONTROL` | 132 | Machine control |
| `F_SECONDARY_TILLAGE_SECONDARY_TILLAGE_DEPTH_CONTROL` | 135 | Depth control for surface preparation |
| `F_SECONDARY_TILLAGE_FRAME_CONTROL` | 136 | Frame control |
| `F_SECONDARY_TILLAGE_NOT_AVAILABLE` | 255 | Not available |
| **Planters/Seeders** | | |
| `F_PLANTERS_SEEDERS_SEED_RATE_CONTROL` | 128 | Rate of seed placed in soil |
| `F_PLANTERS_SEEDERS_SECTION_ON_OFF_CONTROL` | 129 | On/Off control of individual sections |
| `F_PLANTERS_SEEDERS_POSITION_CONTROL` | 131 | Multiple axis position control (row guidance) |
| `F_PLANTERS_SEEDERS_PLANTERS_SEEDERS_MACHINE_CONTROL` | 132 | Machine control |
| `F_PLANTERS_SEEDERS_PRODUCT_FLOW` | 133 | Product flow measurement |
| `F_PLANTERS_SEEDERS_PRODUCT_LEVEL` | 134 | Product level in bin |
| `F_PLANTERS_SEEDERS_DEPTH_CONTROL` | 135 | Depth of seed/tuber placement |
| `F_PLANTERS_SEEDERS_FRAME_CONTROL` | 136 | Frame control |
| `F_PLANTERS_SEEDERS_DOWN_PRESSURE` | 137 | Ground contact pressure on delivery unit |
| `F_PLANTERS_SEEDERS_NOT_AVAILABLE` | 255 | Not available |
| **Fertilizers** | | |
| `F_FERTILIZERS_FERTILIZE_RATE_CONTROL` | 128 | Fertilizer rate control |
| `F_FERTILIZERS_SECTION_ON_OFF_CONTROL` | 129 | Section on/off |
| `F_FERTILIZERS_PRODUCT_PRESSURE` | 130 | Monitoring pressure in sprayer booms |
| `F_FERTILIZERS_POSITION_CONTROL` | 131 | Boom position control |
| `F_FERTILIZERS_FERTILIZERS_MACHINE_CONTROL` | 132 | Machine control |
| `F_FERTILIZERS_PRODUCT_FLOW` | 133 | Product flow monitoring |
| `F_FERTILIZERS_PRODUCT_LEVEL` | 134 | Product level in bin |
| `F_FERTILIZERS_HEIGHT_DEPTH_CONTROL` | 135 | Height/depth of boom placement |
| `F_FERTILIZERS_FRAME_CONTROL` | 136 | Frame control |
| `F_FERTILIZERS_NOT_AVAILABLE` | 255 | Not available |
| **Sprayers** | | |
| `F_SPRAYERS_SPRAY_RATE_CONTROL` | 128 | Rate of crop protection product applied |
| `F_SPRAYERS_SECTION_ON_OFF_CONTROL` | 129 | Section on/off |
| `F_SPRAYERS_PRODUCT_PRESSURE` | 130 | Pressure in delivery booms |
| `F_SPRAYERS_POSITION_CONTROL` | 131 | Boom position control |
| `F_SPRAYERS_SPRAYERS_MACHINE_CONTROL` | 132 | Machine control |
| `F_SPRAYERS_PRODUCT_FLOW` | 133 | Product flow |
| `F_SPRAYERS_PRODUCT_LEVEL` | 134 | Product level in tank |
| `F_SPRAYERS_BOOM_HEIGHT_CONTROL` | 135 | Boom height above soil/crop |
| `F_SPRAYERS_FRAME_CONTROL` | 136 | Frame control |
| `F_SPRAYERS_NOT_AVAILABLE` | 255 | Not available |
| **Harvesters** | | |
| `F_HARVESTERS_TAILING_MONITOR` | 128 | Monitor quantity of unthreshed material |
| `F_HARVESTERS_HEADER_CONTROL` | 129 | Control of header reel height/rotation |
| `F_HARVESTERS_PRODUCT_LOSS_MONITOR` | 130 | Monitor grain loss to soil |
| `F_HARVESTERS_PRODUCT_MOISTURE` | 131 | Monitor moisture content of grain |
| `F_HARVESTERS_HARVESTER_MACHINE_CONTROL` | 132 | Machine control |
| `F_HARVESTERS_PRODUCT_FLOW` | 133 | Yield rate (grain flow into tank) |
| `F_HARVESTERS_PRODUCT_LEVEL` | 134 | Grain level in tank |
| `F_HARVESTERS_HEADER_HEIGHT_CONTROL` | 135 | Header height above soil / below crop top |
| `F_HARVESTERS_NOT_AVAILABLE` | 255 | Not available |
| **Root Harvesters** | | |
| `F_ROOT_HARVESTERS_ROOT_HARVESTERS_MACHINE_CONTROL` | 132 | Machine control |
| `F_ROOT_HARVESTERS_PRODUCT_FLOW` | 133 | Product flow into tank |
| `F_ROOT_HARVESTERS_PRODUCT_LEVEL` | 134 | Product level in tank |
| `F_ROOT_HARVESTERS_DEPTH_CONTROL` | 135 | Lifter share depth |
| `F_ROOT_HARVESTERS_NOT_AVAILABLE` | 255 | Not available |
| **Forage** | | |
| `F_FORAGE_TWINE_WRAPPER_CONTROL` | 128 | Twine wrapping around bale |
| `F_FORAGE_PRODUCT_PACKAGING_CONTROL` | 129 | Packaging process control |
| `F_FORAGE_PRODUCT_MOISTURE` | 131 | Moisture content of forage |
| `F_FORAGE_FORAGE_MACHINE_CONTROL` | 132 | Machine control |
| `F_FORAGE_PRODUCT_FLOW` | 133 | Forage yield rate |
| `F_FORAGE_WORKING_HEIGHT_CONTROL` | 135 | Cutting header height |
| `F_FORAGE_NOT_AVAILABLE` | 255 | Not available |
| **Irrigation** | | |
| `F_IRRIGATION_NOT_AVAILABLE` | 255 | Not available |
| **Transport/Trailer** | | |
| `F_TRANSPORT_TRAILER_TRANSPORT_MACHINE_CONTROL` | 132 | Machine control |
| `F_TRANSPORT_TRAILER_UNLOAD_CONTROL` | 136 | Trailer unloading process control |
| `F_TRANSPORT_TRAILER_NOT_AVAILABLE` | 255 | Not available |
| **Farm Yard Operations** | | |
| `F_FARM_YARD_OPERATIONS_NOT_AVAILABLE` | 255 | Not available |
| **Powered Auxiliary Devices** | | |
| `F_POWERED_AUXILIARY_DEVICES_POWERED_DEVICES_MACHINE_CONTROL` | 132 | Machine control |
| `F_POWERED_AUXILIARY_DEVICES_NOT_AVAILABLE` | 255 | Not available |
| **Special Crops** | | |
| `F_SPECIAL_CROPS_SPECIAL_CROP_MACHINE_CONTROL` | 132 | Machine control |
| `F_SPECIAL_CROPS_NOT_AVAILABLE` | 255 | Not available |
| **Earth Work** | | |
| `F_EARTH_WORK_MATERIAL_RATE_CONTROL` | 128 | Control of material processing rate |
| `F_EARTH_WORK_EARTHWORKS_MACHINE_CONTROL` | 132 | Machine control |
| `F_EARTH_WORK_MATERIAL_FLOW` | 133 | Material flow monitoring |
| `F_EARTH_WORK_MATERIAL_LEVEL` | 134 | Material level monitoring |
| `F_EARTH_WORK_DEPTH_CONTROL` | 135 | Working depth control |
| `F_EARTH_WORK_NOT_AVAILABLE` | 255 | Not available |
| **Skidder** | | |
| `F_SKIDDER_SKIDDER_MACHINE_CONTROL` | 132 | Machine control |
| `F_SKIDDER_NOT_AVAILABLE` | 255 | Not available |
| **Sensor Systems** | | |
| `F_SENSOR_SYSTEMS_GUIDANCE_FEELER` | 128 | Mechanical row position sensing |
| `F_SENSOR_SYSTEMS_CAMERA_SYSTEM` | 129 | Camera system for control operations |
| `F_SENSOR_SYSTEMS_CROP_SCOUTING` | 130 | Vegetation parameter measurement |
| `F_SENSOR_SYSTEMS_MATERIAL_PROPERTIES_SENSING` | 131 | Material properties detection (density, particle size, …) |
| `F_SENSOR_SYSTEMS_INERTIAL_MEASUREMENT_UNIT_IMU` | 132 | Inertial measurement unit |
| `F_SENSOR_SYSTEMS_PRODUCT_FLOW` | 133 | Product flow monitoring |
| `F_SENSOR_SYSTEMS_PRODUCT_LEVEL` | 134 | Product level in tank/bin |
| `F_SENSOR_SYSTEMS_PRODUCT_MASS` | 135 | Product mass monitoring |
| `F_SENSOR_SYSTEMS_VIBRATION_KNOCK` | 136 | Vibration/knock behaviour measurement |
| `F_SENSOR_SYSTEMS_WEATHER_INSTRUMENTS` | 137 | Weather instruments identification |
| `F_SENSOR_SYSTEMS_SOIL_SCOUTING` | 138 | Soil physical parameter measurement |
| **Timber Harvesters** | | |
| `F_TIMBER_HARVESTERS_TIMBER_HARVESTORS_MACHINE_CONTROL` | 132 | Machine control |
| **Forwarders** | | |
| `F_FORWARDERS_FORWARDERS_MACHINE_CONTROL` | 132 | Machine control |
| **Timber Loaders** | | |
| `F_TIMBER_LOADERS_TIMBER_LOADERS_MACHINE_CONTROL` | 132 | Machine control |
| **Timber Processing Machines** | | |
| `F_TIMBER_PROCESSING_MACHINES_TIMBER_PROCESSING_MACHINE_CONTROL` | 132 | Machine control |
| **Mulchers** | | |
| `F_MULCHERS_MULCHER_MACHINE_CONTROL` | 132 | Machine control |
| **Utility Vehicles** | | |
| `F_UTILITY_VEHICLES_UTILITY_MACHINE_CONTROL` | 132 | Machine control |
| **Slurry/Manure Applicators** | | |
| `F_SLURRY_MANURE_APPLICATORS_SLURRY_MANURE_RATE_CONTROL` | 128 | Rate of product placed on soil |
| `F_SLURRY_MANURE_APPLICATORS_SECTION_ON_OFF_CONTROL` | 129 | Section on/off |
| `F_SLURRY_MANURE_APPLICATORS_PRODUCT_PRESSURE` | 130 | Pressure in delivery booms |
| `F_SLURRY_MANURE_APPLICATORS_SLURRY_MANURE_MACHINE_CONTROL` | 132 | Machine control |
| `F_SLURRY_MANURE_APPLICATORS_PRODUCT_FLOW` | 133 | Product flow |
| `F_SLURRY_MANURE_APPLICATORS_PRODUCT_LEVEL` | 134 | Product level in bin |
| `F_SLURRY_MANURE_APPLICATORS_BOOM_HEIGHT_CONTROL` | 135 | Boom height above soil |
| **Feeders/Mixers** | | |
| `F_FEEDERS_MIXERS_FEEDER_MIXER_RATE_CONTROL` | 128 | Rate of product distributed to livestock |
| `F_FEEDERS_MIXERS_SECTION_ON_OFF_CONTROL` | 129 | Section on/off (e.g., Left/Center/Right) |
| `F_FEEDERS_MIXERS_PRODUCT_PRESSURE` | 130 | Pressure in delivery units |
| `F_FEEDERS_MIXERS_FEEDER_MIXER_MACHINE_CONTROL` | 132 | Machine control |
| `F_FEEDERS_MIXERS_PRODUCT_FLOW` | 133 | Product flow |
| `F_FEEDERS_MIXERS_PRODUCT_LEVEL` | 134 | Product level in bin |
| `F_FEEDERS_MIXERS_BOOM_HEIGHT_CONTROL` | 135 | Boom height above soil |
| **Weeders** | | |
| `F_WEEDERS_WEEDER_MACHINE_CONTROL` | 132 | Machine control (non‑chemical weed control) |
| **Turf and Lawn Care Mowers** | | |
| `F_TURF_AND_LAWN_CARE_MOWERS_TURF_AND_LAWN_CARE_MOWERS_MACHINE_CONTROL` | 132 | Machine control for grass cutting |
| **Product/Material Handling** | | |
| `F_PRODUCT_MATERIAL_HANDLING_PRODUCT_MATERIAL_HANDLING_MACHINE_CONTROL` | 132 | Machine control |
| `F_PRODUCT_MATERIAL_HANDLING_PRODUCT_MATERIAL_HANDLING_PRODUCT_FLOW` | 133 | Product flow |
| `F_PRODUCT_MATERIAL_HANDLING_PRODUCT_MATERIAL_HANDLING_PRODUCT_LEVEL` | 134 | Product level in bin |

### **Adapters**
None – The constant set does not contain any adapters.

## Functionality
The `IG2_NAME_Functions` constant set provides a standardised vocabulary for identifying the function of ISOBUS control functions. Each constant corresponds to a byte value that is used in the `function` field of the NAME message (per ISO 11783-7). This allows devices on an agricultural bus to advertise their capabilities, enabling proper configuration and interoperability. The constants are organised by device class (e.g., Tractor, Harvesters, Sprayers) to simplify lookup and ensure that function codes are unique within a given class.

For a given device class, a device may implement one or more of these functions (e.g., a sprayer might have function codes for boom height control, product flow, and section on/off). The availability of `F_*_NOT_AVAILABLE` (value 255) indicates that no explicit function has been assigned yet, serving as a placeholder during development.

## Technical Features
- **Type:** All constants are of type `BYTE` (unsigned 8‑bit integer).
- **Values:** Assigned values range from 128 to 142 (non‑specific system) and 128 to 137 (specific device classes), with 255 universally used as “not available”.
- **Grouping:** Constants are grouped into device classes as defined in the ISO 11783 standard (e.g., `DC_TRACTOR = 1`, `DC_TILLAGE = 2`, …). This grouping is reflected in the naming convention: `F_<DEVICE_CLASS>_<FUNCTION_NAME>`.
- **Naming:** Names are descriptive and follow a pattern `F_<device_class>_<specific_function>` to avoid collisions (e.g., `F_SPRAYERS_BOOM_HEIGHT_CONTROL` vs. `F_FERTILIZERS_HEIGHT_DEPTH_CONTROL`).
- **Purpose:** The constants are intended for use in the `NAME` field of an ISOBUS device, particularly within the `function` byte of the “NAME” attribute.

## State Overview
This constant set does not represent a state machine; it is a static list of predefined values. However, the presence of a `NOT_AVAILABLE` (255) constant can be considered a fallback state for devices that have not yet been assigned a specific function. Once a device is configured, its actual function value is set from one of the listed constants. The constants are not subject to runtime changes – they are global immutable values.

## Application Scenarios
- **ISOBUS Device Development:** Implementers can include these constants in their code to set the NAME field when initialising a control function on an ISO 11783 bus.
- **Configuration Tools:** Software that configures agricultural equipment can use the constants to allow users to select the correct function from a dropdown or list.
- **Diagnostics and Monitoring:** Tools that analyse bus messages can interpret the function byte using these constants to display meaningful device roles.
- **System Simulation:** Simulation models of agricultural networks can use these constants to model various implement types.

## Comparison with Similar Blocks
There is no direct comparison to function blocks, as this is a global constant definition. However, in the context of ISOBUS, comparable constant sets may exist for other industry groups (e.g., Industry Group 1 – forestry, Industry Group 3 – municipal). The structure differs in that those groups would have their own device classes and function codes. This set is specifically tailored for agricultural equipment (Industry Group 2). Unlike a function block, which encapsulates logic and has inputs/outputs, this constant set is purely declarative and serves as a data dictionary.

## Conclusion
The `IG2_NAME_Functions` global constant set is an essential resource for developers working with ISO 11783 (ISOBUS) agricultural networks. It provides a comprehensive, standardised list of function codes covering a wide range of agricultural machinery and sensor systems. By using these constants, developers can ensure consistency and interoperability across different implementations, facilitating seamless communication between tractors, implements, and other devices on the bus.