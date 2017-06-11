# project1.1

mkdir -p /home/acadgild/project1
cd /home/acadgild/project1

cp /home/acadgild/Downloads/crimes  /home/acadgild/project1/crimes

pig -x local

crimes = LOAD 'crime_details' USING PigStorage(';') AS (id:int,
Case_Number:chararray,
Date:chararry,
Block:chararray,
IUCR:chararray,
Primary_Type:chararray,
Description:chararray,
Location_Desc:chararray,
Arrest:chararray,
Domestic:chararray,
Beat:int,
District:int,
Ward:int,
Community_Area:int,
FBICode:chararray,
X_Coordinate:chararray,
Y_Coordinate;chararray,
Year:int,
Updated:chararray,
Latitude:chararray,
Longitude:chararray,
Location:tuple(lat:chararrary,long:chararray));
dump crimes;
describe crimes;

grouped_crimes = group crimes by FBIcode;
dump grouped_crimes;
describe grouped_crimes;
grouped_crimes_count = foreach grouped_crimes generate group, COUNT(*);
dump grouped_crimes_count;

filtered_fbi32 = FILTER grouped_crimes by FBIcode matches '32';
dump filtered_fbi32;

filtered_primary = FILTER crimes by primary_type matches 'THEFT' AND  Arrest matches 'TRUE';
dump filtered_primary;
grouped_district = group filtered_primary by district;
grouped_district_count = foreach grouped_district generate group, COUNT(*);
dump grouped_crimes_count;

filtered_arrest = FILTER crimes by arrest matches 'TRUE';
conv_date = FOREACH filtered_arrest GENERATE ToUnixTime(ToDate(date, 'dd-MM-yyyy'));
Filter_date = FILTER conv_date by ToUnixTime > '01-10-2014'  AND ToUnixTime < '01-10-2015';
dump filter_date;
