# 📊 Power Query Data Preparation Documentation

 
**Prepared by:** Dominik Szczepkowski 
**Dataset** https://www.kaggle.com/datasets/algorismus/adventure-works-in-excel-tables

---

## 🔷 1. SalesTable


**Format:**  
- CSV (space-delimited)  
- 10 columns  
- Encoding: Windows-1250  
- No quoted values

### 🔧 Applied Transformations:

| Step | Action | Description |
|------|--------|-------------|
| 1 | **Source** | Import CSV using specified format and settings. |
| 2 | **Promoted Headers** | Use the first row as column headers. |
| 3 | **Changed Type** | Assign appropriate types:<br>• `SalesOrderNumber`: Text<br>• `OrderDate`: Date<br>• `ProductKey`, `ResellerKey`, `EmployeeKey`, `SalesTerritoryKey`, `Quantity`: Int64<br>• `Unit Price`, `Sales`, `Cost`: Text |
| 4 | **Replaced Value** | Remove `$` from `Unit Price`. |
| 5 | **Replaced Value** | Remove `,` from `Unit Price`. |
| 6 | **Replaced Value** | Replace `.` with `,` in `Unit Price`. |
| 7 | **Changed Type** | Convert `Unit Price` to type `number`. |
| 8 | **Replaced Value** | Remove `$` from `Sales`. |
| 9 | **Replaced Value** | Remove `,` from `Sales`. |
| 10 | **Replaced Value** | Replace `.` with `,` in `Sales`. |
| 11 | **Changed Type** | Convert `Sales` to type `number`. |
| 12 | **Replaced Value** | Remove `$` from `Cost`. |
| 13 | **Replaced Value** | Remove `,` from `Cost`. |
| 14 | **Replaced Value** | Replace `.` with `,` in `Cost`. |
| 15 | **Changed Type** | Convert `Cost` to type `number`. |

### ✅ Outcome

The dataset is fully cleaned:
- Monetary values (`Unit Price`, `Sales`, `Cost`) are numeric.
- Regional number formatting is applied.
- All columns are typed correctly and ready for reporting and analysis.

---

## 🔷 2. TargetsTable

**Source File:**  
`C:\Users\domin\OneDrive\Pulpit\Adveture Works\Dataset\Targets.csv`

**Format:**  
- CSV (tab-delimited)  
- 3 columns  
- Encoding: Windows-1250  
- No quoted values

### 🔧 Applied Transformations:

| Step | Action | Description |
|------|--------|-------------|
| 1 | **Source** | Import CSV using specified format and settings. |
| 2 | **Promoted Headers** | Promote the first row as headers. |
| 3 | **Changed Type** | Assign types:<br>• `EmployeeID`: Int64<br>• `Target`: Text<br>• `TargetMonth`: Date |
| 4 | **Replaced Value** | Remove `$` from `Target`. |
| 5 | **Replaced Value** | Remove `,` from `Target`. |
| 6 | **Changed Type** | Convert `Target` to Int64. |

### ✅ Outcome

- Targets are numeric and date fields are correctly typed.
- Table is ready for analysis by employee and time period.

---

## 📌 Final Notes

Both `SalesTable` and `TargetsTable` are:
- Cleaned and standardized.
- Converted to analysis-ready formats.
- Compatible with DAX measures and Power BI visuals.

