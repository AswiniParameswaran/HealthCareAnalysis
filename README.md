# HealthCareAnalysis
 Power BI | DAX | Data Cleaning | Visualization  Developed an interactive Power BI dashboard to analyze healthcare data, focusing on patient  admissions, disease trends, and hospital performance. Used DAX for calculated metrics and applied  filters for dynamic data exploration and insights to support healthcare decision-making.
PowerBI dashboard creation includes nine major steps that are

Requirement Gathering
Data collection
Transformation and modelling
Data visualization Blueprint
Dashboard layout and design
Interactivity and navigation
Testing
Sharing
Maintenance and refresh
1)First step Requirement Gathering we have 4 phases

1)Identify Stakeholders –

Determine primary stakeholder and establish a point of contact who might be the domain experts or leaders who will eventually use the dashboard

2)Understand Business Objectives –

Through meetings and calls with stakeholders get an outline of Goals from the entire endeavor. Asking open ended questions will help to gain more insights to understand the data and how this dashboard will help achieve a specific business goal.

3)High Level Data Study –

A high level overview of data is required before initiate discussions around scope, metrics and other granular topics. So here are some data in terms of:

Data Sources
Column Description
Data Type
Volume and frequency
Data Quality — Missing Values or Anomalies
4)Define Scope –

This is the perfect stage to discuss Key Metrics, KPIs & Deployment Timelines. Document the calculations, time frames & scope which will help in setting the expectations and avoiding any future disagreements. Also as a best practice remember to keep a 20% buffer while finalizing deadlines because it’s always better to over deliver after a standard commitment than to underdeliver after an extraordinary delivery pitch.

My project goal is to

1. Track current status of patient waiting list
2. Analyze historical monthly trend of waiting list in Inpatient & Outpatient categories
3. Detailed specialty level & age profile analysis

This is the Data Scope
2018–2021
Required Metrics
1. Average & Median Waiting List
2. Current Total Wait List
Views Required
1. Summary Page
2. Detailed Page for Granular Analysis

2)Next move into the second step

There are over 200 different data connectors available to collect data inside power bi, but below are the most widely used connectors:

Excel / CSV
Folder Connection
SQL Server or Any database
Power BI services
Cloud Platforms — Azure, AWS, GCP etc.
ERP Systems — Salesforce, SAP etc.
Sharepoint
Web or JSON
This is the stage where decide the source of your data. This step is very important because this will also define how you are going to refresh the dashboard after deploying the solution. In this project i am going to use a central folder which will host all the files required for the dashboard refresh process.

I’m going to download the zip file from the above download button and extract the file inside a new folder on my desktop. This will act as my data repository going forward.

Open Power BI desktop and click on GET DATA and select FOLDER option. Click Connect button at the bottom right

Now browse the folder path where we have stored Inpatient data and press OK

3)Third step is Data transformation

Data transformation is a process of changing the structure of the data or applying additional steps which will clean or process the data for final usage. These transformations can be done in the Power Query Editor which is inbuilt into Power BI. To get to the Editor,

Go to the Report View
Click on the drop down on the Transform data Icon
You will see 3 Options, select the Transform Data option

Now for the purposes these steps can be done

Renaming Columns
Rearranging Columns
Appending 2 Tables
Replacing & trimming values
There are many other transformation steps which can apply like Pivot/Unpivot, Merge, filtering etc. but for now only need the above 3 steps.

Renaming Columns

While studying the data, I noticed that the Specialty column in Inpatient data is named as Speciality_Name, whereas in Outpatient it is named as just Specialty. So i rename the Outpatient column to match with the Inpatient data. Make sure to name it exactly the same, otherwise it will create an issue in following steps.

Rearranging Columns

Now rearrange the columns of the outpatient so that it matches with inpatient. You can just left click on the column header and drag it to the required position. Now while rearranging, I noticed that it have an additional column in Inpatient i.e. Case_Type which is missing from Outpatient. So i want to create one additional column in Outpatient table called Case_Type by

Go to Add Columns
Click on Custom Columns
Name the column as Case_Type
Enter the formula =”Outpatient”
Remember to place this new column at the same position as inpatients
Appending Tables

Now that both tables have the same column structure, can safely append them together. To do that come to Home tab and click on “Append Queries” button and click on “Append Queries as New”. Select 1st table as inpatient and 2nd table as outpatient. Rename this new table as “All_Data”. Finally click on “Close & Apply”.

Appending Tables

Observe that Age_Profile & Time_Band columns have some redundant data, so first use the Replace function button in power query to clean the data, for eg: “18+ months” and “18 month +”, both are the same, so replace either one to match the other. Secondly there are some trailing blanks in values of these columns so remove trailing blanks by using the Trim function button.

Data Modelling

Data modelling is a way to create relationship with one table to another, so that fetch valuable information from them in our reporting layer.

Data Modelling View, which is located at the left hand panel on Power BI. I will eusing All_Data from now onwards, so i can safely hide inpatient and outpatient data, can also stop it from loading into the data model by disabling it from the power query editor. Just right click on the table name in power query and uncheck Enable Load.

Now since specialty name is one of the key attributes that i am looking in this project, focus on that column now. In the data, there are huge number of specialty available and using all of them in report layer directly will create a clutter in visualization. A better approach would be to distribute them in buckets. So to do this I have created a specialty mapping file which will find in the downloaded resources. Import that file in power bi to create the relationship with All_Data.

Once i import this file, Power BI should auto detect relationship and connect both the tables. However if it does not then i can do it manually by following below steps:

Go to the model view
Click on Specialty_Name column in All_Data
Drag this column over the Specialty column in Mapping table
Now seeconnecting both the tables and an arrow pointing towards All_Data from Mapping table. This means that i have created a relationship between both the tables. The arrow signifies the filter context and tells that now i can use columns from Mapping table to filter data in All_Data.

5)After this moves into the Visualization Blueprint

For this exercise, I have already created a blueprint, however in live scenarios will sit down with team and create a wireframe of the required dashboard. After get this approved from the end stakeholder before starting development activities.


6)The next step is Dashboard Layout and Design

Before start the designing process, I usually enable below two properties from the View tab. This helps me place and align my visuals in a uniform manner.

Gridlines
Snap to Grid
Now use DAX to create our measures which will be used in our visuals. For now create 2 measures for calculating Latest Month & Previous Year Wait List

Latest Month Wait List = CALCULATE(SUM(All_Data[Total]),All_Data[Archive_Date] = MAX(All_Data[Archive_Date])) + 0
PY Latest Month Wait List = CALCULATE(SUM(All_Data[Total]),All_Data[Archive_Date]= EDATE(MAX(All_Data[Archive_Date]),-12)) + 0
Now as per my design blueprint, insert these 2 measures in a card visual. After this create a blank table where to store calculation method headers, i.e. in my dashboard i want to show Average values and Median values.

So enter a blank table from power bi and enter 2 row items manually i.e. Average & Median. Now with the newly created table create a button slicer, so that user can toggle between the two calculation.

Now create below measures which will help to get the calculation i need and also to make few chart titles dynamic

Median Wait List = MEDIAN(All_Data[Total]) 
Average Wait List = AVERAGE(All_Data[Total]) 
Avg/Med Wait List = SWITCH(VALUES('Calculation Method'[Calc Method]),"Average",[Average Wait List],"Median",[Median Wait List]) 
Dynamic Title = SWITCH(VALUES('Calculation Method'[Calc Method]),"Average","Key Indicators - Patient Wait List (Average)","Median","Key Indicators - Patient Wait List (Median)") 
NoDataLeft = IF(ISBLANK(CALCULATE(SUM(All_Data[Total]),All_Data[Case_Type]<>"Outpatient")),"No data for selected criteria","")  
NoDataRight = IF(ISBLANK(CALCULATE(SUM(All_Data[Total]),All_Data[Case_Type]="Outpatient")),"No data for selected criteria","")
Summary Page

Now place the charts based on my blueprint i.e doughnut, clustered column chart & top five Multi Row card. And remember to use the new measure which is Avg/Med Wait List in the values section.

Finally in the line chart at the bottom use Total column directly along with the Archive_Date. Remember to add a visual filter for Case_Type. So one chart will show Day Case & Inpatients and the other chart will show Outpatients. Add slicers for Archive_Date, Case_Type and Specialty.

Detailed View

Add a new page here add a matrix view using the Archive_Date, Specialty_Name, Age_Profile, Time_Bands, Case_Type and Total.

Tooltip Page

Create a new page which will be used as a tooltip. Add a chart to show Specialty and Total waitlist. Also add a card to show the Total sum of Wait List. Now set this page as tooltip by going to formatting >> Page Information >> Enable Allow Use as Tooltip

Now go back to summary page and select the line chart. General section of formatting, go to Tooltips and select the page i.e. the new tooltip page.

Beautify the Dashboard

This is very subjective but I go to Google or Adobe Stock website to draw inspiration. I selected a dashboard as my inspiration. Go to Color.Adobe.com to extract the colors from your reference dashboard. Keep a note of these colors somewhere.

Now go to PowerPoint or Canva to design the background of dashboard. Once done extract thedesign as png file and use that image as a background for Power BI canvas.


7)After this moves to Interactivity and navigation step

Now add interactivity in dashboard like navigation buttons, chart alt display text and hovering info.



8)Eight Step is Testing and Sharing

Ensure to conduct an extensive UAT session which will identify any bugs or data issues. After this ready to share the dashboard with a larger audience. Before sharing/publishing consider Row Level Security features within Power BI services.

9) The Last step Maintenance and refresh

Now focus on implementing a BAU process to run monthly refresh process and maintenance.
