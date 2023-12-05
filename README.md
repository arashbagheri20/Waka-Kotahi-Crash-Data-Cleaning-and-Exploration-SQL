# Waka-Kotahi-Crash-Data-Exploration-SQL
## Project Objective
Exploring and visualisation of 814772 traffic crashes reported since January 2000 to Waka Kotahi by New Zealand police
## Tools:
- Data Cleaning & Exploration: SQL Server
- Visualization: Power Bi
## 1 Data Cleaning & Exploration
### 1.1 Creating a new table (named Crash_Data) in the database without unnecessary columns and cleaning data:   
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
### 	1.2 
		--Executing a query to look at the number of accidents by Crash Severity (using group by)
			- SQL Code Used:  
			select Crash_Severity, 
			count(Mesh_Block_ID) as Acccident_Count
			from  CrashData..Crash_Data 
			group by Crash_Severity
			order by Crash_Severity


--Executing a query to look at the number of accidents by Crash Severity over the years (using group by)
select Crash_Year, Crash_Severity,
count(Mesh_Block_ID) as Acccident_Count
from  CrashData..Crash_Data 
group by Crash_Year, Crash_Severity
order by Crash_Year, Crash_Severity





--Executing a query to look at the number of accidents and the Fatal/Serious/Minor injury casualties involved by Crash Severity
select Crash_Severity,
count(Crash_Severity)		as Acccident_Count,
sum(Fatal_Count)			as Fatal_Casualties,
sum(Serious_Injury_Count)	as Serious_Injuired_Casualties,
sum(Minor_Injury_Count)		as Minor_Injuired_Casualties
from  CrashData..Crash_Data
group by Crash_Severity

-- Executing a query to look at the number of accidents by Road Surface and Crash Severity
select Road_Surface,Crash_Severity, 
count(Crash_Severity) as Acccident_Count
from  CrashData..Crash_Data
group by Crash_Severity,Road_Surface
order by Road_Surface desc

-- Executing a query to look at the number of Fatal/Serious/Minor injury casualties involved in the crashes by Road Surface
select Road_Surface, 
sum(Fatal_Count)		  as Fatal_Casualties,
sum(Serious_Injury_Count) as Serious_Injuired_Casualties,
sum(Minor_Injury_Count)	  as Minor_Injuired_Casualties
from  CrashData..Crash_Data
group by Road_Surface

-- Executing a query to look at the number of accidents Light type
select Light,
count(Crash_Severity) as Acccident_Count
from  CrashData..Crash_Data 
group by light
order by Light, Acccident_Count desc


-- Executing a query to look at the number of accidents grouped by Road Surface and Light type
select Light,Crash_Severity, count(Crash_Severity) as Acccident_Count
from  CrashData..Crash_Data 
group by Crash_Severity,light
order by Light asc

-- Executing a query to look at the number of Fatal/Serious/Minor injury casualties involved by Light
select Light, 
sum(Fatal_Count)			as Fatal_Casualties,
sum(Serious_Injury_Count)	as Serious_Injuy_Casualties,
sum(Minor_Injury_Count)		as Minor_Injuiry_Casualties
from  CrashData..Crash_Data 
group by Light
order by Light asc

--Executing a query to look at the number of Fatal/Serious/Minor injury casualties over the years
select Crash_Year,
sum(Fatal_Count)			as Fatal_Casualties,
sum(Serious_Injury_Count)	as Serious_Injuiry_Casualties,
sum(minor_Injury_Count)		as Minor_Injuiry_Casualties
from  CrashData..Crash_Data
group by Crash_Year
order by Crash_Year

--Executing a query to look at the number of accidents and Fatal/Serious/Minor injury casualties involved by Crash Severity over the years
select Crash_Year,Crash_Severity,
count(Crash_Severity)		as Acccident_Count,
sum(Fatal_Count)			as Fatal_Casualties,
sum(Serious_Injury_Count)	as Serious_Injuiry_Casualties,
sum(minor_Injury_Count)		as Minor_Injuiry_Casualties
from  CrashData..Crash_Data
group by Crash_Year, Crash_Severity
order by Crash_Year,Crash_Severity


--Executing a query to look at the total number of accidents by the vehicles involved in the crashes over the years
select Crash_Year,count(Crash_Severity)		as Acccident_Count,
count(case when Bicycle>0 then Mesh_Block_ID end)as Bicycle_involved_Accidnet_Count,
count(case when Bus>0 then Mesh_Block_ID end)as Bus_involved_Accidnet_Count,
count(case when Car_Station_Wagon>0 then Mesh_Block_ID end)as Car_Station_Wagon_involved_Accidnet_Count,
count(case when Moped>0 then Mesh_Block_ID end)as Moped_involved_Accidnet_Count,
count(case when Motorcycle>0 then Mesh_Block_ID end)as Motorcycle_involved_Accidnet_Count,
count(case when School_Bus>0 then Mesh_Block_ID end)as School_involved_Bus_Accidnet_Count
from  CrashData..Crash_Data 
group by Crash_Year
order by Crash_Year

--Executing a query to look at the number of Fatal/Minor/Non-injury/Serious accidents by the vehicles involved in the crashes over the years
select Crash_Year, Crash_Severity, count(Crash_Severity)		as Acccident_Count,
count(case when Bicycle>0 then Mesh_Block_ID end)as Bicycle_involved_Accidnet_Count,
count(case when Bus>0 then Mesh_Block_ID end)as Bus_involved_Accidnet_Count,
count(case when Car_Station_Wagon>0 then Mesh_Block_ID end)as Car_Station_Wagon_involved_Accidnet_Count,
count(case when Moped>0 then Mesh_Block_ID end)as Moped_involved_Accidnet_Count,
count(case when Motorcycle>0 then Mesh_Block_ID end)as Motorcycle_involved_Accidnet_Count,
count(case when School_Bus>0 then Mesh_Block_ID end)as School_involved_Bus_Accidnet_Count
from  CrashData..Crash_Data 
group by Crash_Year, Crash_Severity
order by Crash_Year


--Executing a query to look at the total number of accidents by Road Surface
select Road_Surface,count(Crash_Severity)		as Acccident_Count
from  CrashData..Crash_Data 
group by Road_Surface
order by Road_Surface

--Executing a query to look at the total number of accidents by Crash Severity over different Road Surfaces
select Road_Surface, Crash_Severity, 
count(Crash_Severity)		as Acccident_Count
from  CrashData..Crash_Data 
group by Road_Surface, Crash_Severity
order by Road_Surface


--Executing a query to look at the total number of accidents by Road Surface over the years
select Crash_Year,Road_Surface,
count(Crash_Severity)		as Acccident_Count
from  CrashData..Crash_Data 
group by Crash_Year ,Road_Surface
order by Crash_Year



--Executing a query to look at the number of Fatal/Minor/Non-injury/Serious accidents by Area Types
select Area_Type,Crash_Severity, count(Crash_Severity)		as Acccident_Count
from  CrashData..Crash_Data 
group by Area_Type, Crash_Severity
order by Crash_Severity


--Executing a query to look at the total number of accidents by Area Type over the years
select Crash_Year, Area_Type,count(Crash_Severity)		as Acccident_Count
from  CrashData..Crash_Data 
group by Crash_Year, Area_Type
order by Crash_Year

--Executing a query to look at the number of accidents by the Weather Type
select Weather,
count(Crash_Severity)		as Acccident_Count
from  CrashData..Crash_Data 
group by Weather
order by Weather

--Executing a query to look at the number of fatal/Minor/Non-injury/Serious accidents by the Weather Type
select Weather,Crash_Severity, 
count(Crash_Severity)		as Acccident_Count
from  CrashData..Crash_Data 
group by Weather, Crash_Severity
order by Weather,Crash_Severity


--Executing a query to look at the total number of accidents by the Weather over the years
select Crash_Year, Weather,
count(Crash_Severity)		as Acccident_Count
from  CrashData..Crash_Data 
group by Crash_Year, Weather
order by Crash_Year
