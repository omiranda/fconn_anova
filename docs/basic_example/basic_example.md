In this example we'll use fake data from 63 participants. Data was parcellated using the ROI definition by [Bezgin et al, 2012](https://pubmed.ncbi.nlm.nih.gov/22521477/). ROIs were assigned to functional networks as proposed by [Grayson et al., 2016](https://pubmed.ncbi.nlm.nih.gov/27477019/)

## Mandatory inputs, quick overview

- [**path_imaging**](./fconn_63_scanns.mat): This input can be a path to a file containing the connectivity matrices for each participant. It can be the output file made by the  [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/).
- [**path_demographics_Table**](./table_subjects.csv): File that describes the relative position of each connectivity matrix on **path_imaging**. It also assign each participant to its corresponding group.
- [**path_group_Design_Table**](./Group_Design_Table.csv): It defines what type of variable the grouping variables as *within* or *between* factor,
- [**path_parcellation_table**](./parcel.mat): File assigning ROIs to functional networks.


## Optional inputs, quick overview
- **output_folder**: path to folder to save data. The folder is created if it does not exist. If not provided, data is saved at the folder you are in.
    

## Runing the example
```
% Mandatory inputs
path_imaging='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\fconn_63_scanns.mat';
path_demographics_Table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\table_subjects.csv';
path_group_Design_Table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\Group_Design_Table.csv';
path_parcellation_table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\parcel.mat';

% Optional input
output_folder='C:\Users\oscar\Downloads\output_fconn_anovan\basic_example';

run_fconn_anovan(path_imaging,...
    path_demographics_Table,...
    path_group_Design_Table,...
    path_parcellation_table,...
    'output_folder',output_folder);
}
```

## Outputs displayed in terminal of Matlab

The function provides some feedback on terminal

### ROI set

First you will see the network names and ROI count for the parcellation schema provided as input ([**path_parcellation_table**](./parcel.mat)).

```
1) auditory          (Aud), n =  4
2) default mode      (Def), n = 26
3) dorsal attention  (DoA), n = 10
4) insular-opercular (InO), n =  7
5) limbic            (Lmb), n = 14
6) somatomotor       (SoM), n = 13
7) visual            (Vis), n =  8
__________________________________
Total = 82 ROIs
```
## Outputs saved in *output_folder*