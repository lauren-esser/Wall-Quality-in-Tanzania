
# Maintaining Functioning Wells in Tanzania
**Author**: Lauren Esser

The contents of this repository detail an analysis of the module three project. This analysis is detailed in hopes of making the work accessible and replicable. 


---

## Business Problem
Using data from Taarifa and the Tanzanian Ministry of Water I created a model that uses given factors in order to predict which pumps are functional, which need repair, and which pumps are no longer working. By creating this model Tanzanian Ministry of Water may use it to help keep potable water available in their communities. The importance of having potable water for these Tanzanian communities is that it decreases diseases, increases education, and increases the economy overall.

<img src= "http://drivendata.materials.s3.amazonaws.com/pumps/pumping.jpg"> 

---


## Data
> Information taken from drivendata.org. Click [here](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/25/#features_list) for access

**Summary of each column**
- amount_tsh: Total static head (amount water available to waterpoint)
- date_recorded: The date the row was entered
- funder: Who funded the well
- gps_height: Altitude of the well
- installer: Organization that installed the well
- longitude: GPS coordinate
- latitude: GPS coordinate
- wpt_name: Name of the waterpoint if there is one
- num_private: *no information*
- basin: Geographic water basin
- subvillage: Geographic location
- region: Geographic location
- region_code: Geographic location (coded)
- district_code: Geographic location (coded)
- lga: Geographic location
- ward: Geographic location
- population: Population around the well
- public_meeting: True/False
- recorded_by: Group entering this row of data
- scheme_management: Who operates the waterpoint
- scheme_name: Who operates the waterpoint
- permit: If the waterpoint is permitted
- construction_year: Year the waterpoint was constructed
- extraction_type: The kind of extraction the waterpoint uses
- extraction_type_group: The kind of extraction the waterpoint uses
- extraction_type_class: The kind of extraction the waterpoint uses
- management: How the waterpoint is managed
- management_group: How the waterpoint is managed
- payment: What the water costs
- payment_type: What the water costs
- water_quality: The quality of the water
- quality_group: The quality of the water
- quantity: The quantity of water
- quantity_group: The quantity of water
- source: The source of the water
- source_type: The source of the water
- source_class: The source of the water
- waterpoint_type: The kind of waterpoint
- waterpoint_type_group: The kind of waterpoint

---

## Methods

### Obtain:
Data was obtained from Taarifa and the Tanzanian Ministry of Water. Found [here](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/23/) In the Obtain section of my notebook I uploaded Training Set Labels and Training Set Values csv using Pandas. I then took time to observe the number of unique groups per data frame and inspect information. The data labels table listed the id and status_group of the wells (Functional, Functional, Needs Repair, Non-Functional). The data values table had 49 different columns containing 3 floats, 7 int, and 30 object columns. 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vT7eYVQHqxnYlx-nvteaIlhevc0QXKLL2KogU4MuJqMCdgNVY6sUZUGEKF77Uw3hc24cUGYo4TqCONF/pub?w=860&amp;h=576">


%%HTML 
<div class='tableauPlaceholder' id='viz1601559908930' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Fu&#47;FunctionalityofWaterPumpsUsingLatitudeandLongitude&#47;Sheet1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='FunctionalityofWaterPumpsUsingLatitudeandLongitude&#47;Sheet1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Fu&#47;FunctionalityofWaterPumpsUsingLatitudeandLongitude&#47;Sheet1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en' /><param name='filter' value='publish=yes' /></object></div>


### Scrub:
In the Scrub section of my notebook I took care of cleaning the data and observing which columns were actually needed to create my models. When reading over the column descriptions (given above) I noticed that many columns covered the exact same material. Therefore I went through and looked at null values, value_counts, and how specific each column was in order to determine which of the repetitive columns were kept. During this section I dropped scheme_name, waterpoint_type, source_type, source_class, quantity_group, quality_group, payment, date_recorded, management, extraction_type, region_code, district_code, and extraction_type_class. While that looks like a high number of columns, the columns that were left covered the same information as those that were dropped from the dataset. Once I dropped unneeded columns I dove deeper into other columns. Below I have listed other action points I took during the Scrub section of my Jupyter notebook:

- Construction year had 20,709 water pumps without a year listed. I assumed these were built before they began recording construction date and set these all to 1959. 

- Public Meeting had nulls so I went with the majority and assumed that wells were in public meeting locations. 

- Population I replaced 0 with the median of all of the water pumps. 

- To take care of the remaining nulls I replaced subvillage and scheme_management nulls with "Unknown".

- Permit had two groups True and 25.0, I assumed 25.0 meant False and switched 25.0 to read False

### Explore:
During the Explore section of my notebook I took time to create visuals in order to help me understand my remaining dataset better. Within this time I also scaled my numerial columns, I did not end up using my scaled columns since the Models I created do not call for scaling. Next, I took a look at my categorical columns. Here I realized that if I used pd.get_dummies on all my categorical columns that my data set would be way too large. Therefore I dropped additional categorical columns that either had similar information in another column, or I thought did not play a role in the functionality of the well. Example: Funder. Whomever raised the money had no role in inserting or maintaining the well so I was comfortable dropping this column. Installer column is one that I found to be very important, but there were over 100 installers listed. I made the decision to keep any installer that built over 1000 wells and to group the remainder in an "Unknown" group. The code shown below is what I used to execute this step in the process: 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vSC0pQBy393gUFIUgkmN_1K3kJRNcmpBg01bIywucHlmYomXI84gSOh1cpXSxkvIg3-0Y4ZyM5oJIxL/pub?w=652&amp;h=102">


Once taking this step I used pd.get_dummies on my categorical columns in order to make a new dataframe. The last steps I took in Explore was to perform a LabelEncoder on my dependent group and then train_test_split the dataset. 

### Model:
Looking at my dataset I thought that Classification Trees, Bagged Trees, or Random Forests would build the best model since I was looking to render feature importance on what specifically impacts the well's functionality. I began with a Vanilla Decision Tree as a baseline model. 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vRU9oOnbbAdQS3jns-DJlnw3PVIjHj0nd67_q9caGVd9as_MmEXS0YKsDUxIsSiHCSXaOqubLsyd90n/pub?w=623&amp;h=271">

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQOdzM_ckDYX_qVIcuA_iHhWEEh1a2F-AAAe_ilYfFh8rah3QHBoI-wg7AacDzgr2rtkkExe9sUGrmc/pub?w=621&amp;h=614">

<img src="https://docs.google.com/drawings/d/e/2PACX-1vR6tJJ0JbUaCXloblQHvSY3M_NJOrmqNIaxqNRyuaImw-saQuTE8f7a-laVTMVlAuCmLV7UBblYwEYf/pub?w=775&amp;h=461">


Following my Vanilla Decision Tree Model I used GridSearch CV in order to find the best parameters. Parameters selected were class_weight = 'balanced', max_depth = 100, min_samples_split = 2, criterion = 'entropy'. These parameters increased my macro avg precision by 1%, testing accuracy by .33%, and everything else was pretty much the same. Therefore I decided to move onto Random Forests. My Vanilla Random Forest already showed great improvement upon my Decision Tree. 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vSm9jCI_5RJ8Tdr2TsKWgdxj5zLpMdggcSBxNaUyu_zH2vYPxn4oHwVuI9_UqBnqvzCC_-cALYSumgQ/pub?w=625&amp;h=267">

<img src="https://docs.google.com/drawings/d/e/2PACX-1vRa8estzywcxfBWg2lP2_CcRBhq_aIRjNLuZF1j_lkBsSa6phrrHIHknedBK5J9escz_wCAgxurJbuI/pub?w=648&amp;h=645">

<img src="https://docs.google.com/drawings/d/e/2PACX-1vSzPJnGB-fAbACotRouFvwheKUJyRvIaT0ADxLo4XrrfJ3PDcQW8m2hSsCkyb6Lhwh0JWXCwGZsSGeA/pub?w=780&amp;h=449">

I once again chose to use GridSearchCV in order to find the best parameters for my Random Forest Model. The optimal parameters were class weight= 'balanced', criterion = 'entropy', max_depth = 70m and min_samples_split = 3.

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQaJCY85luPHIQ-kcVBTwgjO3svrYkOlsEi239TJmad-oYlcuf9VWonRdzw-PbuJwwtjqYDo_x21ED7/pub?w=638&amp;h=269">

<img src="https://docs.google.com/drawings/d/e/2PACX-1vTZHJ2AtAIAThHXGprCGFDhsP9qK0zGKpD5EDVWsyzl5CQBlWWUM9zu0__-mtDH6wrXIh2dF0RnJpAR/pub?w=632&amp;h=624">

<img src="https://docs.google.com/drawings/d/e/2PACX-1vTMZ8wfN26i945RZ4y9CkOyjM0j5hkjleFzBef-VTjmZUzGi66H9--2ENuflCEd_nJjT6zTJpAHto3d/pub?w=779&amp;h=406">


### Interpret

To interpret my model I chose to use SHAP as a visualization technique. Below is a summary plot showing which factors impact the functionality of the wells the most. 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vTf7Svn_80_5yN4ADwc9dvFHmrOEVpNnBXaDi8Q6oFIbkt94f4O21cHHEpGqLFoQA8aiKa_I-4F64q5/pub?w=733&amp;h=570">

Class 0 = Functional

Class 1 = Functional Needs Repair

Class 2 = Not Functional

Based on the graph above we can see that Quantity Dry, Quantity_Enough, Extraction_type_gravity, Waterpoint_type_group_other, Extraction_type_other, and Construction Year have the greatest impact on the well's functionality. 


## Recommendations
1. Ensure water pumps have access to a water source. (Could insert water gauge into pumps.)

2. Check wells that use extractino type of Gravity, Nira, or are labeled as "Other".

3. Check wells that use waterpoint type of Handpump, Standpipe, or are labeled as "Other".

4. Inspect pumps that are built before 1965 or have no construction year listed.


## For Further Information
Please review the narrative of my analysis in my jupyter notebook or review my presentation. For any additional questions please contact via email at CLEsser02@gmail.com or Lauren Esser on LinkedIn
