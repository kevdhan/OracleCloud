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

## (2) Actual Steps: Data Flow, Unions, Schedule

### Steps A: Data Flow + Unionization
First, we'll create the Data Flow to unionize our datasets.

![A_OverallArchitecture](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_OverallArchitecture.png)

Here is the overall finalized Data Flow Architecture! Lets begin the work to build this out!

Let's begin!!

![A_CreateDataFlow](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_CreateDataFlow.png)

1. Click Create a Data Flow, then Add “AggregatedWeatherData”.

![A_DataFlowPage](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_DataFlowPage.png)

2. Before moving on, I’ll explain the Data Flow page: 
   1. **Top** – Data Flow Architecture, the flow of your data and transformations applied
   2. **Left** – Data Flow Steps, basically different transformations you can apply to your data
   3. **Middle** – Data Flow Steps Extra Parameters, specify what you want 
   4. **Bottom** – Preview of Data, shows most current version of data (even after transformations)

![A_AddSteps](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_AddSteps.png)

3. Now, we’ll apply some Data Flow Steps to show the capabilities of Data Flow Steps. To add Data Flow Steps, either (1) Click and Drag a Step from the left to the Architecture area or (2) In the Architecture area, hover to the right of an icon and click “+” and select the wanted Data Flow Step. 

4. To “AggregatedWeatherData”: 
   1. Nothing. Moving forward this dataset will contain the “final” format, meaning it contains the finalized wanted column format – like naming convention, datatype, etc.
5. To "NewWeatherData":
   1. If you remember in the prerequisite, we “Prepared” the “AggregatedWeatherData” dataset – we added an ifnull transformation. We can actually apply this in the Data Flow, removing the need to do this manually for each new dataset we upload… let Data Flow take care of it!

   ![A_TransformColumn](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_TransformColumn.png)

   2. Add the “Transform Column” step. In the Function finder, search “ifnull”, then double click. Add the expression “IFNULL(Wind Chill, 0)”. This will check the Wind Chill column, and if null, replace with the value 0. 

   ![A_SplitColumn](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_SplitColumn.png)
   
   3. Next, we’ll also replicate the “Split Columns” and “Select Columns” step we did in the Prerequisite section to the “AggregatedWeatherData” dataset. To unionize, the datasets need to match all columns – Name and Datatype and need to be in the same column order! First, we’ll split the Name Column into columns “Zip Code” and “Country”. 

   ![A_SelectColumns](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_SelectColumns.png)
   
   4. Then, we’ll select all the columns, except the “Name” column, in order that match up with the ordering of columns in the “AggregatedWeatherData” dataset. This must be done so that when unionizing, the tool can correctly match up the common columns and unionize the rows. 
   
   **Here is the following order of the columns**:
      1. Zip Code 
      2. Country
      3. Date Time
      4. Max Temp
      5. Min Temp
      6. Temp
      7. Wind Chill
      8. Heat Index
      9. Precipitation
      10. Snow
      11. Snow Depth
      12. Wind Speed 
      13. Wind Gust
      14. Visibility
      15. Cloud Cover
      16. Relative Humidity
      17. Conditions

![A_UnionRows](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_UnionRows.png)
   
6. Now, we’ll add the “Union Rows” step. Select to “Keep – All rows from Input 1 and Input 2 (Union All)”. What this will do combine both datasets, “AggregatedWeatherData” and “NewWeatherData”.

![A_SaveData](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_SaveData.png)

7. Finally, add the “Save Data” step. Choose the name “AggregatedWeatherData”. This will replace the existing “AggregatedWeatherData”. But, don’t worry, this is done purposefully. Moving forward, we want “AggregatedWeatherData” to be the dataset with combined weather data from different months, and “NewWeatherData” to be the dataset with new fresh Weather data. This way, because we don’t need to rename our datasets, we won’t need to create a new Data Flow and can run the Data Flow on a schedule. Click Save & Run. The Data Flow should start running.

![A_Check](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/A_Check.png)

8. Before we move on to scheduling the data flow and using the newly created Data Flow to add new incoming weather data, we’ll check to make sure the Data Flow unionized our data correctly. Go back to your Data Sets and click on “AggregatedWeatherData”. You should see that it contains both November and December data.

### Steps B: Testing Data Flow with new Weather Data + Scheduling the Data Flow
Now that we created our Data Flow, we won’t need to touch anything else (other than uploading our new Weather Data – which can technically be automated too if the data was from a Connections and not a local CSV file, but more on that next time!)! We’ll test our Data Flow with new Weather data and even schedule it (further reducing manual clicks)!

Let's Begin!!

![B_ReloadData](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/B_ReloadData.png)

1. Go to your datasets, then on the farthest right of the dataset “NewWeatherData” click on “Actions Menu”.
2. From here, click Reload Data, and upload the January dataset. 

![B_Run](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/B_Run.png)

3. Then, go to Data Flows, and click on the “Actions Menu” (furthest to the right) of the  Data Flow created in this tutorial, and click Run. But wait!!! We didn’t perform any transformations on the new dataset! Won’t this affect the unionization process? Nope, remember, we added these steps in the Data Flow, so it’s all taken care of.

![B_Verification](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/B_Verification.png)

4. To verify, go to your Data Sets, click on “AggregatedWeatherData”, and verify that it contains all 3 months: November, December, January!

![B_Schedule](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Data%20Flow/Union/images2/B_Schedule.png)

5. To simplify life even further, go back to your Data Flows and click “Actions Menu” again. But, instead of “Run”, click “New Schedule”. Here, you can set a schedule for your Data Flow, where it’ll automatically run based off of the schedule you set!

## (3) Conclusion
That covers this tutorial! In this tutorial, we unionized weather data from different months using the feature Data Flow within OAC. In addition, we covered topics like adding new data sets, manually preparing the data sets, creating a data flow and using features like Data Flow Steps and Expression Editor to transform our data, and even running this data flow on a schedule.

Hopefully you saw a lot of the benefits of this tool. This can help save you a lot of time by allowing you to reuse transformations (reducing manual tasks) and even running it on a schedule. 

## (4) Documents
* [Prepare Your Data Set for Analysis](https://docs.oracle.com/en/cloud/paas/analytics-cloud/acubi/enrich-and-transform-data.html)
* [About Data Flows](https://docs.oracle.com/en/cloud/paas/analytics-cloud/acubi/data-flows.html#GUID-E914E1B4-1D62-44A5-8785-E1ED51DA2F7E)
* [Merge Rows in a Data Flow]( https://docs.oracle.com/en/cloud/paas/analytics-cloud/acubi/using-steps.html#GUID-25385368-4B38-47C1-8AA6-36D780719920)
* [Schedule a Data Flow](https://docs.oracle.com/en/cloud/paas/analytics-cloud/acubi/schedule-data-flow.html#GUID-63084FD8-19DB-4825-873B-5D0E479F9A4A)



