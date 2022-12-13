<img src="assets/schematics/process_schematic.png">

# Process State Identification

<img src="https://img.shields.io/badge/Stellenbosch University-BEng ChemE-008BC0?style=flat"/> <img src="https://img.shields.io/badge/Supervisor-Dr Cripwell-008BC0?style=flat"/> <img src="https://img.shields.io/badge/Process Engineering-80%25 (Pass with Distinction)-008BC0?style=flat"/> <img src="https://img.shields.io/badge/SAIMM-Prize for the Best Final Year Mineral Processing Project-008BC0?style=flat"/>

Chemical Engineering final-year project simulating a copper solvent extraction process with valve faults using PCA and statistical classification to identify when the process enters a fault state.

The process model consists of a dynamic model of a copper solvent extraction plant based on the model developed by Komulainen et al. (2009, 2006). 
The model simulates two control valve faults, namely:

- stuck valves
- and valve stiction, based on the physical valve stiction model developed by Shoukat Choudhury et al. (2005)

## Resources

Instructions for running the process model, fitting the ARX models, tuning the feedback and feedforward controllers, simulating the process states, performing the statistical process control, detecting the simulated control valve faults, and quantifying the performance of the fault detection classifiers are available in [intructions.md](instructions.md).

The results of the project are available in the [results](results) folder. The simulation and fault detection data are provided in .csv files, and the generated graphs produced and used throughout and for the project are available.

## References

Komulainen, T., Doyle, F.J., Rantala, A., Jämsä-Jounela, S.L., 2009. Control of an industrial copper solvent extraction process. Journal of Process Control 19, 2–15. https://doi.org/10.1016/j.jprocont.2008.04.019

Komulainen, T., Pekkala, P., Rantala, A., Jämsä-Jounela, S.L., 2006. Dynamic modelling of an industrial copper solvent extraction process. Hydrometallurgy 81, 52–61. https://doi.org/10.1016/j.hydromet.2005.11.001

Shoukat Choudhury, M.A.A., Thornhill, N.F., Shah, S.L., 2005. Modelling Valve Stiction. Control Eng Pract 13, 641–658. https://doi.org/10.1016/j.conengprac.2004.05.005


