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

![](Query_results/image1.png)

***Season-CREATE DB, Schema, Table & Stage**

***Create a database:***

CREATE database PowerBI;

<span style="font-size:18px;"><b><i>Result:</i></b></span>

![](Query_results/image2.png)

***Create a schema:***

create schema BI_Data;

<span style="font-size:18px;"><b><i>Result:</i></b></span>

![](Query_results/image3.png)

***Create a Table:***
create table BI_Dataset (

Year int, Location string, Area int,

Rainfall float, Temperature float, Soil_type string,

Irrigation string, yeilds int,Humidity float,

Crops string,price int,Season string

);

<span style="font-size:18px;"><b><i>Result:</i></b></span>

![](Query_results/image4.png)

***To check the dataset:***

select \* from BI_Dataset;

<span style="font-size:18px;"><b><i>Result:</i></b></span>

![](Query_results/image5.png)

***Create a stage:***

create stage BI_Data.s1

url = \'s3://powerbi-season.proj\'

storage_integration = integration_PBI

<span style="font-size:18px;"><b><i>Result:</i></b></span>

![](Query_results/image6.png)


***To load data from the stage into the dataset:***
copy Into BI_Dataset

from \@s1

file_format = (type=csv field_delimiter=\',\' skip_header=1 )

on_error = \'continue\'

<span style="font-size:18px;"><b><i>Result:</i></b></span>

![](Query_results/image7.png)

***To verify the data:***
select \* from BI_Dataset;

<span style="font-size:18px;"><b><i>Result:</i></b></span>

![](Query_results/image8.png)
