# TXST-TECH4397-WaterQuality-ML
Repository for tracking code for TECH 4397 class, Texas State University, Fall 2026.

Created by Alejo Ruiz Camauer

# TECH 4397 - SPECIAL PROBLEMS - FALL 2026 - TEXAS STATE UNIVERSITY

Authors:
- Alejo Ruiz Camauer (Undergraduate) 
- Gia Spencer (Undergraduate)
- Saul Gonzalez (Undergraduate)
- Skylar Dudley (Undergraduate)

- Shashank Musunooru (Graduate, original thesis author)

- Rodney Raymond Okanya (Research Associate, Department of Engineering Technology)
- Dr. Meysam Khaleghian (Course Overseer)
- Dr. Junseo Choi (Course Overseer)

---

# Abstract
This repository collects the data, analysis notebooks, reports, and supporting materials developed during a semester-long TECH 4397 project on sensor-based water-quality monitoring. Building on Shashank Musunooru’s experimental framework, the project examined whether pH, conductivity-derived total dissolved solids, dissolved oxygen, and turbidity measurements could support machine-learning identification of five chemicals and estimation of their doses.

The work progressed from statistical analysis and model evaluation to audits of sensor transduction, experimental procedures, dissolved-oxygen drift, natural-water matrix effects, and chemical mixtures. Findings highlighted the limits of transferring controlled laboratory models to field conditions: baseline correction can remove constant offsets, but cannot restore chemically suppressed responses or supply the independent information needed to distinguish ambiguous mixtures.

The repository preserves experimental source data alongside computational demonstrations and documents their assumptions, limitations, and corrections. It provides a basis for inspecting the analysis, reproducing supported results, and designing further experiments toward reliable field monitoring.

## Machine Learning Architecture

The project uses a two-stage pipeline to identify a chemical and estimate its dose from water-quality sensor readings.

1. **Stage 1 — Chemical classification:** An SVM classifier with a radial basis function (RBF) kernel predicts one of five chemical classes: magnesium, sodium chloride, calcium carbonate, sodium bicarbonate, or hydrogen peroxide.
2. **Stage 2 — Dose estimation:** An SVR regressor uses the sensor features and predicted chemical identity to estimate the dose. A classification error can therefore affect the dose estimate.

The full model uses four sensor channels and three engineered features: log-transformed turbidity, pH-voltage/TDS-voltage, and TDS-voltage/DO-voltage. A five-feature alternative excludes DO and TDS/DO to reduce sensitivity to DO instability.

The pipeline assumes a single chemical from the known classes. It does not establish reliable identification of unknown contaminants or mixtures. Notebook implementations vary; some use synthetic data or alternative regressors.

## Sensor Hardware

The experimental system records analog sensor outputs through a National Instruments USB-6218 data-acquisition unit.

| Parameter | Sensor type | Measurement principle | Main limitations |
|---|---|---|---|
| **pH** | Glass combination electrode | Measures hydrogen-ion-dependent membrane potential against a reference electrode | Temperature dependence, reference-junction fouling, glass aging, and ionic interference |
| **TDS / conductivity** | Two-pin contact probe | Measures ionic conductance; estimates TDS through a calibration relationship | Electrode polarization, temperature dependence, fouling, and composition-dependent conversion |
| **Dissolved oxygen (DO)** | Galvanic membrane probe with a lead anode | Oxygen crosses a membrane and undergoes reduction at the cathode, producing current | Flow dependence, membrane condition, anode consumption, and sensitivity to sample aeration |
| **Turbidity** | Optical turbidity probe | Measures changes in light scattering and/or transmission caused by suspended particles | Bubbles, settling, particle properties, optical fouling, and nonlinear response |

Readings are **raw voltage outputs unless otherwise labeled**. Calibrations are required to express them in physical units. Confirm exact probe models, optical geometry, ranges, and accuracy against the original hardware documentation.

## Running the Notebooks in Google Colab

1. **Download the repository.** Select **Code → Download ZIP** on GitHub, then extract the files.
2. **Open Google Colab.** Visit [Google Colab](https://colab.research.google.com/) and sign in.
3. **Upload a notebook.** Select **File → Upload notebook** and choose an `.ipynb` file from `notebooks/`.
4. **Save a working copy.** Select **File → Save a copy in Drive** to preserve your changes.
5. **Provide input data if required.** Open Colab’s **Files** sidebar and upload the relevant files from `data/`. Update the notebook’s file paths to match their uploaded locations, typically `/content/filename.xlsx`. Self-contained simulation notebooks may not require uploads.
6. **Check dependencies.** Run any setup cells first. If an import fails, install the missing package in a code cell, for example:
   ```python
   %pip install numpy pandas matplotlib scikit-learn openpyxl
   ```
7. **Run the cells in order.** Select **Runtime → Run all**, or run individual cells with **Shift + Enter**. Review any errors before continuing.
8. **Save the results.** Save the notebook to Drive or download it through **File → Download → Download .ipynb**.

Files uploaded to Colab’s temporary runtime may disappear when the session ends. Check whether each notebook uses experimental measurements or generated data before interpreting its results.
