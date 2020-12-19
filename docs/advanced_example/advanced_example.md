In this example we will use fake data from 20 participants. We will assume that each participant was scanned four times (at 6, 12, 24 and 36 months of age) and that each participant is assigned to either thre control (ctrl) or case (case) group. Hence, we need a total of 80 scans:

- **between factor**: group: ctrl and case
- **within factor**: age: 06, 12, 24 and 36

## 1. Preparing the inputs


### path_imaging

We will assume that the connectivity matrices were made using the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/) and that there were 96 surviving scans. This means that [this file](./Zfconn_407_frames.mat) contains a 3D array made by the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/) contains connectivity matrices for 96 scans and that we need to ignore data from 16 scans. Order and ID of the participants are saved in the corresponding text file made by the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/).

### path_demographics_Table

In [this csv](./subjects_table.csv) file we will make group assignment and also indicate which scan to include (see details in the section [Detailed specifications for input files and data](../details/detailed_description.md)). This file has 4 columns with the following headers:

- **id**: This column indicate the subject id.
- **consecutive_number**: Here you specify the relative position of the corresponding coonnectivity matrix, as stored in **path_imaging**.
- **age**: Here we decided to name one of the factors **age**. Notice that you can use the name that you want. Corresponding row indicates the age of the participant when the scan was acquired. Noticed that this variable is *text*. The analysis will assume that this variable is categorical. This is a *within* factor and will be declared as such in the **path_group_Design_Table**.
- **group**: Here we decided to name one of the factors **group**. Notice that you can use the name that you want. Corresponding row indicates the treatment (ctrl or case). Noticed that this variable is *text*. The analysis will assume that this variable is categorical. This is a *between* factor and will be declared as such in the **path_group_Design_Table**.

Here is a preview of a few rows (notice that connectivity matrices 7-10 from the file [path_imaging](./Zfconn_407_frames.mat) will be ignored):

```
id,consecutive_number,age,group
sub-MMU45938,1,12m,case
sub-MMU45938,2,24m,case
sub-MMU45938,3,36m,case
sub-MMU45938,4,06m,case
sub-MMU45967,11,12m,case
```

### path_group_Design_Table

In [this csv](./Group_Design_Table.csv) file we will define the groups as *between* or *within(repeated)* factors. This is the content of the file:

```
Variable,Design
age,within
group,between
```

### path_parcellation_table

This is a [Matlab file](./parcel.mat) that has the assignment of each ROI to their corresponding network (see details in the section [Detailed specifications for input files and data](../details/detailed_description.md)).

### path_Group_Color_Table

In [this optional csv](./Group_Design_Table.csv) file we will define the colors for each subgroup. Notice that the R, G and B components are provided in the range of 0 to 1. You might want to use a colorblind palette. Here is a couple of resources to identify colors:

- [Color Brewer](https://colorbrewer2.org/)
- [Color Universal Design](https://jfly.uni-koeln.de/color/)


This is the content of this file:
```
subgroup,R,G,B
ctrl,0.874509804,0.639215686,0.788235294
case,0.631372549,0.843137255,0.415686275
06m,0.992156863,0.552941176,0.235294118
12m,0.988235294,0.305882353,0.164705882
24m,0.890196078,0.101960784,0.109803922
36m,0.694117647,0,0.149019608
```

### Defining options

In [this optional matlab file](./define_options.m) you can modify default options for the analysis. See details in the section [Advanced options](../advanced_usage/advanced_usage.md). This is the content of the file used here:

```
%% Define options for repeated measures anova n

% Correction for multiple comparisons
% correction_type:
% 1}='tukey-kramer';
% 2}='dunn-sidak';
% 3}='bonferroni';
% 4}='scheffe';
% 5}='lsd';
options.correction_type=2;

% Here you define if you want to make the analysis on a few networks or
% not. It acceots as input a vector with the functional networks you want
% to include. IF not provided, it uses all the available networks. You can
% also us as input '[]' to use all the networks
% options.resort_parcel_order=[];
options.resort_parcel_order=[5 6 7];
options.resort_parcel_order=[];

% Apply Fisher_Z_transform to connectivity values. Use 1 or 0
options.calculate_Fisher_Z_transform=0;

% Apply an optimized box_cox transform to the data.
options.boxcox_transform=0;

options.save_figures=1;
options.display_figures=1;
% options.plot_uncorrected_NN_other_factor=1;

% p-value threshold used for visualization
options.p_th=[0.05];

% show numerical scale for connectivity values (y-axis)
options.show_y_scale=1;

% options.filename_to_save_all_before_plotting
options.filename_to_save_all_before_plotting='all_results';
```


## 2. Runing the example
```
% Mandatory inputs
path_imaging='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Example_4_time_points\Zfconn_407_frames.mat';
path_demographics_Table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Example_4_time_points\subjects_table.csv';
path_group_Design_Table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Example_4_time_points\Group_Design_Table.csv';
path_parcellation_table='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Example_4_time_points\parcel.mat';

% Optional inputs
path_Group_Color_Table ='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Example_4_time_points\Group_Color_Table.csv';
path_options='C:\Users\oscar\OneDrive\matlab_code\fconn_stats\fconn_anova\readme\Data\Example_4_time_points\define_options.m';
output_folder='C:\Users\oscar\Downloads\output_fconn_anovan\Example_4_time_points';

% Do it!
run_fconn_anovan(path_imaging,...
    path_demographics_Table,...
    path_group_Design_Table,...
    path_parcellation_table,...
    'path_Group_Color_Table',path_Group_Color_Table,...
    'options',path_options,...
    'output_folder',output_folder);
```

## 3. Outputs displayed in terminal of Matlab

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

                         correction_type: 'dunn-sidak'
                     resort_parcel_order: [1 2 3 4 5 6 7]
            calculate_Fisher_Z_transform: 0
                        boxcox_transform: 0
                            save_figures: 1
                         display_figures: 1
                                    p_th: 0.0500
                            show_y_scale: 1
    filename_to_save_all_before_plotting: 'all_results'
                              ix_sorting: [82×1 double]
        plot_uncorrected_NN_other_factor: 0
                            show_p_value: 1
                       is_connectotyping: 0
                        avoid_main_table: 1
                         use_half_matrix: 0

```

### Test being executed

Then, you will see the test being executed:

```
Executing rm = fitrm(t,'x1-x13284 ~ group', 'WithinDesign',within_plus_conn);
Mauchly's test for sphericity 

tbl =

  1×4 table

    W    ChiStat        DF        pValue
    _    _______    __________    ______

    0     -Inf      8.8226e+07      1   
```

### Repeated Measures ANOVA table

When completed, you will see the main anova table

```
ranovatbl =

  18×8 table

                                SumSq         DF         MeanSq       F         pValue        pValueGG      pValueHF      pValueLB 
                                ______    __________    ________    ______    ___________    __________    __________    __________

    (Intercept)                 154.58             1      154.58    131.45     2.0147e-09    2.0147e-09    2.0147e-09    2.0147e-09
    group                        5.674             1       5.674     4.825       0.042207      0.042207      0.042207      0.042207
    Error                       19.991            17       1.176                                                                   
    (Intercept):Networks        4049.9            27         150    125.05    1.3962e-192    1.3942e-25    7.4862e-32    2.9371e-09
    group:Networks              47.796            27      1.7702    1.4758       0.060241       0.22839       0.21682       0.24103
    Error(Networks)             550.55           459      1.1995                                                                   
    (Intercept):age             30.659             3       10.22    19.029     2.0536e-08    5.0957e-08    2.0536e-08    0.00042429
    group:age                   1.8928             3     0.63094    1.1748        0.32852        0.3278       0.32852       0.29356
    Error(age)                  27.391            51     0.53707                                                                   
    (Intercept):conn             10244          3320      3.0855    49.839              0    1.1353e-20    1.3043e-27    1.9169e-06
    group:conn                  212.08          3320    0.063879    1.0318         0.1051       0.39942       0.40861       0.32397
    Error(conn)                 3494.2         56440     0.06191                                                                   
    (Intercept):Networks:age     345.4            81      4.2642    7.6076     6.5639e-66    6.1842e-09    7.9828e-17      0.013435
    group:Networks:age          61.064            81     0.75388     1.345       0.024851       0.22078       0.15512       0.26219
    Error(Networks:age)         771.83          1377     0.56051                                                                   
    (Intercept):age:conn        1075.1          9960     0.10794    3.2403              0      0.001145    3.4601e-06       0.08962
    group:age:conn              442.89          9960    0.044467    1.3348     2.2763e-95       0.22188       0.14973       0.26393
    Error(age:conn)             5640.6    1.6932e+05    0.033313                                                                   

```

## 4. Outputs saved in *output_folder*

The function will save figures and data in the **output_folder**. 

```
.
|____main_analysis
|____planB_by_networks
|____figures_by_network
```

- **main_analysis**: Tables and figures for the main analysis, ie, using all the connectivity values. Figures are saved in the formats png, tif and fig. Fig is a Matlab file format that allows open the figure in Matlab in a different system. You might want to use this format if you run the analysis in a linux-like machine but you prefered the rendering provided by Matlab running on a Mac or Windows computer. Them you can just open and print/export the figure from the prefered computer.
- **planB_by_networks**: Tables and figures for the repeated measures anova calculated for each functional system pair.
- **figures_by_network**: Unipanel figures for each functional system pair.