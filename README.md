
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

<img src="https://docs.google.com/drawings/d/e/2PACX-1vTOgQ6kYC7QYYMmH69ps83eFd1qdK70Wtz0_CSB7u30AR4q82NJr4WzkLTRdSZgX2NbO4QOTSuVX88K/pub?w=460&amp;h=300">

%%HTML 
<div class='tableauPlaceholder' id='viz1601559908930' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Fu&#47;FunctionalityofWaterPumpsUsingLatitudeandLongitude&#47;Sheet1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='FunctionalityofWaterPumpsUsingLatitudeandLongitude&#47;Sheet1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Fu&#47;FunctionalityofWaterPumpsUsingLatitudeandLongitude&#47;Sheet1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1601559908930');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>

### Scrub:
In the Scrub section of my notebook I took care of cleaning the data and observing which columns were actually needed to create my models. When reading over the column descriptions (given above) I noticed that many columns covered the exact same material. Therefore I went through and looked at null values, value_counts, and how specific each column was in order to determine which of the repetitive columns were kept. During this section I dropped scheme_name, waterpoint_type, source_type, source_class, quantity_group, quality_group, payment, date_recorded, management, extraction_type, region_code, district_code, and extraction_type_class. While that looks like a high number of columns, the columns that were left covered the same information as those that were dropped from the dataset. Once I dropped unneeded columns I dove deeper into other columns. Below I have listed other action points I took during the Scrub section of my Jupyter notebook:

> Construction year had 20,709 water pumps without a year listed. I assumed these were built before they began recording construction date and set these all to 1959. 
> Public Meeting had nulls so I went with the majority and assumed that wells were in public meeting locations. 
> Population I replaced 0 with the median of all of the water pumps. 
> To take care of the remaining nulls I replaced subvillage and scheme_management nulls with "Unknown".
> Permit had two groups True and 25.0, I assumed 25.0 meant False and switched 25.0 to read False

### Explore:
During the Explore section of my notebook I took time to create visuals in order to help me understand my remaining dataset better. Within this time I also scaled my numerial columns, I did not end up using my scaled columns since the Models I created do not call for scaling. Next, I took a look at my categorical columns. Here I realized that if I used pd.get_dummies on all my categorical columns that my data set would be way too large. Therefore I dropped additional categorical columns that either had similar information in another column, or I thought did not play a role in the functionality of the well. Example: Funder. Whomever raised the money had no role in inserting or maintaining the well so I was comfortable dropping this column. Installer column is one that I found to be very important, but there were over 100 installers listed. I made the decision to keep any installer that built over 1000 wells and to group the remainder in an "Unknown" group. The code shown below is what I used to execute this step in the process: 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vSC0pQBy393gUFIUgkmN_1K3kJRNcmpBg01bIywucHlmYomXI84gSOh1cpXSxkvIg3-0Y4ZyM5oJIxL/pub?w=652&amp;h=102">


Once taking this step I used pd.get_dummies on my categorical columns in order to make a new dataframe. The last steps I took in Explore was to perform a LabelEncoder on my dependent group and then train_test_split the dataset. 

### Model:
Looking at my dataset I thought that Classification Trees, Bagged Trees, or Random Forests would build the best model since I was looking to render feature importance on what specifically impacts the well's functionality. I began with a Vanilla Decision Tree as a baseline model. 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vSOgu6BRFRrAlQQeDtMp0k0IqksTqMZJKRrwSjXcKeof3TcZJV1Vq4H8JRfXYZuE0Z-DfuRH7Rw3F1F/pub?w=960&amp;h=720">

Following my Vanilla Decision Tree Model I used GridSearch CV in order to find the best parameters. Parameters selected were class_weight = 'balanced', max_depth = 100, min_samples_split = 2, criterion = 'entropy'. These parameters increased my macro avg precision by 1%, testing accuracy by .33%, and everything else was pretty much the same. Therefore I decided to move onto Random Forests. My Vanilla Random Forest already showed great improvement upon my Decision Tree. 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vSLVvWUq3nY8u_lcuTyTF31PYROEjBs1TmXA2WrhUzrw1IFRkEo25rlPM5eCxBXjCSl4KvzJ8g7pIdm/pub?w=689&amp;h=703">

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQriFQmu1t1dhC5XJc3KkeM86klmoNyRrQgFcCXNdHAfb-2ZiINSb-N1SklDzFn8KYRmfhvzt8rhs3F/pub?w=738&amp;h=399">

I once again chose to use GridSearchCV in order to find the best parameters for my Random Forest Model. The optimal parameters were class weight= 'balanced', criterion = 'entropy', max_depth = 70m and min_samples_split = 3.

<img src="https://docs.google.com/drawings/d/e/2PACX-1vRYLMGAJKEhZj9QCW7QFtUgVCIhsU7ZwoduLoBeHMerYPSDtuG6d3gEeicI0YG9CimyJTZzNWWG6rtN/pub?w=637&amp;h=814">

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQ4X6ThGV_ZB6r5pn5puCJE51uFCG50LY_iC5zBzhMcWEoYgMpTTEk4hRwnBZot3TCcTo9VLZlyYeyd/pub?w=745&amp;h=394">

The different metrics used to evaluate my model is 

### Interpret

To interpret my model I chose to use SHAP as a visualization technique. Below is a summary plot showing which factors impact the functionality of the wells the most. 

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQu3rUc5nH_gIUQB8IMq_VuA73W9vHtniS5n2cMcUJrVQnWHHWjSyCNll0_rGyLmtvBVzannrp0zZPE/pub?w=592&amp;h=459">

Class 0 = Functional
Class 1 = Functional Needs Repair
Class 2 = Not Functional

Based on the graph above we can see that Quantity Dry, Quantity_Enough, Extraction_type_gravity, Waterpoint_type_group_other, Extraction_type_other, and Construction Year have the greatest impact on the well's functionality. 


## Recommendations



## For Further Information
Please review the narrative of my analysis in my jupyter notebook or review my presentation. For any additional questions please contact via email at CLEsser02@gmail.com or Lauren Esser on LinkedIn
