📊 Manufacturing Efficiency Dashboard
Enhancing Manufacturing Performance Through Data-Driven Insights

🔍 Project Overview
This project leverages manufacturing production data and employee performance metrics to uncover inefficiencies, identify trends, and drive operational improvements. Built using Power BI, the dashboard offers a centralized, interactive view of key factors influencing manufacturing efficiency — including production costs, workforce productivity, and training investments.

By visualizing these elements together, stakeholders can make informed decisions that align cost management with workforce optimization and strategic growth.

🎯 Objectives
Analyze production cost trends, output quantities, and efficiency ratios across product lines and locations.

Evaluate how employee training and performance ratings impact overall productivity.

Investigate the relationship between salary levels and workforce output for better HR planning.

Deliver actionable insights through intuitive dashboards to support leadership decision-making.

🛠️ Tools & Technologies
Tool

Purpose

Power BI

Dashboard development & visualization

DAX

Custom calculations & KPIs

Power Query

Data cleaning & transformation

Excel

Initial data preprocessing

📂 Datasets
1. Manufacturing Production Data
Fields:

ProductID

ProductType

ProductionDate

ProductionCost

CountryOfOrigin

QuantityProduced

WarehouseLocation

2. Employee Performance Metrics
Fields:

EmployeeID

Department

HireDate

Salary

CountryOfOperation

ProductID

PerformanceRating

TrainingRecords

💻 Code Implementation
Power Query (M Language) - Data Cleaning
// Clean Manufacturing Production Data
let
    Source = Excel.Workbook(File.Contents("Manufacturing_Data.xlsx"), null, true),
    Production_Data = Source{[Item="Production",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Production_Data, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{
        {"ProductID", Int64.Type}, 
        {"ProductType", type text}, 
        {"ProductionDate", type date}, 
        {"ProductionCost", Currency.Type}, 
        {"CountryOfOrigin", type text}, 
        {"QuantityProduced", Int64.Type}, 
        {"WarehouseLocation", type text}
    }),
    #"Added Efficiency Column" = Table.AddColumn(#"Changed Type", "CostPerUnit", each [ProductionCost]/[QuantityProduced])
in
    #"Added Efficiency Column"
```m
// Clean Employee Performance Data
let
    Source = Excel.Workbook(File.Contents("Employee_Data.xlsx"), null, true),
    Employee_Data = Source{[Item="Employee",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Employee_Data, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{
        {"EmployeeID", Int64.Type}, 
        {"Department", type text}, 
        {"HireDate", type date}, 
        {"Salary", Currency.Type}, 
        {"CountryOfOperation", type text}, 
        {"ProductID", Int64.Type}, 
        {"PerformanceRating", Int64.Type}, 
        {"TrainingRecords", Int64.Type}
    })
in
    #"Changed Type"

DAX Measures
// Key Performance Indicators
Total Production Cost = SUM('Production'[ProductionCost])

Total Quantity Produced = SUM('Production'[QuantityProduced])

Average Cost Per Unit = DIVIDE([Total Production Cost], [Total Quantity Produced])

Average Performance Rating = AVERAGE('Employee'[PerformanceRating])

Total Training Hours = SUM('Employee'[TrainingRecords])

Employee Count = DISTINCTCOUNT('Employee'[EmployeeID])

Salary vs Performance Ratio = DIVIDE([Average Performance Rating], AVERAGE('Employee'[Salary])) * 1000

Efficiency Score = 
DIVIDE(
    [Total Quantity Produced], 
    [Total Production Cost]
) * 1000

// Time-based Calculations
MTD Production = 
TOTALMTD('Production'[QuantityProduced], 'Production'[ProductionDate])

QTD Production = 
TOTALQTD('Production'[QuantityProduced], 'Production'[ProductionDate])

YoY Production Growth = 
DIVIDE(
    CALCULATE(SUM('Production'[QuantityProduced]), SAMEPERIODLASTYEAR('Production'[ProductionDate])),
    SUM('Production'[QuantityProduced])
) - 1

Calculated Columns
// Production Table
Cost Efficiency Category = 
IF(
    'Production'[CostPerUnit] <= PERCENTILE.INC('Production'[CostPerUnit], 0.33), "High Efficiency",
    IF('Production'[CostPerUnit] <= PERCENTILE.INC('Production'[CostPerUnit], 0.66), "Medium Efficiency", "Low Efficiency")
)

// Employee Table
Experience Level = 
VAR YearsOfExperience = DATEDIFF('Employee'[HireDate], TODAY(), YEAR)
RETURN
IF(YearsOfExperience < 2, "Junior", IF(YearsOfExperience < 5, "Mid-Level", "Senior"))

Performance Category = 
IF('Employee'[PerformanceRating] >= 4, "High Performer", 
   IF('Employee'[PerformanceRating] >= 3, "Average Performer", "Low Performer"))

📈 Key Insights
📉 Cost Efficiency Trends: Production costs varied significantly by location and product type, highlighting opportunities for localized cost optimization.

👩‍🏭 Workforce Productivity Boost: Employees who received more training consistently achieved higher performance ratings, emphasizing ROI in L&D initiatives.

💰 Salary vs. Productivity Mismatch: Salary adjustments were often reactive rather than aligned with measurable productivity gains.

🌍 Global Operations Visibility: The dashboard enabled cross-country comparisons of KPIs, supporting global operational alignment and benchmarking.

📊 Dashboard Features
✅ Interactive Filters: Drill down by product, department, or country for granular analysis

✅ Dynamic Salary vs. Productivity Analysis: Visual correlation between compensation and output

✅ Training Impact Tracker: Monitor how training hours correlate with performance ratings over time

✅ End-to-End Efficiency Monitoring: Unified view of production metrics and workforce KPIs

✨ Summary
This dashboard demonstrates how integrating operational data with human capital metrics can reveal critical inefficiencies and opportunities. By bridging the gap between production outcomes and workforce dynamics, organizations can make smarter, faster, and more strategic decisions.

📌 Tags
#PowerBI #DAX #PowerQuery #DataModeling #DashboardDevelopment #ManufacturingAnalytics #BusinessIntelligence #WorkforceAnalytics
