# RCBldGH - Parametric Building Energy Simulation Plugin for Rhino and Grasshopper

**RCBldGH** is a parametric building energy consumption and heating/cooling load simulation plugin developed based on the Rhino and Grasshopper API. It automates the processing of 3D building geometries in Rhino and invokes the grey-box RC building energy simulation tool **[RCBldEng](https://github.com/andersonspy/RCBIdEng)** (written in C#) to output energy consumption and heating/cooling load results for building designs.

## 🚀 Features

*   Process Rhino Brep geometries for energy simulation.
*   Assign materials, window configurations (including custom shapes, properties, and shading), building usage programs, and detailed schedules (hourly, weekly, monthly, annual).
*   Integrate site information (EPW weather files, ground properties) and basic energy system details.
*   Invoke the [`RCBldEng.exe`](https://github.com/andersonspy/RCBIdEng/releases) engine for simulation via a dedicated `Run` component.
*   Output simulation results (energy consumption, loads, indoor temperatures) as CSV files.
*   Provide components (`Hourly CSV Reader`, `Indoor Temperature CSV Reader`, `Monthly CSV Reader`) for reading and visualizing results within Grasshopper.

## 📦 Package Contents

The RCBldGH package includes the following:

```
Parent_Directory/
├── RCBldEng/
│   └── RCBldEng.exe (grey-box RC simulation engine)
├── Projects/ (stores energy simulation results - initially empty)
├── example.gh (example Grasshopper file)
├── RCBldGH.gha (Grasshopper plugin file)
└── RCBldGH Manual.docx (Detailed user manual)
```

## 🛠️ Installation

1.  **Configure Environment Variable:**
    *   Create a system environment variable named `RCBldEng`.
    *   Set its value to the **full file path** of the `RCBldEng` folder included in this release.

2.  **Install Grasshopper Plugin:**
    *   Copy the `RCBldGH.gha` file into your Grasshopper Components folder (usually accessible via Grasshopper menu: `File > Special Folders > Components Folder`).
    *   Restart Rhino and Grasshopper.

## 🏗️ Workflow Overview (using `example.gh`)

The `example.gh` file demonstrates the typical workflow for using RCBldGH.

### 1. Input Data Section

This section focuses on preparing building geometry and related data for simulation.

#### 1.1 MaterialAssign Component

This component is crucial for aggregating and assigning basic information to thermal zones:

*   **Building Geometry (Breps):** Your 3D building model from Rhino.
*   **Window Configurations (Windows):**
    *   Custom window shapes (Surfaces).
    *   Window material properties (thermal resistance, emissivity, SHGC).
    *   Shading settings (tilt angle, type, location, control method).
    *   *Use the dedicated **Window Component** to define these settings.*
*   **Envelope Material Settings (MaterialSetting):**
    *   Define materials for Roof, exterior walls, interior walls, exterior floor, interior floor, ground, and window glass.
    *   Material properties are defined by U-value, absorption coefficient, and reflection coefficient.
    *   *Use the dedicated **MaterialSetting Component** for this.*
*   **Building Usage (Program):**
    *   Configure occupancy rate, metabolic rate, appliance intensity, outdoor air rate, air infiltration rate, window open area percentage, air infiltration level, ventilation type, DHW load, night flushing, lighting template, and HVAC template.
    *   *Use the dedicated **Program Component** for this.*
*   **Schedules:**
    *   Define 24-hour schedules for heating/cooling, HVAC, lighting, occupancy, and appliance usage.
    *   Aggregate 24-hour schedules into weekly schedules using **Building Use Schedule (BUS)** and **Indoor Temperature Setpoint Schedule (ITSS)**.
    *   Combine weekly schedules with monthly schedules (e.g., Monthly BUS, Monthly ITSS, Infiltration Monthly Schedule) and Timespans to generate annual schedules in **ScheduleSetting**.
    *   *Use the dedicated **Schedule Component** for defining and combining schedules.*

The **MaterialAssign** component outputs **SuperBreps**, which automatically assign material data to building geometries, resolve adjacency relationships, and generate thermal zone data ready for energy simulation.

#### 1.2 BEM Component

This component integrates the thermal zone data with site and energy system information:

*   **Site Information:**
    *   EPW-format weather file.
    *   Building name (for labeling).
    *   Building type, terrain class, ground type.
    *   Envelope heat capacity, effective mass area.
    *   Monthly ground temperatures.
*   **Energy Systems:**
    *   Hot water, heat pumps.
    *   Energy sources.
    *   Control strategies.
    *   Renewable energy types.

*   *Use the dedicated **BEM Component** to provide these inputs.*

### 2. Simulation & Results

#### 2.1 Run Component

After setting up all the inputs, use the **Run** component to initiate the simulation.

*   Set the Toggle input to `True` to start the simulation.
*   The plugin will invoke `RCBldEng.exe` (using the configured `RCBldEng` system variable) to perform the simulation.
*   Simulation results will be output as CSV files to the `Projects` folder.

#### 2.2 Result Visualization

Use the dedicated components to read and visualize the simulation results within Grasshopper:

*   **Hourly CSV Reader:** Displays hourly energy consumption data.
*   **Indoor Temperature CSV Reader:** Shows indoor temperature results.
*   **Monthly CSV Reader:** Aggregates and displays monthly energy usage.

## ⚠️ Requirements & Important Notes

*   Requires **Rhino** and **Grasshopper**.
*   The `RCBldEng` environment variable **must** be correctly configured for the simulation to run.
*   Ensure your Rhino model units are set to **Meters (m)**. The plugin may issue a yellow warning if units are incorrect.

## 📚 Getting Started

*   Review the `RCBldGH Manual.docx` for detailed instructions on each component and parameter.
*   Explore the `example.gh` file to understand the typical workflow and component connections.

Enjoy using **RCBldGH** for your building energy simulations!
