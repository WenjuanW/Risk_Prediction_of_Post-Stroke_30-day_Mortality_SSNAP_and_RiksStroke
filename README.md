# Risk_Prediction_of_Post-Stroke_30-day_Mortality_SSNAP_and_RiksStroke
This repository publishes the models for risk predictions of 30-day mortality after stroke using machine learning and statistical methods. The models were developed and validated with SSNAP (Stroke registry in UK) and externally validated with RiksStroke (Stroke registry in Sweden).


## From manuscript: Developing and externally validating a generalisable risk prediction model for 30-day mortality after stroke: a UK and Sweden based multicountry registry study

## Overview

* This repository provides pre-trained models for future studies to externally validate the trained models in a paper
* Please get in touch if you would like to collaborate on externally validate this post-stroke 30-day mortality prediction model
   + Email: wenjuan.wang@kcl.ac.uk
* If you use code/trained models from this repository, please cite the paper as a condition of use.

## About the models

The file External_validation_with_trained_models.ipynb shows how to load the trained models and use it for predictions. The models are: Logistic Regression (LR), LR with elastic net and interaction terms, and XGBoost.

## How to use this repository

1. Prepare your validation dataset according to the below specification;
2. Run External_validation_with_trained_models.ipynb using your own validation dataset;
3. We would appreciate if you emial the results to wenjuan.wang@kcl.ac.uk.

### Measures needed to validate these models

#### Outcomes

* The outcome is in-hospital 30-day mortality after stroke;
* Each outcome is coded as 1 if the patient died within 30 days in hospital after stroke;
* In the SSNAP sample (n=358588), the event rate was: 12.4%.

#### Required Variables

The 13 required variables, including the name, coding of the variables are listed below


| Variables/features         |  dataset column names | Measurements | Coding |
|:--------------|:---------------|:----------------------|:----------------- |
|         Age              | Age_Groups_by5              |      band by 5 from age 15 to age 125     |  levels: 0-20      |
|               Sex        | Male                        |   Female and Male                |   0-Female, 1-Male           |
|Inpatient at time of stroke |Inpatient_at_time_of_stroke |         Yes or No                    | 0-No, 1-Yes            |
|Hour of admission   | Code as following                | 6 Levels, 4 hours band              | One hot encoding (00.00.00.to.03.59.59 as reference) |
|                    | hour_of_admission_4h_band.04.00.00.to.07.59.59          |               | Code 1 if in 04.00.00.to.07.59.59      |
|                   | hour_of_admission_4h_band.08.00.00.to.11.59.59           |               |   Code 1 if in 08.00.00.to.11.59.59      |
|                  | hour_of_admission_4h_band.12.00.00.to.15.59.59           |               |  Code 1 if in 12.00.00.to.15.59.59      |
|                  | hour_of_admission_4h_band.16.00.00.to.19.59.59           |               | Code 1 if in 16.00.00.to.19.59.59      |
|                  | hour_of_admission_4h_band.20.00.00.to.23.59.59           |               |  Code 1 if in 20.00.00.to.23.59.59      |
|Day of week of admission | Code as following      | Monday – Sunday             | One hot encoding (Sunday as reference)              |
|                 | day_of_week_of_admission.Monday      |              | Code 1 if Monday             |
|                 | day_of_week_of_admission.Saturday      |              | Code 1 if Saturday               |
|                 | day_of_week_of_admission.Sunday      |              | Code 1 if Sunday             |
|                 | day_of_week_of_admission.Thursday      |              | Code 1 if Thursday               |
|                 | day_of_week_of_admission.Tuesday      |              | Code 1 if Tuesday              |
|                 | day_of_week_of_admission.Wednesday      |              | Code 1 if Wednesday              |
|hypertension             | hypertension                  |      Yes or No               | 0-No, 1-Yes                   |
|Atrial fibrillation (AF)  | atrial_fibrillation          |    Yes or No                  | 0-No, 1-Yes                  |
|diabetes                 | diabetes                      |      Yes or No                  | 0-No, 1-Yes                |
|Previous stroke/tia    | previous_stroke_tia           |   No,  Yes     | 0-No, 1-Yes          |
|Prior anticoagulation if AF*|     prior_anticoagulation_if_Afib |    No, No but, Unknown, Yes| One hot encoding (No as reference)      |
|    | prior_anticoagulation_if_Afib.No.but              |        | Code 1 if No but        |
|    | prior_anticoagulation_if_Afib.Unknown              |        | Code 1 if Unknown          |
|    | prior_anticoagulation_if_Afib.Yes              |        | Code 1 if Yes           |
|Modified Rankin Scale pre stroke| rankin_scale_prestroke  |                             | 0-5                         |
|level of consciousness|  nihss_loss_of_consciousness      |                                  | 0-3           |
|Type of stroke    |  Code as following   |    Infarction, Primary Intracerebral Haemorrhage, Unknown |    One hot encoding (Infarction as reference)  |
|        |  type_of_stroke.Primary.Intracerebral.Haemorrhage   |    |  Code 1 if Primary Intracerebral Haemorrhage    |
|        |  type_of_stroke.Uknown                     |    |  Code 1 if Unknown   | 


#### Important

* All variables/features must be measured within 24 hours after hospital admission
* All variable names and coding have to be exactly the same as the above table, including the sequence


### Software enviornment

* Training were performed in Python
* Required packages are shown in xxx;
* For testing purposes, validation_sample was randomly generated according to the above table. These values are randomly generated and are not representative of the training dataset.


### How to impute the Missing data in the validation dataset

* Missing data in the training set were imputed as the following:
   + New category for Unknown as stated in the above Table;
* Missing data in the validation set can use mean for continual numerical variable and median for categorical variable in the validation set (many cases) OR Multiple Imputation.



### Hyperparemeter tuning strategy for the trained mdoels

For LR with elastic net, we used the “train” function from caret R package, with 5-fold CV and 10 grids for each tuning parameters. For XGBoost, we tuned with 5-fold CV and 100 random combinations of all hyperparameters in certain intervals, i.e. maximum depth of each tree to be 3 to 10, minimum child weight to be 1 to 10, gamma (regularisation parameter) to be 0 to 1, the proportion of observations supplied to a tree to be 0.5 to 1, the proportion of features supplied to a tree to be 0.5 to 1.


