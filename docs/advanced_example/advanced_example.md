In this example we will use fake data from 20 participants. We will assume that each participant was scanned four times (at 6, 12, 24 and 36 months of age) and that each participant is assigned to either thre control (ctrl) or case (case) group. Hence, we need a total of 80 scans.
- **between factor**: group: ctrl and case
- **within factor**: age: 06, 12, 24 and 36


## 1. **path_imaging**

We will assume that the connectivity matrices were made using the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/) and that there were 96 surviving scans. This means that the 3D array made by the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/) contains connectivity matrices for 96 scans and that we need to ignore data from 16 scans. Order and ID of the participants are saved in the corresponding text file made by the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/).

## 2. path_demographics_Table

In this csv file we will make group assignment and also indicate which scan to include (see details in the section [Detailed specifications for input files and data](../details/detailed_description.md)). This file has 4 columns with the following headers:

- **id**: This column indicate the subject id.
- **consecutive_number**: Here you specify the relative position of the corresponding coonnectivity matrix as stored in **path_imaging**.
- **age**: Here we decided to name one of the factors **age**. Notice that you can use the name that you want. Corresponding row indicates the age of the participant when the scan was acquired. Noticed that this variable is *text*. The analysis will assume that this variable is categorical.
- **group**: Here

Here we show a few rows 
```
id,consecutive_number,age,group
sub-MMU45938,1,12m,case
sub-MMU45938,2,24m,case
sub-MMU45938,3,36m,case
sub-MMU45938,4,06m,case
sub-MMU45967,11,12m,case
```

## 3. path_group_Design_Table

## 4. path_Group_Color_Table

## 5. Defining options

## 6. Runing the example
```
% Mandatory inputs
path_imaging='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\fconn_63_scanns.mat';
path_demographics_Table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\table_subjects.csv';
path_group_Design_Table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\Group_Design_Table.csv';
path_parcellation_table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Basic_example\parcel.mat';

% Optional input
output_folder='C:\Users\oscar\Downloads\output_fconn_anovan\basic_example';

% Actual work
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

### Options
Next, you will see the options used for the analysis:

```
options = 

  struct with fields:

                 resort_parcel_order: [1×7 double]
                          ix_sorting: [82×1 double]
        calculate_Fisher_Z_transform: 0
                    boxcox_transform: 0
                     correction_type: 'tukey-kramer'
                        save_figures: 1
                     display_figures: 1
    plot_uncorrected_NN_other_factor: 0
                                p_th: 0.0500
                        show_y_scale: 0
                        show_p_value: 1
                   is_connectotyping: 0
                    avoid_main_table: 1
                     use_half_matrix: 0
```

### Test being executed

Then, you will see the test being executed:

```
Executing rm = fitrm(t,'x1-x3321 ~ Housing', 'WithinDesign',within_plus_conn);
Mauchly's test for sphericity 


tbl =

  1×4 table

    W    ChiStat        DF        pValue
    _    _______    __________    ______

    0     -Inf      5.5129e+06      1   

```

### Repeated Measures ANOVA table

When completed, you will see the main anova table

```

                             SumSq        DF         MeanSq        F         pValue        pValueGG      pValueHF      pValueLB 
                            _______    _________    _________    ______    ___________    __________    __________    __________

    (Intercept)              1.0812            1       1.0812    25.872     3.8504e-06    3.8504e-06    3.8504e-06    3.8504e-06
    Housing                 0.70258            2      0.35129    8.4058     0.00060499    0.00060499    0.00060499    0.00060499
    Error                    2.5075           60     0.041791                                                                   
    (Intercept):Networks      5.665           27      0.20982    33.171    5.7264e-134     1.924e-50    1.4525e-60    3.0868e-07
    Housing:Networks        0.70867           54     0.013124    2.0748     1.0394e-05     0.0043155     0.0020329        0.1345
    Error(Networks)          10.247         1620    0.0063252                                                                   
    (Intercept):conn         44.087         3320     0.013279    3.1201              0    1.7244e-11    5.5522e-42       0.08242
    Housing:conn             29.238         6640    0.0044033    1.0346       0.025955       0.39207       0.31223       0.36162
    Error(conn)               847.8    1.992e+05     0.004256                                                                   

```

## Outputs saved in *output_folder*

The function will save figures and data in the **output_folder**. 

```
.
|____figures_by_network
|____main_analysis
|____planB_by_networks

```