**fconn anova** is a package designed to identify differences in functional connectivity between groups using a **repeated measures anova**. By default, the package uses all the data to test for differences in connectivity for groups, groups and networks and connections. Then, it creates post-hoc tables. This package also does a **plan B** test, where a repeated measures anova test is run for each functional network pair.

**It works in cross-sectional and longitudinal samples**. As a prerrequisite, you need to have connectivity matrices where the Regions of Interest (ROIs) can be grouped into different functional networks, such as [Gordon](https://pubmed.ncbi.nlm.nih.gov/25316338/).


To run this analysis you need group assignment as well as connectivity matrices for each participant. This assignment is provided as a table (csv file). Connectivity matrices can be done using the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/).

Following sections show several examples, define mandatory and optional arguments and detail the outputs.

If you use it, please cite:


Miranda-Domínguez, Ó., Ragothaman, A., Hermosillo, R., Feczko, E., Morris, R., Carlson-Kuhta, P., Nutt, J. G., Mancini, M., Fair, D., & Horak, F. (2020). Lateralized Connectivity between Globus Pallidus and Motor Cortex is Associated with Freezing of Gait in Parkinson’s Disease. Neuroscience.  doi: 10.1016/j.neuroscience.2020.06.036