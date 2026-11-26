# BargraphSplit_S

* * * * * * * * * *

## Introduction

Description from the comment of the type: ISOBUS UT split-bargraph object pool properties (two adjacent Linear Bar Graphs visualizing one signed value as two unsigned magnitude fills)

Package: `isobus::utils::bargraph`

## Interface structure

### **Fields**

-   **stLeft** (Type: `logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object pool properties of the Linear Bar Graph filling outward for negative values
-   **stRight** (Type: `logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object pool properties of the Linear Bar Graph filling outward for positive values
-   **r32MinMagnitude** (Type: `REAL`): Shared lower clamp bound for both sides' magnitude, matches both Bargraphs' own Min (Default: `0.0`)
-   **r32MaxMagnitude** (Type: `REAL`): Shared upper clamp bound for both sides' magnitude, matches both Bargraphs' own Max (Default: `0.0`)

Note: This page was generated from the type definition (`BargraphSplit_S.dtp`) and only shows the interface. A detailed description of the behavior is still missing.
