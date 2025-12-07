# Hemophilia Treatment Dynamics Simulation

This project simulates the pharmacokinetics and pharmacodynamics (PK/PD) of Hemophilia A treatment using a system of coupled Ordinary Differential Equations (ODEs). It models the interaction between Clotting Factor VIII ($S_T$), immune inhibitors ($Y$), and Gene Therapy interventions.

The simulation handles a **hybrid dynamical system**, combining continuous physiological dynamics with discrete impulsive control events (bolus infusions).

## 📝 Overview

The code models three primary state variables:
1.  **$S_T$ (Factor VIII Level):** The concentration of the clotting factor in the blood (IU/dL). It is affected by infusions, natural clearance, Von Willebrand Factor (VWF) stabilization, and neutralization by inhibitors.
2.  **$Y$ (Inhibitor Titer):** The level of anti-drug antibodies (Bethesda Units - BU). It grows based on exposure to Factor VIII (immune response) and decays via Immune Tolerance Induction (ITI) or natural clearance.
3.  **$F8_{gene}$ (Gene Therapy Expression):** The rate of endogenous factor production following a gene therapy vector administration, modeled as a decaying burst.

## ⚙️ Dependencies

To run this simulation, you need a standard scientific Python environment.

*   **Python 3.x**
*   **NumPy** (Numerical operations)
*   **SciPy** (ODE integration via `solve_ivp`)
*   **Matplotlib** (Visualization)

### Installation
```bash
pip install numpy scipy matplotlib
```

## 🚀 Usage

Simply run the script. It contains self-contained classes, solver logic, and an execution block that generates a plot.

```bash
python hemophilia_simulation.py
```

## 🧠 Mathematical Model

The system uses the following mechanisms, defined in `clotting_factor_system`:

*   **Michaelis-Menten Kinetics:** Used for continuous infusion rates (though the default example uses bolus dosing).
*   **Hill Kinetics:** Used to model non-linear inhibitor neutralization and VWF cooperative stabilization.
*   **Impulsive Differential Equations:** The solver `simulate_hemophilia_treatment` integrates the ODEs between infusion events, then applies an instantaneous "jump" (bolus) to the state vector at specific time points.

### Key Parameters (`ClottingFactorParameters`)
*   `lambda_ST`: Physiological clearance rate of Factor VIII.
*   `alpha`, `beta`, `K_I`: Parameters governing how effectively inhibitors neutralize the factor.
*   `rho`, `Y_max`: Parameters governing the immune system's production of inhibitors (proliferation).
*   `A`, `gamma_GT`: Gene therapy burst magnitude and decay rate.

## 📊 Scenarios Simulated

The `__main__` execution block simulates two distinct scenarios for comparison:

1.  **High Inhibitor Patient (Red Line):**
    *   A patient with a high starting inhibitor titer (10 BU).
    *   Receives prophylactic factor infusions (50 IU/dL) every 48 hours.
    *   Demonstrates how high inhibitors suppress effective factor levels despite frequent dosing.

2.  **Gene Therapy Intervention (Blue Dashed Line):**
    *   A patient with low inhibitors.
    *   Starts with standard prophylaxis.
    *   **Event:** At $t=360$ hours, a Gene Therapy vector is administered.
    *   Demonstrates the "burst" of endogenous factor production and its subsequent decay, overlaid with continued prophylaxis.

## 📂 Code Structure

*   **`ClottingFactorParameters`**: A configuration class holding all physiological constants (rates, half-saturation constants, Hill coefficients).
*   **`clotting_factor_system(t, X, params)`**: The core ODE function defining the derivatives $dX/dt$.
*   **`simulate_hemophilia_treatment(...)`**: The wrapper function. It manages the time-stepping, pauses integration at infusion times to add the bolus dose, and restarts integration.
*   **Visualization**: The final section generates a 3-panel subplot comparing Factor Levels, Inhibitor Titers, and Gene Therapy output over 30 days.

## ⚠️ Disclaimer

**This software is for educational and research purposes only.**
The parameters provided in the code are placeholder values derived from theoretical models. This simulation should **not** be used for actual medical dosing or clinical decision-making without calibration against real patient data.
