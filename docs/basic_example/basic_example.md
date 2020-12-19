In this example we'll use fake data from 63 participants. Data was parcellated using the ROI definition provided by [Bezgin et al, 2012](https://pubmed.ncbi.nlm.nih.gov/22521477/) and the ROIs were assigned to functional networks as defined by [Grayson et al., 2016](https://pubmed.ncbi.nlm.nih.gov/27477019/)



```
fconn_path='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\fconn_63_scanns.mat';
path_imaging=fconn_path;
path_demographics_Table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\table_subjects.csv';
path_group_Design_Table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\Group_Design_Table.csv';
path_parcellation_table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\parcel.mat';
output_folder='C:\Users\oscar\Downloads\output_fconn_anovan\basic_example';

run_fconn_anovan(path_imaging,path_demographics_Table,path_group_Design_Table,path_parcellation_table,...
    'output_folder',output_folder);
}
```