# 📘 Power BI Dashboard – DEX DEPO Analysis  
**File Name:** `PR.3 DEX DEPO.pbix`

---

## 📌 Project Objective

The dashboard provides an advanced, matrix-based analytical view of sales data across multiple dimensions such as Region, Month, Product Category, and Customer Segment. It demonstrates DAX mastery including advanced measures, time intelligence, conditional logic, iterator functions, and dimension lookups — strictly using **Matrix visualizations only**, as per business requirement.

---

## 🧩 Data Model Overview

**Fact Table:**  
- `Sales`: SalesAmount, Quantity, UnitPrice, OrderDate, ProductID, CustomerID, RegionID

**Dimension Tables:**  
- `Date`  
- `Product` (ProductID, Category)  
- `Customer` (CustomerID, Segment)  
- `Region` (RegionID, RegionName)

---

## 📈 DAX Measures & Logic Implemented

### 🔹 1. Total Sales
```DAX
Total Sales = SUM(Sales_Dim[SalesAmount])
```

---

### 🔹 2. Sales Category using `SWITCH()`
```DAX
Sales Category = 
SWITCH(
    TRUE(),
    [SalesAmount] > 1000, "High",
    [SalesAmount] > 500 "Average",
    [SalesAmount] > 200 , "Low",
"Poor"
)
```

---

### 🔹 3. Iterator Functions

#### ✅ `SUMX()` – Row-wise Total Sales
```DAX
SumX Sales = 
SUMX(
    VALUES(Sales_Dim[ProductID]),
    Sales_Dim[Quantity] * Sales_Dim[UnitPrice]
)
```

#### ✅ `AVERAGEX()` – Avg. Sales per Customer
```DAX
Average Sales per Customer = 
AVERAGEX(
    VALUES(Sales_Dim[CustomerID]),
    [Total Sales]
)
```

---

### 🔹 4. Filter Context & Overrides

#### ✅ Ignore Region Filter using `ALL()`
```DAX
All Region Sales = 
CALCULATE(
    [SalesAmount],
    ALL(Sales_Dim[RegionID])
)
```

#### ✅ Forced Filter using `FILTER()`
```DAX
North Only Sales = 
CALCULATE(
    [Total Sales],
    FILTER(ALL(Sales_Dim), Sales_Dim[RegionID] = 101)
)
```

---



#### ✅ Last 3 Months Sales
```DAX
Last 3 Months Sales = 
CALCULATE(
    [Sales Amount],
    DATESINPERIOD('Date'[Date], MAX('Date'[Date]), -3, MONTH)
)
```

#### ✅ Running Total Sales
```DAX
Running Total Sales = 
CALCULATE(
    [Total Sales],
    DATESBETWEEN(
        'Date_Dim'[Date],
        DATE(YEAR(MIN('Date_Dim'[Date])), 1, 1),
        MAX('Date_Dim'[Date])
    )
)
```

---

### 🔹 6. Text & Date Manipulations

```DAX
Full Name = CONCATENATE(RELATED(Customer_Dim[FirstName]), " " & RELATED(Customer_Dim[LastName]))
Region Upper = UPPER(RELATED(Region_Dim[RegionName]))
Order Year = YEAR(Sales_Dim[OrderDate])
Order Month = MONTH(Sales_Dim[OrderDate])
```

---

## 🧠 Business Insights Derived

- Categorized sales performance into Low, Medium, High bands
- Comparison between current and last year sales
- Running totals for cumulative monitoring
- Region-wise and segment-wise contributions
- Accurate customer-level and product-level profitability analysis

---

## 📊 Matrix Visualization Layout

**Matrix Rows:**
- Region (from Region table)
- Month (from Date table)
- Product Category (from Product table)
- Customer Segment (from Customer table)

**Matrix Values:**
- Total Sales  
- Sales Category  
- All Region Sales  
- North Only Sales  
- Last Year Sales  
- Last 3 Months Sales  
- Running Total  
- Average Sales per Customer  
- SumX Sales  

✅ **No other visuals used**  
✅ Strictly Matrix-based reporting

---

## 🔧 Tools & Stack

- **Platform:** Microsoft Power BI
- **Data Modeling:** Star schema with fact/dimension relationships
- **DAX Used:** SWITCH, CALCULATE, FILTER, SUMX, AVERAGEX, SAMEPERIODLASTYEAR, DATESINPERIOD, DATESBETWEEN, CONCATENATE, UPPER, YEAR, MONTH, RELATED

---

## 📝 Author Info

**Prepared by:** Mihir Limje  
**Version:** 1.0  
**Created On:** June 2025

---

## ✅ Notes

- All relationships are assumed active and correctly marked (e.g., Date table marked as Date Table).
- Replace RegionID `101` with actual North ID in your dataset.
- Clean and structured data model ensures accurate DAX evaluation.
