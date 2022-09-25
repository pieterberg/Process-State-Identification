<img src="assets/schematics/process_schematic.png">

# Process State Identification

<img src="https://img.shields.io/badge/Stellenbosch University-BEng ChemE-008BC0?style=flat"/>

Chemical Engineering final year project using PCA and statistical classification to identify a process' state.

The process model consists of a dynamic model of a copper solvent extraction plant based on the model developed by Komulainen et al. (2009, 2006). 
The model simulates two control valve faults, namely:

- stuck valves
- and valve stiction, based on the physical valve stiction model developed by Shoukat Choudhury et al. (2005)

## 1. Usage instructions

### 1.1. Running the simulation

1. Run the Simulink model from the MATLAB Live Script `process_model/copper_solvent_extraction_model.mlx`.

1. Run the MATLAB Live Script from the main `Process-State-Identification` folder.

### 1.2. Simulation settings

| Setting                           | Values                       | Description                                                                 |
|-----------------------------------|------------------------------|-----------------------------------------------------------------------------|
| `SAVE_IMAGES`                     | `true` <br> `false`          | Saves the generated images to the respective output folder if set to `true`.|
| `SAVE_DATA`                       | `true` <br> `false`          | Saves the simulation data to the respective output folder if set to `true`. |
| `SENSOR_NOISE`                    | `true` <br> `false`          | Adds sensor noise to the measurements if set to `true`.                     |
| `FEEDBACK_CONTROL`                | `true` <br> `false`          | Enables feedback control of the process if set to `true`.                   |
| `FEEDFORWARD_CONTROL`             | `true` <br> `false`          | Enables feedforward control of the process if set to `true`.                |
| `EXTERNAL_VARIABLES_STEADY_STATE` | `true` <br> `false`          | Sets the external variables to their constant steady-state values when set to `true`, or loads the non-steady-state external variable dataset selected by `EXTERNAL_VARIABLES_DATASET` when set to `false`.|
| `EXTERNAL_VARIABLES_DATASET`      | `'training'` <br> `'testing'`| Loads the training non-steady-state external variable dataset when set to `'training'`, or the testing non-steady-state external variable dataset when set to `'testing'`. Must specify a dataset to use regardless of the value of `EXTERNAL_VARIABLES_STEADY_STATE`.|
| `PROCESS_STATE`                   | see below                    | Select which process faults occur by selecting the process state. See Process States section below. |

### 1.3. Output folder structure

Create the following folder structure to save to simulation results using the `SAVE_IMAGES` and `SAVE_DATA` settings:

```
.
|__ process_model
    |__ output
        |__ data
        |__ graphs
            |__ measured_variables
            |__ process_variables
```

Create the following folder structure to save the ARX model results:

```
.
|__ controller_tuning
    |__ output
        |__ feedback
        |__ feedforward
```

Create the following folder structure to save the smoothed external variable data and graphs:

```
.
|__ external_variables
    |___ output
         |__ data
         |__ graphs

```

## 2. Process States

The desired fault combinations can be set using the `PROCESS_STATE` setting. The following table shows which loaded organic valve faults and which lean electrolyte valve faults occur for the selected process state.

| Process State | Loaded Organic Valve | Lean Electrolyte Valve |
|:-------------:|:--------------------:|:----------------------:|
|      `0`      |        normal        |         normal         |
|      `1`      |         stuck        |         normal         |
|      `2`      |       stiction       |         normal         |
|      `3`      |        normal        |          stuck         |
|      `4`      |        normal        |        stiction        |
|      `5`      |         stuck        |          stuck         |
|      `6`      |         stuck        |        stiction        |
|      `7`      |       stiction       |          stuck         |
|      `8`      |       stiction       |        stiction        |

The time that the selected process faults occur can be set using the following `start_time` parameters. Corresponding `stop_time` parameters are also provided to set the time at which the process faults stop ocurring. If no stop time is desired, set the respective `stop_time` parameter equal to the simulation time.

| Parameter                   | Units | Description                                          |
|-----------------------------|:-----:|------------------------------------------------------|
| `valve_LO_fault_start_time` |   s   | Time for the loaded organic valve's fault to occur   |
| `valve_LO_fault_stop_time`  |   s   | Time for the loaded organic valve's fault to stop    |
| `valve_LE_fault_start_time` |   s   | Time for the lean electrolyte valve's fault to occur |
| `valve_LE_fault_stop_time`  |   s   | Time for the lean electrolyte valve's fault to stop  |

The valve states are represented using the following values:

| Value | Valve State |
|:-----:|:-----------:|
|  `0`  |    normal   |
|  `1`  |    stuck    |
|  `2`  |   stiction  |

The valve states are selected using Variant Subsystems. The variant subsystems are defined in the `PreLoadFcn` callback of the Simulink model.

## References

Komulainen, T., Doyle, F.J., Rantala, A., Jämsä-Jounela, S.L., 2009. Control of an industrial copper solvent extraction process. Journal of Process Control 19, 2–15. https://doi.org/10.1016/j.jprocont.2008.04.019

Komulainen, T., Pekkala, P., Rantala, A., Jämsä-Jounela, S.L., 2006. Dynamic modelling of an industrial copper solvent extraction process. Hydrometallurgy 81, 52–61. https://doi.org/10.1016/j.hydromet.2005.11.001

Shoukat Choudhury, M.A.A., Thornhill, N.F., Shah, S.L., 2005. Modelling Valve Stiction. Control Eng Pract 13, 641–658. https://doi.org/10.1016/j.conengprac.2004.05.005


