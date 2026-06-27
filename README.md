# EV-Charging-Scenarios-for-Flexibility-Simulation-Limited-Data

This repository contains a Python-based simulation tool designed to model Electric Vehicle (EV) charging profiles and evaluate grid flexibility. It uses real-world constraints—such as EV battery capacity, onboard AC charger limits, session duration, and specific charging curves—to generate highly accurate minute-by-minute instantaneous power delivery models.

The simulation leverages **Monte Carlo sampling** to assign EV models to charging sessions and calculates the State of Charge (SOC) based on user-defined probability distributions.

##  Features

* **Three Simulation Modes:** Run simulations with fixed SOC ranges, variable starting SOCs, or variable start and end SOCs based on normalized probability distributions.
* **Multiprocessing Support:** Utilizes Python's `multiprocessing` library to parallelize calculations across multiple CPU cores, drastically reducing execution time for large datasets.
* **Realistic Charging Physics:** Calculates actual energy delivered by taking the minimum value between the station's max power, the EV's onboard AC charger, and the battery's specific charging speed at a given SOC.
* **Interactive CLI:** An easy-to-use command-line interface that guides you through selecting files, CPU cores, and inputting custom probability distributions.

##  Prerequisites

The program requires **Python 3.7+**. Most of the modules used are part of the Python Standard Library (e.g., `os`, `multiprocessing`, `json`, `datetime`), but you will need to install a few external dependencies.

1. Clone this repository:
   ```bash
   git clone [https://github.com/yourusername/ev-charging-simulation.git](https://github.com/yourusername/ev-charging-simulation.git)
   cd ev-charging-simulation



```

2. Install the required dependencies:
```bash
pip install -r requirements.txt

```


## 📂 Project Structure

```text
├── calculate_charging_profile_multiprocess_relative.py   # Main simulation script
├── requirements.txt                                      # Python dependencies
└── data/
    ├── inputs/
    │   ├── sample_charging_sessions.csv                  # Session data (duration, max power)
    │   └── ev_models_for_simulation.json                 # EV specs and charging curves
    └── outputs/                                          # Generated simulation results

```

## Usage

To start the simulation, simply run the script from your terminal:

```bash
python calculate_charging_profile_multiprocess.py

```

The script will launch an interactive prompt asking you to configure the simulation. You can choose to use the default sample files or provide absolute/relative paths to your own custom datasets.

### Simulation Modes

When you run the program, you will be prompted to select one of three modes:

* **Mode 1: Fixed SOC ranges**
Calculates all possible charging profiles iteratively across fixed SOC start/end intervals (e.g., 25% to 30%, 25% to 35%, etc.).
* **Mode 2: Variable SOC_start**
Uses a discrete probability distribution (e.g., 20% probability of arriving with 30% SOC). It uses Monte Carlo inverse transform sampling to assign a starting SOC to each vehicle, assuming the target is always a 100% charge.
* **Mode 3: Variable SOC_start & Charging Amount**
Extends Mode 2 by also applying a probability distribution to the *amount* of charge added during the session, allowing for highly realistic, varied user behavior where users don't always charge to 100%.

*Note: For Modes 2 and 3, you can use the default probability distributions or type in your own directly via the command line. The program will automatically validate that your probabilities sum to 1.0 (and offer to normalize them if they don't).*

## Data Inputs

If you are providing your own files, ensure they follow this format:

**1. EV Models (`.json`)**
Must contain EV brands/models, market percentage, battery size (kWh), onboard AC charger limit (kW), and a 0-100% `charging_profile` speed mapping.

**2. Charging Sessions (`.csv`)**
Must include at least the following columns: `max_electric_power`, `start_time`, `end_time`, and `duration_minutes`.

## 📈 Outputs

The script generates outputs in the `data/outputs/` directory:

1. **Instantaneous Power CSVs:** Minute-by-minute charging power (kW) data for every single session simulated.
2. **Charging Info CSVs:** A summary dataset detailing the total energy delivered, actual time spent charging vs. time plugged in, and a boolean `Flexibility` flag indicating if the car finished charging before it was unplugged.

## 📄 License

This project is licensed under the MIT License.

```

```
