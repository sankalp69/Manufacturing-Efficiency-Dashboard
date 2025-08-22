# 📊 Manufacturing Efficiency Dashboard
**Enhancing Manufacturing Performance Through Data-Driven Insights**

## 🔍 Project Overview

This project leverages **manufacturing production data** and **employee performance metrics** to uncover inefficiencies, identify trends, and drive operational improvements. Built using **Power BI**, the dashboard offers a centralized, interactive view of key factors influencing manufacturing efficiency — including production costs, workforce productivity, and training investments.

By visualizing these elements together, stakeholders can make informed decisions that align cost management with workforce optimization and strategic growth.

## 🎯 Objectives

- Analyze **production cost trends**, **output quantities**, and **efficiency ratios** across product lines and locations.
- Evaluate how **employee training** and **performance ratings** impact overall productivity.
- Investigate the relationship between **salary levels** and **workforce output** for better HR planning.
- Deliver **actionable insights** through intuitive dashboards to support leadership decision-making.

## 🛠️ Tools & Technologies

| Tool         | Purpose                              |
|--------------|---------------------------------------|
| **Power BI** | Dashboard development & visualization |
| **DAX**      | Custom calculations & KPIs            |
| **Power Query** | Data cleaning & transformation     |
| **Excel**    | Initial data preprocessing            |

## 📂 Datasets

### 1. Manufacturing Production Data
Fields:
- `ProductID`
- `ProductType`
- `ProductionDate`
- `ProductionCost`
- `CountryOfOrigin`
- `QuantityProduced`
- `WarehouseLocation`

### 2. Employee Performance Metrics
Fields:
- `EmployeeID`
- `Department`
- `HireDate`
- `Salary`
- `CountryOfOperation`
- `ProductID`
- `PerformanceRating`
- `TrainingRecords`

## 💻 Code Implementation

### Power Query (M Language) - Data Cleaning

```m
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
