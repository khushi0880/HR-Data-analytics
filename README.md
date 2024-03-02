# Presence Insights Dashboard

## Dataset Used
[Download Here](https://docs.google.com/spreadsheets/d/1uPXHI99WhBhbqGw6I8uj172N9x1MsNLx/edit?usp=drive_link&ouid=111948071681361579328&rtpof=true&sd=true)

### Dashboard Link : https://drive.google.com/file/d/12SwjUMhUmqISBFfJhKGErDE29JAM-dkT/view?usp=drive_link
## Problem Statement

This dashboard helps the company understand the working preference of their employees. It helps the company to plan Team Building Activity on days when majority of employees
are present. Company can also do better utilization of space and save their infrastructure cost. Find sick leave percentage and reason behind it to enable precautionary 
measures. It helps them to understand on which day people are taking Work From Home, Percentage of Sick leave and Presence percentage is more.

### Import and Data Transformation in Power BI 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Transform and combine the data of April, May, June.
- Step 3 : Duplicat the query and create tempelate to instruct Power BI that transformation has been made in one sheet and copy the same over all other sheet
- Step 4 : Creating transformation in the template and that transformation will be replicated to all the sheets.
- Step 5 : Select one sheet which is Apr 2022
- Step 7 : Delete all other column except for the 'Date'.
- Step 8 : Delete 'Change Type' step for referencing column from all the three months.
- Step 9 : Unpivot other columns except for Employee Code, Name, Date and Value.
- Step 10 : Change data type of 'Date' column to date and remove errors from column.
- Step 12 : Create a new parameter named Worksheet for sheet Apr 2022. Parameter is a dynamic way of filtering data
- Step 13 : Hard Filter data based on Parameter Worksheet.
- Step 14 : Create function named GetData to encapsulate all the steps into it and replicate it across all the entities.
- Step 15 : Delete Change Type in a Tempelate.
- Step 16 : Add this fuction as column into Attendance sheet by invoking custom function.
- Step 17 : Remove all other columns except for Sheet name, Employee code, Name, Date, Value.
- Step 18 : Rename Attendance sheet to Final data and load it into Power BI afetr diabling Template load.

### Creating Metrics using DAX
- Step 1 : Cteate new table called Measure Table.
- Step 2 : Create a new measure for calculating Total Working Days
```Power BI
Total Working Days =
var totaldays = COUNT('Final Data'[Value])
var nonworkday = CALCULATE(COUNT('Final Data'[Value]),'Final Data'[Value] in {"Wo","Ho"})
RETURN
totaldays - nonworkday
```
- Step 3 : Create a new Column in Final Data for WFH Count.
```Power BI
WFH count = SWITCH(TRUE(),
'Final Data'[Value] = "WFH",1,
'Final Data'[Value] = "HWFH",0.5,
0)
```
- Step 4 : Create a new measure in Measure Table named WFH Count.
```
WFH Count = SUM('Final Data'[WFH count])
```
- Step 5 : Create a new measure for calculating Present Days.
```Power BI
Present Days =
Var Presentdays = CALCULATE(COUNT('Final Data'[Value]),'Final Data'[Value]="P")
RETURN
Presentdays + [WFH Count]
```
- Step 6 : Create a new measure for calculating Present %.
```
Presence % = DIVIDE([Present Days],'Measure Table (2)'[Total Working Days],0)
```
- Step 7 : Create a new column as Date Month in 'mmm yy' format  in Final Data table.
```
Month = STARTOFMONTH('Final Data'[Date])
```
- Step 8 : Create a new measure for calculating WFH %.
```
WFH % = DIVIDE([WFH Count],[Present Days],0)
```
- Step 9 : Create a new column for calculating SL Count
```
SL count = SWITCH(TRUE(),
'Final Data'[Value] = "SL",1,
'Final Data'[Value] = "HSL",0.5,
0)
```
- Step 10 : Create a new measure in Measure Table named SL Count.
```
SL Count = SUM('Final Data'[SL count])
```
- Step 11 : Create a new measure for calculating SL %.
```
SL % = DIVIDE([SL Count],[Present Days],0)
```
