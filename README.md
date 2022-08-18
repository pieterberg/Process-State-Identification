<img src="assets/schematics/process_schematic.png">

# Process State Identification

<img src="https://img.shields.io/badge/Stellenbosch University-BEng ChemE-008BC0?style=flat"/>

BEng ChemE final year project using PCA and statistical classification to identify a process' state.

## Usage instructions

1. Run the Simulink model from the MATLAB Live Script `process_model/copper_solvent_extraction_model.mlx`.

1. Run the MATLAB Live Script from the main `Process-State-Identification` folder.

## Simulation settings

| Setting               | Values                    | Description                                                                 |
|-----------------------|---------------------------|-----------------------------------------------------------------------------|
| `SAVE_IMAGES`         | - `true` <br> - `false`   | Saves the generated images to the respective output folder if set to `true` |
| `SAVE_DATA`           | - `true` <br> - `false`   | Saves the simulation data to the respective output folder if set to `true`  |
| `SENSOR_NOISE`        | - `true` <br> - `false`   | Adds sensor noise to the measurements if set to `true`                      |
| `FEEDBACK_CONTROL`    | - `true` <br> - `false`   | Enables feedback control of the process if set to `true`                    |
| `FEEDFORWARD_CONTROL` | - `true` <br> - `false`   | Enables feedforward control of the process if set to `true`                 |
