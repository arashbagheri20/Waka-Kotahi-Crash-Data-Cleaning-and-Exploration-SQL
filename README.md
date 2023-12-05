# Waka-Kotahi-Crash-Data-Exploration-SQL
## Project Objective
Exploring and visualisation of 814772 traffic crashes reported since January 2000 to Waka Kotahi by New Zealand police
## Tools:
- Data Cleaning & Exploration: SQL Server
- Visualization: Power Bi
## 1 Data Cleaning & Exploration
### 	1.1 Creating a new table (named Crash_Data) in the database without unnecessary columns and cleaning data:   
Replacing the Null values in “Train” and “Pedestrian” columns with '0' (using the isnull function).   
Replacing the Null values in the “RoadSurface” column with 'Unknown' (using the replace function).   
Adding Country name to the crash location (because there are other countries with similar city or district names as New Zealand) not to face problems in future for visualisation (using the concat function).   
Replacing 'NULL' values in the “Weather” column with "Unknown' (using the replace function).   
#### SQL Code Used:
	select meshblockId as Mesh_Block_ID, 
	bicycle as Bicycle ,bus as Bus,carStationWagon as Car_Station_Wagon,moped as Moped,motorcycle as Motorcycle,schoolBus as School_Bus, truck as Truck, isnull(train,0) as Train,
	crashSeverity as Crash_Severity, fatalCount as Fatal_Count, minorInjuryCount as Minor_Injury_Count,seriousInjuryCount as Serious_Injury_Count, isnull(pedestrian,0) as Pedestatian, 
	isnull(roadSurface,'Unknown') as Road_Surface,concat(tlaName,', New Zealand') as Crash_Location, urban as Area_Type, replace(weatherA,'NULL','Unknown') as Weather, light as Light, crashYear as Crash_Year 
	into CrashData..[Crash_Data]
	from CrashData..[Crash_Analysis_System_(CAS)_data] 
	where crashYear <2023;
### 1.2 Executing a query to look at the number of accidents by Crash Severity (using group by)
#### SQL Code Used:
	select Crash_Severity, 
	count(Mesh_Block_ID) as Acccident_Count
	from  CrashData..Crash_Data 
	group by Crash_Severity
	order by Crash_Severity
### 1.3 Executing a query to look at the number of accidents by Crash Severity over the years (using group by)
#### SQL Code Used:
	select Crash_Year, Crash_Severity,
	count(Mesh_Block_ID) as Acccident_Count
	from  CrashData..Crash_Data 
	group by Crash_Year, Crash_Severity
	order by Crash_Year, Crash_Severity
### 1.4 Executing a query to look at the number of accidents and the Fatal/Serious/Minor injury casualties involved by Crash Severity
#### SQL Code Used:
	select Crash_Severity,
	count(Crash_Severity)		as Acccident_Count,
	sum(Fatal_Count)		as Fatal_Casualties,
	sum(Serious_Injury_Count)	as Serious_Injuired_Casualties,
	sum(Minor_Injury_Count)		as Minor_Injuired_Casualties
	from  CrashData..Crash_Data
	group by Crash_Severity

### 1.5 Executing a query to look at the number of accidents by Road Surface and Crash Severity
#### SQL Code Used:
	select Road_Surface,Crash_Severity, 
	count(Crash_Severity) as Acccident_Count
	from  CrashData..Crash_Data
	group by Crash_Severity,Road_Surface
	order by Road_Surface desc

### 1.6 Executing a query to look at the number of Fatal/Serious/Minor injury casualties involved in the crashes by Road Surface
#### SQL Code Used:
	select Road_Surface, 
	sum(Fatal_Count)	  as Fatal_Casualties,
	sum(Serious_Injury_Count) as Serious_Injuired_Casualties,
	sum(Minor_Injury_Count)	  as Minor_Injuired_Casualties
	from  CrashData..Crash_Data
	group by Road_Surface

### 1.7 Executing a query to look at the number of accidents Light type
#### SQL Code Used:
	select Light,
	count(Crash_Severity) as Acccident_Count
	from  CrashData..Crash_Data 
	group by light
	order by Light, Acccident_Count desc


### 1.8 Executing a query to look at the number of accidents grouped by Road Surface and Light type
#### SQL Code Used:
	select Light,Crash_Severity, count(Crash_Severity) as Acccident_Count
	from  CrashData..Crash_Data 
	group by Crash_Severity,light
	order by Light asc

### 1.9 Executing a query to look at the number of Fatal/Serious/Minor injury casualties involved by Light
#### SQL Code Used:
	select Light, 
	sum(Fatal_Count)		as Fatal_Casualties,
	sum(Serious_Injury_Count)	as Serious_Injuy_Casualties,
	sum(Minor_Injury_Count)		as Minor_Injuiry_Casualties
	from  CrashData..Crash_Data 
	group by Light
	order by Light asc

### 1.10 Executing a query to look at the number of Fatal/Serious/Minor injury casualties over the years
#### SQL Code Used:
	select Crash_Year,
	sum(Fatal_Count)		as Fatal_Casualties,
	sum(Serious_Injury_Count)	as Serious_Injuiry_Casualties,
	sum(minor_Injury_Count)		as Minor_Injuiry_Casualties
	from  CrashData..Crash_Data
	group by Crash_Year
	order by Crash_Year

### 1.11 Executing a query to look at the number of accidents and Fatal/Serious/Minor injury casualties involved by Crash Severity over the years
#### SQL Code Used:
	select Crash_Year,Crash_Severity,
	count(Crash_Severity)		as Acccident_Count,
	sum(Fatal_Count)		as Fatal_Casualties,
	sum(Serious_Injury_Count)	as Serious_Injuiry_Casualties,
	sum(minor_Injury_Count)		as Minor_Injuiry_Casualties
	from  CrashData..Crash_Data
	group by Crash_Year, Crash_Severity
	order by Crash_Year,Crash_Severity


### 1.12 Executing a query to look at the total number of accidents by the vehicles involved in the crashes over the years
#### SQL Code Used:
	select Crash_Year,count(Crash_Severity)	as Acccident_Count,
	count(case when Bicycle>0 then Mesh_Block_ID end)as Bicycle_involved_Accidnet_Count,
	count(case when Bus>0 then Mesh_Block_ID end)as Bus_involved_Accidnet_Count,
	count(case when Car_Station_Wagon>0 then Mesh_Block_ID end)as Car_Station_Wagon_involved_Accidnet_Count,
	count(case when Moped>0 then Mesh_Block_ID end)as Moped_involved_Accidnet_Count,
	count(case when Motorcycle>0 then Mesh_Block_ID end)as Motorcycle_involved_Accidnet_Count,
	count(case when School_Bus>0 then Mesh_Block_ID end)as School_involved_Bus_Accidnet_Count
	from  CrashData..Crash_Data 
	group by Crash_Year
	order by Crash_Year

### 1.13 Executing a query to look at the number of Fatal/Minor/Non-injury/Serious accidents by the vehicles involved in the crashes over the years
#### SQL Code Used:
	select Crash_Year, Crash_Severity, count(Crash_Severity)as Acccident_Count,
	count(case when Bicycle>0 then Mesh_Block_ID end)as Bicycle_involved_Accidnet_Count,
	count(case when Bus>0 then Mesh_Block_ID end)as Bus_involved_Accidnet_Count,
	count(case when Car_Station_Wagon>0 then Mesh_Block_ID end)as Car_Station_Wagon_involved_Accidnet_Count,
	count(case when Moped>0 then Mesh_Block_ID end)as Moped_involved_Accidnet_Count,
	count(case when Motorcycle>0 then Mesh_Block_ID end)as Motorcycle_involved_Accidnet_Count,
	count(case when School_Bus>0 then Mesh_Block_ID end)as School_involved_Bus_Accidnet_Count
	from  CrashData..Crash_Data 
	group by Crash_Year, Crash_Severity
	order by Crash_Year


### 1.14 Executing a query to look at the total number of accidents by Road Surface
#### SQL Code Used:
	select Road_Surface,count(Crash_Severity)as Acccident_Count
	from  CrashData..Crash_Data 
	group by Road_Surface
	order by Road_Surface

### Executing a query to look at the total number of accidents by Crash Severity over different Road Surfaces
#### SQL Code Used:
	select Road_Surface, Crash_Severity, 
	count(Crash_Severity) as Acccident_Count
	from  CrashData..Crash_Data 
	group by Road_Surface, Crash_Severity
	order by Road_Surface


### 1.15 Executing a query to look at the total number of accidents by Road Surface over the years
#### SQL Code Used:
	select Crash_Year,Road_Surface,
	count(Crash_Severity)	as Acccident_Count
	from  CrashData..Crash_Data 
	group by Crash_Year ,Road_Surface
	order by Crash_Year



### 1.16 Executing a query to look at the number of Fatal/Minor/Non-injury/Serious accidents by Area Types
#### SQL Code Used:
	select Area_Type,Crash_Severity, count(Crash_Severity)	as Acccident_Count
	from  CrashData..Crash_Data 
	group by Area_Type, Crash_Severity
	order by Crash_Severity


### 1.17 Executing a query to look at the total number of accidents by Area Type over the years
#### SQL Code Used:
	select Crash_Year, Area_Type,count(Crash_Severity) as Acccident_Count
	from  CrashData..Crash_Data 
	group by Crash_Year, Area_Type
	order by Crash_Year

### 1.18 Executing a query to look at the number of accidents by the Weather Type
#### SQL Code Used:
	select Weather,
	count(Crash_Severity) as Acccident_Count
	from  CrashData..Crash_Data 
	group by Weather
	order by Weather

### 1.19 Executing a query to look at the number of fatal/Minor/Non-injury/Serious accidents by the Weather Type
#### SQL Code Used:
	select Weather,Crash_Severity, 
	count(Crash_Severity) as Acccident_Count
	from  CrashData..Crash_Data 
	group by Weather, Crash_Severity
	order by Weather,Crash_Severity


### 1.20 Executing a query to look at the total number of accidents by the Weather over the years
#### SQL Code Used:
	select Crash_Year, Weather,
	count(Crash_Severity)as Acccident_Count
	from  CrashData..Crash_Data 
	group by Crash_Year, Weather
	order by Crash_Year
### 1.21 Creating a view table for visualisation in Power Bi
#### SQL Code Used:
	create view Crash_Data_Cleaned_in_SQL as 
	select *
	from CrashData..[Crash_Data]
## 2 Visualization in Power Bi
	### 2.1	Questions to answer/KPIs 
	#### 2.1.1	What is the total number of crashes 
	#### 2.1.2	What is the total number of casualties involved
	#### 2.1.3	What is the total number of fatal crashes
	#### 2.1.4	What is the total number of fatal casualties
	#### 2.1.5	What is the total number of seriously injured casualties
	#### 2.1.6	What is the total number of minorly-injured casualties
	#### 2.1.7	What is the distribution of fatal casualties by the type of vehicles involved? (Cars, trucks, motorcycles, etc)
	#### 2.1.8	What is the trend of fatal casualties from 2000-2022
	#### 2.1.9	What is the distribution of fatal casualties by road surface used?
	#### 2.1.10	What is the distribution of fatal casualties by the type of area (open or urban) where the crash happened?
	#### 2.1.11	What is the distribution of fatal casualties by the district(city) area where the crash happened?






















 
	


