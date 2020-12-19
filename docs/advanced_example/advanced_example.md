In this example we will use fake data from 20 participants. We will assume that each participant was scanned four times (at 6, 12, 24 and 36 months of age) and that each participant is assigned to either thre control (ctrl) or case (case) group. Hence, we need a total of 80 scans:

- **between factor**: group: ctrl and case
- **within factor**: age: 06, 12, 24 and 36


## 1. path_imaging

We will assume that the connectivity matrices were made using the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/) and that there were 96 surviving scans. This means that [this file](./Zfconn_407_frames.mat) contains a 3D array made by the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/) contains connectivity matrices for 96 scans and that we need to ignore data from 16 scans. Order and ID of the participants are saved in the corresponding text file made by the [GUI environments](https://gui-environments-documentation.readthedocs.io/en/latest/GUI_environments/).

## 2. path_demographics_Table

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

## 3. path_group_Design_Table

In [this csv](./Group_Design_Table.csv) file we will define the groups as *between* or *within(repeated)* factors. This is the content of the file:

```
Variable,Design
age,within
group,between
```

## 4. path_parcellation_table

This is a [Matlab file](./parcel.mat) that has the assignment of each ROI to their corresponding network (see details in the section [Detailed specifications for input files and data](../details/detailed_description.md)).

## 5. path_Group_Color_Table

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

## 6. Defining options

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


## 7. Runing the example
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

## 8. Outputs displayed in terminal of Matlab

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

```

## Outputs saved in *output_folder*

The function will save figures and data in the **output_folder**. 

```
.
|____figures_by_network
|____main_analysis
|____planB_by_networks

```