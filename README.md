
# Maintaining Functioning Wells in Tanzania
**Author**: Lauren Esser

The contents of this repository detail an analysis of the module three project. This analysis is detailed in hopes of making the work accessible and replicable. 


## Business Problem
Using data from Taarifa and the Tanzanian Ministry of Water I created a model that uses given factors in order to predict which pumps are functional, which need repair, and which pumps are no longer working. By creating this model Tanzanian Ministry of Water may use it to help keep potable water available in their communities. 

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

## Methods
### Obtain:
Data was obtained from Taarifa and the Tanzanian Ministry of Water. In the Obtain section of my notebook I uploaded two csv's of data using Pandas. I then took time to observe the number of unique groups per data frame and inspect information. The data labels table listed the id and status_group of the wells. (Functional, Functional, Needs Repair, Non-Functional. The data values table had 49 different columns containing 3 floats, 7 int, and 30 object columns. 

### Scrub:
In the Scrub section of my notebook I took care of cleaning the data and observing which columns were actually needed to create my models. When reading over the column descriptions (given above) I noticed that many columns covered the exact same material. Therefore I went through and looked at null values, value_counts, and how specific each column was in order to determine which of the repetitive columns were kept. During this section I dropped scheme_name, waterpoint_type, source_type, source_class, quantity_group, quality_group, payment, date_recorded, management, extraction_type, region_code, district_code, and extraction_type_class. While that looks like a high number of columns, the columns that were left covered the same information as those that were dropped from the dataset. Once I dropped unneeded columns I dove deeper into other columns. Below I have listed other action points I took during the Scrub section of my Jupyter notebook:

> Construction year had 20,709 water pumps without a year listed. I assumed these were built before they began recording construction date and set these all to 1959. 
> Public Meeting had nulls so I went with the majority and assumed that wells were in public meeting locations. 
> Population I replaced 0 with the median of all of the water pumps. 
> To take care of the remaining nulls I replaced subvillage and scheme_management nulls with "Unknown".
> Permit had two groups True and 25.0, I assumed 25.0 meant False and switched 25.0 to read False

### Explore:
During the Explore section of my notebook I took time to create visuals in order to help me understand my remaining dataset better. Within this time I also scaled my numerial columns, I did not end up using my scaled columns since the Models I created do not call for scaling. Next, I took a look at my categorical columns. Here I realized that if I used pd.get_dummies on all my categorical columns that my data set would be way too large. Therefore I dropped additional categorical columns that I either had similar information in another column, or I thought did not play a role in the functionality of the well. Example: Funder. Whomever raised the money had no role in inserting or maintaining the well so I was comfortable dropping this column. Installer column is one that I found to be very important, but there were over 100 installers listed. I made the decision to keep any installer that built over 1000 wells and to group the remainder in an "Unknown" group. Once taking this step I used pd.get_dummies on my categorical columns in order to make a new dataframe. The last steps I took in Explore was to perform a LabelEncoder on my dependent group and then train_test_split the dataset. 

### Model:

### Interpret









## The Deliverables

For online students, your completed project should contain the following four deliverables:

1. A **_Jupyter Notebook_** containing any code you've written for this project. This work will need to be pushed to a public GitHub repository dedicated for this project.

2. An organized **README.md** file in the GitHub repository that describes the contents of the repository. This file should be the source of information for navigating through the repository. 


4. An **_"Executive Summary" PowerPoint Presentation_** that gives a brief overview of your problem/dataset, and each step of the OSEMN process.


### Jupyter Notebook Must-Haves

### 1. Business Understanding

Start by reading this document, and making sure that you understand the kinds of questions being asked.  In order to narrow your focus, you will likely want to make some design choices about your specific audience, rather than addressing all of the "many people" mentioned in the background section.  Do you want to emphasize affordability, investment, or something else?  This framing will help you choose which stakeholder claims to address.

Three things to be sure you establish during this phase are:

1. **Objectives:** what questions are you trying to answer, and for whom?
2. **Project plan:** you may want to establish more formal project management practices, such as daily stand-ups or using a Trello board, to plan the time you have remaining.  Regardless you should determine the division of labor, communication expectations, and timeline.
3. **Success criteria:** what does a successful project look like?  How will you know when you have achieved it?

### 2. Data Understanding

Write a script to download the data (or instructions for future users on how to manually download it), and explore it.  Do you understand what the columns mean?  How do the three data tables relate to each other?  How will you select the subset of relevant data?  What kind of data cleaning is required?

It may be useful to generate visualizations of the data during this phase.

### 3. Data Preparation

Through SQL and Pandas, perform any necessary data cleaning and develop a query that pulls in all relevant data for analysis in a linear regression model, including any merging of tables.  Be sure to document any data that you choose to drop or otherwise exclude.  This is also the phase to consider any feature scaling or one-hot encoding required to feed the data into a classification model.

### 4. Modeling

The focus this time is on prediction. Good prediction is a matter of the model generalizing well. Steps we can take to assure good generalization include: testing the model on unseen data, cross-validation, and regularization. What sort of model should you build? A diverse portfolio is probably best. Classification models we've looked at so far include logistic regression, decision trees, bagging, and boosting, each of these with different flavors. You are encouraged to try any or all of these.

### 5. Evaluation

Recall that there are many different metrics we might use for evaluating a classification model. Accuracy is intuitive, but can be misleading, especially if you have class imbalances in your target. Perhaps, depending on you're defining things, it is more important to minimize false positives, or false negatives. It might therefore be more appropriate to focus on precision or recall. You might also calculate the AUC-ROC to measure your model's *discrimination*.

### 6. Deployment

In this case, your "deployment" comes in the form of the deliverables listed above. Make sure you can answer the following questions about your process:

 - "How did you pick the question(s) that you did?"
 - "Why are these questions important from a business perspective?"
 - "How did you decide on the data cleaning options you performed?"
 - "Why did you choose a given method or library?"
 - "Why did you select those visualizations and what did you learn from each of them?"
 - "Why did you pick those features as predictors?"
 - "How would you interpret the results?"
 - "How confident are you in the predictive quality of the results?"
 - "What are some of the things that could cause the results to be wrong?"

