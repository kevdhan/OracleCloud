# Data Flow - Unions

Topics this Tutorial will go over:
* Uploading Data 
* Manual Preparation/Transformation of Data
* Data Flow - ETL Tool Available within OAC
   * Data Steps:
      * Unions
      * Other Transformations
   * Scheduling - Automating Data Flows

The goal of this tutorial is to show how to use OAC, and more specifically Data Flow (an ETL tool within OAC), to unionize data sets. To best show this, weather datasets from different months (November, December, and January) will be unionized.

First, datasets will be uploaded. Then the data will be prepared (transformed) so that it is formatted correctly. Next, a Data Flow will be created to unionize the datasets. Lastly, we will set a schedule for the newly created Data Flow so that we can unionize new incoming data. For example, let’s say you want to unionize the aggregated data set with the newest month’s data on a monthly basis. What you can do is schedule the data flow to run once at the end of every month, that way when you upload the newest dataset, the data flow will automatically on schedule unionize the data set for you, leaving out that extra manual step you would have had to do!

## (1) Prerequesites: Upload Data + Data Preparation
Before we begin creating the Data Flow and Unionizing the datasets, we will upload November and December Weather datasets. We will “prepare” (perform transformations) the data so that it’s in the wanted form.

Here is a preview of the dataset:
![SampleDataSet](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/SampleDataSet.png)

You can use your own personal data, however, for simplicity, you can use the same datasets as this tutorial.
[Datasets](https://github.com/kevdhan/OracleCloud/tree/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/datasets)

Lets begin!!

![Prereq_CreateDataSet](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_CreateDataSet.png)

1. Click Create Data Set

![Prereq_UploadNovember](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_UploadNovember.png)
![Prereq_Aggregated](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_Aggregated.png)

2. Upload November Data as "AggregatedWeatherData"
   
   ![Prereq_PrepareAggregated](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_PrepareAggregated.png)
   
   1. Prepare it – Perform Transformations. Click on the “AggregatedWeatherData” dataset, then click “Prepare”

   ![Prereq_Split_1](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_Split_1.png)
   ![Prereq_Split_2](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_Split_2.png)
   
   2. Edit “Name” Column – split column on Delimiter “,” , and create 2 new columns: “Zip Code” and “Country”. We do this to create more meaningful columns.

   ![Prereq_Delete](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_Delete.png)
   
   3. Delete the Name Column, we do not need this column anymore.

   ![Prereq_Edit_1](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_Edit_1.png)
   ![Prereq_Edit_2](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/Prereq_Edit_2.png)
   
   4. Edit “Wind Chill” Column – add a PL/SQL based expression where if null, replace it with the number value “0”
   5. Review change, Apply Script

3. Upload December Data as "NewWeatherData"
   1. We'll hold off on any transformations for now!

## Actual Steps

### Steps A: Data Flow + Unionization

### Steps B: Testing Data Flow with new Weather Data + Scheduling the Data Flow
