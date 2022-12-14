<img src="assets/schematics/process_schematic.png">

# Process State Identification

<img src="https://img.shields.io/badge/Stellenbosch University-BEng ChemE-008BC0?style=flat"/> <img src="https://img.shields.io/badge/Supervisor-Dr Cripwell-008BC0?style=flat"/> <img src="https://img.shields.io/badge/Process Engineering-80%25 (Pass with Distinction)-008BC0?style=flat"/> <img src="https://img.shields.io/badge/SAIMM-Prize for the Best Final Year Mineral Processing Project-008BC0?style=flat"/>

Chemical Engineering final-year project simulating a copper solvent extraction process with valve faults using PCA and statistical classification to identify when the process enters a fault state.

The process model consists of a dynamic model of a copper solvent extraction plant based on the model developed by Komulainen et al. (2009, 2006). 
The model simulates two control valve faults, namely:

- stuck valves
- and valve stiction, based on the physical valve stiction model developed by Shoukat Choudhury et al. (2005)

## Resources

- [Intructions.md](instructions.md) contains instructions for running the process model, fitting the ARX models, tuning the feedback and feedforward controllers, simulating the process states, performing the statistical process control, detecting the simulated control valve faults, and quantifying the performance of the fault detection classifiers.

- The [results](results) folder contains the simulation and fault detection data in .csv format, and the generated graphs produced throughout the project are available.

## Results

<img src="results/roc_curves/faults_training/recommended_UCLs/roc_curve_smooth_operating.png" />

Figure 1: ROC curves for the Hotelling's T<sup>2</sup> statistic and Q statistic control charts for the simulated process faults dataset with the left dark squares indicating the trained upper control limits, the right dark squares indicating the optimal sensitivity upper control limits, and the recommended upper control limits indicated by the dark triangles

Table 1 provides the trained, recommended, and optimal sensitivity T<sup>2</sup><sub>α</sub> and Q<sub>α</sub> upper control limits. The associated combined fault detection’s senstivity and 1 - specificity for the different upper control limits are also given.

Table 1: Combined fault detection’s sensitivity and 1 - specificity for the different upper control limits

| Upper control limits       | T<sup>2</sup><sub>α</sub> | Q<sub>α</sub> |  Sensitivity  | 1 - Specificity |
|----------------------------|:-------------------------:|:-------------:|:-------------:|:---------------:|
|     Trained                |             16            |        3      |     0.5974    |      0.0732     |
|     Recommended            |             12            |        2      |     0.6528    |      0.0830     |
|     Optimal sensitivity    |              8            |        1      |     0.7805    |      0.2043     |

## References

Komulainen, T., Doyle, F.J., Rantala, A., Jämsä-Jounela, S.L., 2009. Control of an industrial copper solvent extraction process. Journal of Process Control 19, 2–15. https://doi.org/10.1016/j.jprocont.2008.04.019

Komulainen, T., Pekkala, P., Rantala, A., Jämsä-Jounela, S.L., 2006. Dynamic modelling of an industrial copper solvent extraction process. Hydrometallurgy 81, 52–61. https://doi.org/10.1016/j.hydromet.2005.11.001

Shoukat Choudhury, M.A.A., Thornhill, N.F., Shah, S.L., 2005. Modelling Valve Stiction. Control Eng Pract 13, 641–658. https://doi.org/10.1016/j.conengprac.2004.05.005


