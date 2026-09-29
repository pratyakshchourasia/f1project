# F1 Aero & Lap Time Simulator

A Python-based Formula 1 data visualization and telemetry analysis project that uses real Formula 1 race data to visualize a driver's lap, speed, throttle, braking, and gear usage on a circuit.

---

##  Project Overview

The **F1 Aero & Lap Time Simulator** is a Python project designed to analyze and visualize Formula 1 racing telemetry using real-world data.

The project uses the **FastF1 Python library** to obtain Formula 1 session and telemetry data. The extracted data is processed using Python and displayed using **Matplotlib**.

For the current implementation, the project analyzes **Charles Leclerc's Ferrari telemetry from the 2024 Monaco Grand Prix**.

The simulator visualizes the car's movement around the circuit while displaying important driving parameters such as:

-  Car position on the circuit
-  Speed
-  Throttle input
-  Brake application
- Gear selection
-  Lap telemetry

---

# Features

## 1. Formula 1 Data Retrieval

- Retrieves real Formula 1 session data using FastF1.
- Supports race session data from different Formula 1 events.
- Extracts driver-specific telemetry.

## 2. Telemetry Analysis

The project works with telemetry parameters including:

- X and Y track coordinates
- Vehicle speed
- Throttle percentage
- Brake application
- Gear selection
- Distance travelled
- Lap information
- Session time

## 3. Circuit Visualization

- Displays the driver's movement around the circuit.
- Uses X-Y telemetry coordinates to recreate the track layout.
- Animates the car's movement around the circuit.

## 4. Driver Telemetry Visualization

The animation allows the user to observe:

- Current car position
- Current speed
- Current gear
- Throttle input
- Brake application

## 5. Data Processing

The project:

- Selects the required driver.
- Identifies the fastest lap.
- Retrieves telemetry data.
- Removes incomplete telemetry points.
- Converts telemetry data into arrays for visualization.

---

# Technologies and Tools Used

## Programming Language

- **Python 3**

## Python Libraries

### FastF1

Used to retrieve and process Formula 1 timing, telemetry, lap, and session data.

### Pandas

Used for handling and filtering telemetry and lap data.

### Matplotlib

Used for creating the circuit visualization and telemetry graphs.

### Matplotlib Animation

`FuncAnimation` is used to animate the movement of the car around the circuit.

## Development Tools

- Visual Studio Code
- Python Terminal / PowerShell
- Git
- GitHub

---

# Installation & Running the Project

## 1. Prerequisites

Before running the project, make sure the following are installed:

- Python 3.x
- Visual Studio Code (recommended)
- Git (optional)

Check Python installation:

```bash
## Testing Instructions

The project can be tested using the following procedure.

### Test 1: Check Python Installation

Run:

```bash
python --version
```

Expected result:

```text
Python 3.x.x
```

---

### Test 2: Check FastF1 Installation

Run:

```bash
python -c "import fastf1; print('FastF1 installed successfully')"
```

Expected output:

```text
FastF1 installed successfully
```

---

### Test 3: Check Matplotlib Installation

Run:

```bash
python -c "import matplotlib; print('Matplotlib installed successfully')"
```

Expected output:

```text
Matplotlib installed successfully
```

---

### Test 4: Run the Main Program

Run:

```bash
python f1_data.py
```

### Expected Result

If all tests pass successfully, the program should run without Python errors and display the F1 telemetry visualization and animation for Charles Leclerc's fastest lap at the **2024 Monaco Grand Prix**.
