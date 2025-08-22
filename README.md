# 📊 Manufacturing Efficiency Dashboard
**Enhancing Manufacturing Performance Through Data-Driven Insights**
<img width="1468" height="798" alt="image" src="https://github.com/user-attachments/assets/f14190b5-4555-4387-9ccc-f76992314234" />


> Replace the image path above with your actual screenshot (e.g., from your repo’s `assets/` folder).

---

## 🧭 Table of Contents
- [Project Overview](#-project-overview)
- [Objectives](#-objectives)
- [Tools & Technologies](#-tools--technologies)
- [Data Sources & Schemas](#-data-sources--schemas)
- [Data Preparation (Power Query / M)](#-data-preparation-power-query--m)
- [Data Model](#-data-model)
- [DAX Measures & Calculated Columns](#-dax-measures--calculated-columns)
- [KPIs & Insights](#-kpis--insights)
- [Dashboard Features](#-dashboard-features)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Performance Tips](#-performance-tips)
- [Roadmap](#-roadmap)
- [License](#-license)
- [Tags](#-tags)

---

## 🔍 Project Overview
This project leverages **manufacturing production data** and **employee performance metrics** to uncover inefficiencies, identify trends, and drive operational improvements. Built using **Power BI**, the dashboard offers a centralized, interactive view of key factors influencing manufacturing efficiency — including production costs, workforce productivity, and training investments.

By visualizing these elements together, stakeholders can make informed decisions that align cost management with workforce optimization and strategic growth.

---

## 🎯 Objectives
- Analyze **production cost trends**, **output quantities**, and **efficiency ratios** across product lines and locations.
- Evaluate how **employee training** and **performance ratings** impact overall productivity.
- Investigate the relationship between **salary levels** and **workforce output** for better HR planning.
- Deliver **actionable insights** through intuitive dashboards to support leadership decision-making.

---

## 🛠️ Tools & Technologies

| Tool            | Purpose                                  |
|-----------------|-------------------------------------------|
| **Power BI**    | Dashboard development & visualization     |
| **DAX**         | Custom calculations & KPIs                |
| **Power Query** | Data cleaning & transformation            |
| **Excel**       | Initial data preprocessing                |

---

## 📂 Data Sources & Schemas

### 1) Manufacturing Production Data
Fields:
- `ProductID`
- `ProductType`
- `ProductionDate`
- `ProductionCost`
- `CountryOfOrigin`
- `QuantityProduced`
- `WarehouseLocation`

### 2) Employee Performance Metrics
Fields:
- `EmployeeID`
- `Department`
- `HireDate`
- `Salary`
- `CountryOfOperation`
- `ProductID`
- `PerformanceRating`
- `TrainingRecords` *(e.g., hours or count of sessions)*

> ✅ Ensure `ProductID` is a shared key for modeling production ↔ employee relationships where relevant.

---

## 🧹 Data Preparation (Power Query / M)

> **Files expected**: `Manufacturing_Data.xlsx` (sheet: `Production`) and `Employee_Data.xlsx` (sheet: `Employee`).
> Update file paths if your data lives elsewhere (e.g., SharePoint, OneDrive, SQL).

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
