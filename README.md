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

<span style="font-size:18px;"><b><i>Result:</i></b></span>

![](Query_results/image1.png){width="6.268055555555556in"
height="2.8027777777777776in"}

***Season-CREATE DB, Schema, Table & Stage**

***Create a database:***

CREATE database PowerBI;

<span style="font-size:18px;"><b><i>Result:</i></b></span>
![](Query_results/image2.png){width="6.268055555555556in"
height="1.2597222222222222in"}

***Create a schema:***

create schema BI_Data;

<span style="font-size:18px;"><b><i>Result:</i></b></span>
![](Query_results/image3.png){width="6.268055555555556in"
height="1.2958333333333334in"}

***Create a Table:***
create table BI_Dataset (

Year int, Location string, Area int,

Rainfall float, Temperature float, Soil_type string,

Irrigation string, yeilds int,Humidity float,

Crops string,price int,Season string

);

<span style="font-size:18px;"><b><i>Result:</i></b></span>
![](Query_results/image4.png){width="6.268055555555556in"
height="1.2159722222222222in"}

***To check the dataset:***

select \* from BI_Dataset;

<span style="font-size:18px;"><b><i>Result:</i></b></span>
![](Query_results/image5.png){width="6.268055555555556in"
height="1.4743055555555555in"}**RESULTS:**

***Create a stage:***

create stage BI_Data.s1

url = \'s3://powerbi-season.proj\'

storage_integration = integration_PBI

<span style="font-size:18px;"><b><i>Result:</i></b></span>
![](Query_results/image6.png){width="6.268055555555556in"
height="1.3006944444444444in"}**RESULTS:**


***To load data from the stage into the dataset:***
copy Into BI_Dataset

from \@s1

file_format = (type=csv field_delimiter=\',\' skip_header=1 )

on_error = \'continue\'

<span style="font-size:18px;"><b><i>Result:</i></b></span>
![](Query_results/image7.png){width="6.268055555555556in"
height="1.1444444444444444in"}**RESULTS:**

***To verify the data:***
select \* from BI_Dataset;

<span style="font-size:18px;"><b><i>Result:</i></b></span>
![](Query_results/image8.png){width="6.268055555555556in"
height="1.538888888888889in"}**RESULTS:**
