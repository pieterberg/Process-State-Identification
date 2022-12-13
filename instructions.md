# Instructions

Instructions for running the process model, generating the external variable datasets, tuning the feedback and feedforward controllers, performing the fault detection, and quantifying the fault detection performance are provided in this document.

### Table of Contents

- [1. Process model](#1-process-model)
  - [1.1. Running the model](#11-running-the-model)
  - [1.2. Process settings](#12-process-settings)
  - [1.3. Output folder structure](#13-output-folder-structure)
  - [1.4. Process states](#14-process-states)
  - [1.5 Lowpass filter tuning](#15-lowpass-filter-tuning)
- [2. External variables](#2-external-variables)
  - [2.1. Running the model](#21-running-the-model)
  - [2.2. Output folder structure](#22-output-folder-structure)
  - [2.3. Using the generated external variables](#23-using-the-generated-external-variables)
  - [2.4. External variables used in the simulation](#24-external-variables-used-in-the-simulation)
- [3. Controller tuning](#3-controller-tuning)
  - [3.1. Running the model](#31-running-the-model)
  - [3.2. Output folder structure](#32-output-folder-structure)
- [4. Fault detection](#4-fault-detection)
  - [4.1. Running the fault detection](#41-running-the-fault-detection)
  - [4.2. Fault detection settings](#42-fault-detection-settings)
  - [4.3. Output folder structure](#43-output-folder-structure)
- [5. ROC curves](#5-roc-curves)
  - [5.1. Generating the ROC curves](#51-generating-the-roc-curves)
  - [5.2. ROC curve settings](#52-roc-curve-settings)
  - [5.3. Constructing the ROC curves](#53-constructing-the-roc-curves)
  - [5.4. Output folder structure](#54-output-folder-structure)

## 1. Process model

### 1.1. Running the model

1. Run the Simulink model from the MATLAB Live Script `process_model/copper_solvent_extraction_model.mlx`
1. Run the MATLAB Live Script from the main `Process-State-Identification` folder

### 1.2. Process settings

#### Table 1: Process settings
| Setting                           | Values                       | Description                                                                  |
|-----------------------------------|------------------------------|------------------------------------------------------------------------------|
| `SAVE_IMAGES`                     | `true` <br> `false`          | Saves the generated images to the respective output folders if set to `true`.|
| `SAVE_DATA`                       | `true` <br> `false`          | Saves the simulation data to the respective output folder if set to `true`.  |
| `SENSOR_NOISE`                    | `true` <br> `false`          | Adds sensor noise to the measurements if set to `true`.                      |
| `FEEDBACK_CONTROL`                | `true` <br> `false`          | Enables feedback control of the process if set to `true`.                    |
| `FEEDFORWARD_CONTROL`             | `true` <br> `false`          | Enables feedforward control of the process if set to `true`.                 |
| `EXTERNAL_VARIABLES_STEADY_STATE` | `true` <br> `false`          | Sets the external variables to their constant steady-state values when set to `true`, or loads the non-steady-state external variable dataset selected by `EXTERNAL_VARIABLES_DATASET` when set to `false`.|
| `EXTERNAL_VARIABLES_DATASET`      | `'training'` <br> `'testing'`| Loads the training non-steady-state external variable dataset when set to `'training'`, or the testing non-steady-state external variable dataset when set to `'testing'`. Must specify a dataset to use regardless of the value of `EXTERNAL_VARIABLES_STEADY_STATE`.|
| `LOWPASS_FILTER_TUNING`           | `true` <br> `false`          | Stops the loaded organic and lean electrolyte stiction models from 'sticking' when set to `true`. This can be used to tune the loaded organic and lean electrolyte stiction lowpass filters (see below). The loaded organic and lean electrolyte stiction models work as expected when set to `false`.|
| `PROCESS_STATE`                   | see below                    | Select which process faults occur by selecting the process state. See section 1.4. Process States below.                                   |
| `PROCESS_STATE_PLOTTING_REDUCED`  | `true` <br> `false`          | Only plots the single-fault process states (`0` to `4`) when set to `true`. Plots all the process states (`0` to `8`) when set to `false`. |

### 1.3. Output folder structure

The .gitignore file has been set up to ignore all output folders.

Create the following folder structure to save the simulation results using the `SAVE_IMAGES` and `SAVE_DATA` settings:

```
.
|__ process_model
    |__ output
        |__ data
        |__ graphs
            |__ measured_variables
            |__ process_variables
            |__ valve_states
```

### 1.4. Process states

The desired fault combinations can be set using the `PROCESS_STATE` setting. The following table shows which loaded organic valve faults and which lean electrolyte valve faults occur for the selected process state.

#### Table 2: Process states
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
|      `9`      |        custom        |         custom         |

State `9` allows for custom behaviour to be defined in each control valve's `custom` variant subsystem's Stateflow chart.

The valve states are represented by the following values, and are selected using variant subsystems:

#### Table 3: Valve states
| Value | Valve State |
|:-----:|:-----------:|
|  `0`  |    normal   |
|  `1`  |    stuck    |
|  `2`  |   stiction  |

The time that the selected process faults occur can be set using the following `start_time` parameters. Corresponding `stop_time` parameters are also provided to set the time at which the process faults stop occurring. If no stop time is desired, set the respective `stop_time` parameter equal to the simulation time plus one.

#### Table 4: Process fault start and stop time settings
| Parameter                   | Units | Description                                          |
|-----------------------------|:-----:|------------------------------------------------------|
| `valve_LO_fault_start_time` |   s   | Time for the loaded organic valve's fault to occur   |
| `valve_LO_fault_stop_time`  |   s   | Time for the loaded organic valve's fault to stop    |
| `valve_LE_fault_start_time` |   s   | Time for the lean electrolyte valve's fault to occur |
| `valve_LE_fault_stop_time`  |   s   | Time for the lean electrolyte valve's fault to stop  |

### 1.5 Lowpass filter tuning

The loaded organic and lean electrolyte control valve stiction models use 1<sup>st</sup> order Butterworth lowpass filters to account for the sensor noise's effect on the valve stem positions. The valves 'stick' when the filtered valve position reaches a turning point.

The cutoff frequency of the lowpass filters can be tuned by seting the `LOWPASS_FILTER_TUNING` setting to `true`. The stiction models then still run, but the control valves will not 'stick'. The filtered signals can be viewed using the `Valve LO Lowpass Filter Scope` and `Valve LE Lowpass Filter Scope` scopes found in their respective stiction-valve-model subsytems. The process state has to be set to such a state in which the valve under consideration is experiencing stiction for the lowpass filters to be active.

The cutoff frequency for the loaded organic control valve's lowpass filter can be set using `valve_LO_stiction_lowpass_filter_cutoff_frequency` (rad/s). The cutoff frequency for the lean electrolyte control valve's lowpass filter can be set using `valve_LE_stiction_lowpass_filter_cutoff_frequency` (rad/s). 



## 2. External variables

### 2.1. Running the model

1. Run the Simulink model from the MATLAB Live Script `gaussian_external_variables.mlx`
1. Run the MATLAB Live Script from the main `Process-State-Identification` folder

### 2.2. Output folder structure

Create the following folder structure to save the smoothed external variable data and graphs:

```
.
|__ external_variables
    |__ output
        |__ data
        |__ graphs

```

### 2.3. Using the generated external variables

Copy the generated external variable .mat files over to the following folder to use them in the process model:

```
.
|__ external_variables
    |__ data

```

### 2.4. External variables used in the simulation

The following settings were used in the Signal Builder blocks in the `gaussian_external_variables.slx` file to generate the non-steady-state external variables used for the process model:

#### Table 5: External variable properties
|      Property      |                 Value                |
|:------------------:|:------------------------------------:|
|   Simulation time  |      200 hours (720 000 seconds)     |
|    Sampling time   |              1 second                |
| Standard deviation | $\sigma$ = 0.10 x steady-state value |
|  Sample frequency  |              0.00005 Hz              |

The first and last points of each signal were manually changed to start and stop on the steady-state values of the respective variable.

#### Table 6: Training external variable properties
|    Variable    |    Mean    |  Standard deviation    | Units |    Seed    |
|:--------------:|:----------:|:----------------------:|:-----:|:----------:|
|    $c_{PLS}$   |      7     |           0.7          |  g/L  |      1     |
|    $c_{LE}$    |     35     |           3.5          |  g/L  |      2     |
|   $f_{PLSP}$   |     278    |          27.8          |  L/s  |      5     |
|   $f_{PLSS}$   |     278    |          27.8          |  L/s  |     16     |

#### Table 7: Testing external variable properties
|    Variable    |    Mean    |  Standard deviation    | Units |    Seed    |
|:--------------:|:----------:|:----------------------:|:-----:|:----------:|
|    $c_{PLS}$   |      7     |           0.7          |  g/L  |     19     |
|    $c_{LE}$    |     35     |           3.5          |  g/L  |     22     |
|   $f_{PLSP}$   |     278    |          27.8          |  L/s  |     28     |
|   $f_{PLSS}$   |     278    |          27.8          |  L/s  |     30     |

## 3. Controller tuning

### 3.1. Running the model

1. Run the Simulink model from either of the following two MATLAB Live Scripts:
    1. `feedback_arx_models.mlx`
    1. `feedforward_arx_models.mlx`
1. Run the above mentioned MATLAB Live Scripts from the main `Process-State-Identification` folder

### 3.2. Output folder structure

Create the following folder structure to save the ARX model results:

```
.
|__ controller_tuning
    |__ output
        |__ feedback
        |__ feedforward
```

## 4. Fault detection

### 4.1. Running the fault detection

1. Run the fault detection MATLAB Live Script from the main `Process-State-Identification` folder

### 4.2. Fault detection settings

#### Table 8: Fault detection settings
| Setting                                 | Values                           | Description                                                                                                                                |
|-----------------------------------------|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| `SAVE_IMAGES`                           | `true` <br> `false`              | Saves the generated images to the respective output folders if set to `true`.                                                              |
| `SAVE_DATA`                             | `true` <br> `false`              | Saves the fault detection data to the respective output folder if set to `true`.                                                           |
| `NUMBER_OF_PRINCIPLE_COMPONENTS_TO_USE` | `1` to `11`                      | Specify the number of principal components to use for the reduced principal component subspace.                                            |
| `T2_ALPHA`                              | $T^{2}_{\alpha}\in\mathbb{R}>0\$ | Upper control limit (UCL) for the Hotelling's T<sup>2</sup> statistic control chart.                                                       |
| `Q_ALPHA`                               | $Q_{\alpha}\in\mathbb{R}>0$      | UCL for the Q statistic control chart.                                                                                                     |
| `NUMBER_CONSECUTIVE_ABOVE_UCL`          | $n\in{Z}\geq0$                   | Specify the number of consecutive measurements that must be above the UCL before a fault is detected.                                      |
| `PROCESS_STATE_PLOTTING_REDUCED`        | `true` <br> `false`              | Only plots the single-fault process states (`0` to `4`) when set to `true`. Plots all the process states (`0` to `8`) when set to `false`. |
| `ROC_CURVE_CONSTRUCTION_T2`             | `true` <br> `false`              | Appends the Hotelling's T<sup>2</sup> ROC curve results for the current $T^{2}_{\alpha}$ to the `ROC_CURVE_DATA_T2` table to use for constructing a Hotelling's T<sup>2</sup> ROC curve. |   
| `ROC_CURVE_CONSTRUCTION_Q`              | `true` <br> `false`              | Appends the Q statistic ROC curve results for the current $Q_{\alpha}$ to the `ROC_CURVE_DATA_Q` table to use for constructing a Q statistic ROC curve.                                  | 
| `STATE_1_START_TIME`                    | $n\in{Z}\geq0$                   | The time at which process state 1 starts in the fault detection dataset in units of seconds. Must be divisable by `SAMPLING_RATE`.         | 
| `STATE_1_STOP_TIME`                     | $n\in{Z}\geq0$                   | The time at which process state 1 stops in the fault detection dataset in units of seconds. Must be divisable by `SAMPLING_RATE`.          | 
| `STATE_2_START_TIME`                    | $n\in{Z}\geq0$                   | The time at which process state 2 starts in the fault detection dataset in units of seconds. Must be divisable by `SAMPLING_RATE`.         | 
| `STATE_2_STOP_TIME`                     | $n\in{Z}\geq0$                   | The time at which process state 2 stops in the fault detection dataset in units of seconds. Must be divisable by `SAMPLING_RATE`.          | 
| `STATE_3_START_TIME`                    | $n\in{Z}\geq0$                   | The time at which process state 3 starts in the fault detection dataset in units of seconds. Must be divisable by `SAMPLING_RATE`.         | 
| `STATE_3_STOP_TIME`                     | $n\in{Z}\geq0$                   | The time at which process state 3 stops in the fault detection dataset in units of seconds. Must be divisable by `SAMPLING_RATE`.          | 
| `STATE_4_START_TIME`                    | $n\in{Z}\geq0$                   | The time at which process state 4 starts in the fault detection dataset in units of seconds. Must be divisable by `SAMPLING_RATE`.         | 
| `STATE_4_STOP_TIME`                     | $n\in{Z}\geq0$                   | The time at which process state 4 stops in the fault detection dataset in units of seconds. Must be divisable by `SAMPLING_RATE`.          | 
| `SAMPLING_RATE`                         | $n\in{Z}\geq0$                   | The sensor sampling rate used for the data in units of seconds.                                                                            |
| `DETECTION_DELAY_CONSTRUCTION_T2`       | `true` <br> `false`              | Appends the Hotelling's T<sup>2</sup> detection delay results for the current $T^{2}_{\alpha}$ to the `DETECTION_DELAY_DATA_T2` table to use for constructing a Hotelling's T<sup>2</sup> detection delay plot. |   
| `DETECTION_DELAY_CONSTRUCTION_Q`        | `true` <br> `false`              | Appends the Q statistic detection delay results for the current $Q_{\alpha}$ to the `DETECTION_DELAY_DATA_Q` table to use for constructing a Q statistic detection delay plot.                                  | 

### 4.3. Output folder structure

Create the following folder structure to save to fault detection results using the `SAVE_IMAGES` and `SAVE_DATA` settings:

```
.
|__ fault_detection
    |__ output
        |__ data
        |__ fault_detection
        |__ PCA
        |__ training

```

## 5. ROC curves

### 5.1. Generating the ROC curves

1. Run the ROC curves MATLAB Live Script from the main `Process-State-Identification` folder

### 5.2. ROC curve settings

#### Table 9: ROC curve settings
| Setting                        | Values                                                 | Description                                                                                                                                 |
|--------------------------------|--------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| `T2_TRAINING_INDEX`            | $n\in{Z}\in[1 \ , nr \ of \ T^{2} \ ROC \ datapoints]$ | The index of the Hotelling's T<sup>2</sup> training $T^{2}_{\alpha}$ point in the Hotelling's T<sup>2</sup> ROC curve data file.            |
| `Q_TRAINING_INDEX`             | $n\in{Z}\in[1 \ , nr \ of \ Q \ ROC \ datapoints]$     | The index of the Q statistic training $Q_{\alpha}$ point in the Q statistic ROC curve data file.                                            |
| `T2_RECOMMENDED_INDEX`         | $n\in{Z}\in[1 \ , nr \ of \ T^{2} \ ROC \ datapoints]$ | The index of the Hotelling's T<sup>2</sup> recommended $T^{2}_{\alpha}$ point in the Hotelling's T<sup>2</sup> ROC curve data file.         |
| `Q_RECOMMENDED_INDEX`          | $n\in{Z}\in[1 \ , nr \ of \ Q \ ROC \ datapoints]$     | The index of the Q statistic recommended $Q_{\alpha}$ point in the Q statistic ROC curve data file.                                         |
| `T2_OPTIMAL_SENSITIVITY_INDEX` | $n\in{Z}\in[1 \ , nr \ of \ T^{2} \ ROC \ datapoints]$ | The index of the Hotelling's T<sup>2</sup> optimal sensitivity $T^{2}_{\alpha}$ point in the Hotelling's T<sup>2</sup> ROC curve data file. |
| `Q_OPTIMAL_SENSITIVITY_INDEX`  | $n\in{Z}\in[1 \ , nr \ of \ Q \ ROC \ datapoints]$     | The index of the Q statistic optimal sensitivity $Q_{\alpha}$ point in the Q statistic ROC curve data file.                                 |

### 5.3. Constructing the ROC curves

Save the `ROC_CURVE_DATA_T2` and `ROC_CURVE_DATA_Q` data tables from the fault detection as .mat files and copy them over to the following folder to use for constructing the ROC curves:

```
.
|__ roc_curves
    |__ data

```

### 5.4. Output folder structure

Create the following folder structure to save to generated ROC curves:

```
.
|__ roc_curves
    |__ output

```
