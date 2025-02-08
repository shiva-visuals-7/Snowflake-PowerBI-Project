# Snowflake-PowerBI-Project

*This repository contains SQL scripts for integrating Snowflake with AWS S3 for Power BI analysis.*


**Season-storage_integration**

create or replace storage integration integration_PBI

type = external_stage

storage_provider = \'S3\'

enabled = true

storage_aws_role_arn = \'arn:aws:iam::222634379646:role/powerbi_role\'

storage_allowed_locations = (\'s3://powerbi-season.proj/\')

comment = \'optional comment\'

**To describe the integration object:**

//description integration obj

desc integration integration_PBI;

![](Query_Results/image1.png)**RESULTS:**

***Season-CREATE DB, Schema, Table & Stage**

***Create a database:***

CREATE database PowerBI;

![](Query_Results/image2.png){width="6.268055555555556in"
height="1.2597222222222222in"}

***Create a schema:***

create schema BI_Data;

![](Query_Results/image3.png){width="6.268055555555556in"
height="1.2958333333333334in"}

***Create a Table:***
create table BI_Dataset (

Year int, Location string, Area int,

Rainfall float, Temperature float, Soil_type string,

Irrigation string, yeilds int,Humidity float,

Crops string,price int,Season string

);


![](Query_Results/image4.png){width="6.268055555555556in"
height="1.2159722222222222in"}

***To check the dataset:***

select \* from BI_Dataset;

![](Query_Results/image5.png){width="6.268055555555556in"
height="1.4743055555555555in"}**RESULTS:**

***Create a stage:***

create stage BI_Data.s1

url = \'s3://powerbi-season.proj\'

storage_integration = integration_PBI

![](Query_Results/image6.png){width="6.268055555555556in"
height="1.3006944444444444in"}**RESULTS:**


***To load data from the stage into the dataset:***
copy Into BI_Dataset

from \@s1

file_format = (type=csv field_delimiter=\',\' skip_header=1 )

on_error = \'continue\'

![](Query_Results/image7.png){width="6.268055555555556in"
height="1.1444444444444444in"}**RESULTS:**

***To verify the data:***
select \* from BI_Dataset;

![](Query_Results/image8.png){width="6.268055555555556in"
height="1.538888888888889in"}**RESULTS:**
