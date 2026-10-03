
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

## 2. Running Total Sales

### Measure
```dax
Running Total Sales = 
CALCULATE(
    [Total Sales],
    FILTER(
        ALL(Dim_Date[date]),
        Dim_Date[date] <= MAX(Dim_Date[date])
    )
)
```

### Copilot Suggestion
The running total measure was implemented using DAX with the date dimension. The calculation uses the current date as the upper limit and includes all dates up to that date.

### Correction / Rewrite
The measure was tested in a Power BI table visual. The running total was checked across the dates to make sure that the sales value accumulated progressively instead of showing only the sales for the current date. 

No major correction was required after testing the measure.

### Result
The measure successfully produced a cumulative sales value that increased as the date progressed.

### Learning
A running total can be created by removing the individual date filter and then filtering the date dimension to include all dates up to the current date.

## 3. Product Rank using RANKX

### Measure
```dax
Product Rank = 
RANKX(
    ALL(Dim_Product[item]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
```

### Copilot's First Suggestion
GitHub Copilot suggested the following approach using ALLSELECTED:

```dax
Product Rank = 
RANKX(
    ALLSELECTED(Dim_Product[item]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
```

Copilot also explained that this would rank products according to their Total Sales, with the highest sales receiving Rank 1.

### Correction
The ALLSELECTED version was tested in the Power BI table visual. 

The result showed Rank 1 for every product in the tested visual, so the measure was not producing the expected ranking. 

The measure was therefore rewritten using ALL:

```dax
Product Rank = 
RANKX(
    ALL(Dim_Product[item]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
```

### Result
After replacing ALLSELECTED with ALL, the measure produced different ranking values for different products. 

The products were ranked according to their Total Sales, with the highest-selling product receiving Rank 1.

### Learning
The test showed that filter context is important when using RANKX. The generated DAX should always be tested in the actual report because the result can change depending on the filters and fields used in the visual.


## 4. Average Sales per Transaction

### Measure
```dax
Average Sales per Transaction = 
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(Fact_Sales[sale_id])
)
```

### Copilot Suggestion
The measure was created to calculate the average sales value for each transaction. 

The calculation uses the existing `[Total Sales]` measure and counts the unique transaction IDs from `Fact_Sales[sale_id]`.

### Correction / Rewrite
The measure was tested in Power BI using the product item along with Total Sales and Average Sales per Transaction. 

No major correction was required after testing because the measure produced reasonable values for the different products.

### Result
The measure successfully calculated the average sales per transaction for each product. 

Examples from testing:
* **Brownie:** 163.62
* **Cappuccino:** 207.14
* **Coffee Beans Pack:** 668.95
* **Cold Brew:** 361.25
* **Tumbler:** 841.22

### Learning
The measure shows how much sales revenue is generated on average per transaction. It uses `DISTINCTCOUNT` on the transaction ID so that each transaction is counted once.

