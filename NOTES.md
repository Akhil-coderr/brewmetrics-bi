
# Copilot Notes

## 1. MoM Growth %

### Measure
```dax
MoM Growth % = 
DIVIDE(
    [Total Sales] - [Previous Month Sales],
    [Previous Month Sales]
)
```

### Copilot Suggestion
The MoM measure was initially implemented using the DAX logic for comparing the current month's sales with the previous month's sales.

The supporting measure was:

### Code snippet
```dax
Previous Month Sales = 
CALCULATE(
    [Total Sales], 
    DATEADD(Dim_Date[date], -1, MONTH)
)
```

### Correction / Rewrite
During testing, the main issue was with the date context used in the table visual. The numeric Month field was producing an incorrect aggregation instead of displaying the months correctly. The visual was changed to use the Date hierarchy with Year and Month so that the `DATEADD` calculation could evaluate the previous month correctly.

### Result
After correcting the date context, the measure produced the expected month-over-month results. 

Examples from testing:
* **May:** 9.45% growth compared with April
* **June:** -16.18% growth compared with May

### Learning
The MoM calculation depends on the correct date context. The date column from the date dimension should be used for time-intelligence calculations instead of relying on an aggregated numeric month field.

