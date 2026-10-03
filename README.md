# BrewMetrics BI

A version-controlled Power BI analytics solution for BrewMetrics Coffee Co., developed as a Business Intelligence mini-project.

## 1. Project Overview

The BrewMetrics BI project analyzes coffee shop sales transactions across multiple cities and store formats. The solution uses Power BI to provide interactive analysis of sales performance, product categories, Cold Brew trends, and city-level performance.

### Key Business Goals

- Track total sales across cities and store formats
- Analyze sales by product category and item
- Monitor month-over-month sales growth
- Rank products based on total sales
- Explore sales using interactive filters and drill-downs

---

## 2. Power BI Data Model

The project follows a star schema with one central fact table and three dimension tables.

### Schema Overview

```text
                    Dim_Date
                       |
                       |
                       v
Dim_City -------> Fact_Sales <------- Dim_Product
```

### Tables

#### Fact_Sales
The central transactional fact table containing the sales records. Key columns include:

- `sale_id` – unique transaction identifier
- `date` – transaction date
- `city` – city where the sale occurred
- `store_format` – Flagship, Kiosk, or Drive-Thru
- `category` – Coffee, Bakery, or Merchandise
- `item` – specific product
- `quantity` – quantity sold
- `unit_price` – price per unit
- `sales_amount` – transaction sales amount

#### Dim_Date
The date dimension supports time-based analysis and time-intelligence calculations. It contains:
- `date`
- `Year`
- `Month`
- `Day`

#### Dim_City
Contains the distinct cities used for geographic analysis.

#### Dim_Product
Contains distinct product items and their corresponding categories.

### Relationships

The model contains one-to-many relationships from the dimension tables to the fact table:

- `Dim_Date[date]` → `Fact_Sales[date]`
- `Dim_City[city]` → `Fact_Sales[city]`
- `Dim_Product[item]` → `Fact_Sales[item]`

---

## 3. DAX Measures

The semantic model contains the following measures:

### Total Sales
Calculates total sales for the current filter context.
```dax
Total Sales = SUM(Fact_Sales[sales_amount])
```

### Previous Month Sales
Returns sales for the previous month.
```dax
Previous Month Sales = 
CALCULATE(
    [Total Sales],
    DATEADD(Dim_Date[date], -1, MONTH)
)
```

### MoM Growth %
Calculates month-over-month sales growth.
```dax
MoM Growth % = 
DIVIDE(
    [Total Sales] - [Previous Month Sales],
    [Previous Month Sales]
)
```

### Running Total Sales
Calculates cumulative sales over time.
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

### Product Rank
Ranks products according to total sales, with the highest sales receiving rank 1.
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

### Average Sales per Transaction
Calculates the average sales value per unique transaction.
```dax
Average Sales per Transaction = 
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(Fact_Sales[sale_id])
)
```

---

## 4. Key Business Insights

The dashboard and project data provide the following observations:

- **City-level performance:** The project specification identifies Bengaluru as consistently outperforming the other three cities in sales performance.
- **Month-over-month sales movement:** During testing, May recorded sales of approximately ₹14.27 lakh, representing 9.45% growth compared with April. June recorded approximately ₹11.96 lakh, representing a 16.18% decline compared with May.
- **Cold Brew seasonal pattern:** The dashboard includes a dedicated Cold Brew sales trend so that the seasonal sales pattern identified in the project data can be explored over time.

---

## 5. Dashboard Features

The final Power BI dashboard provides an interactive view of BrewMetrics sales performance.

### Key Visualizations
- **Cold Brew Sales Trend** – tracks Cold Brew sales over time
- **Total Sales by City** – compares sales across cities
- **Sales by City and Store Format** – supports City → Store Format drill-down
- **Sales by Category** – compares sales across product categories
- **Total Sales KPI** – displays overall sales
- **Total Transactions KPI** – displays the number of transactions

### Interactive Filters
- **Filter by City** – filters the dashboard by city
- **Filter by Category** – filters the dashboard by product category

### Drill-Down
The dashboard includes a **City → Store Format** drill-down hierarchy, allowing users to move from overall city performance to individual store-format performance.

---

## 6. Data Source

The project uses the `brewmetrics_sales.csv` transactional sales dataset supplied for the mini-project. 

The dataset contains approximately **15,500 transactions** and includes information about dates, cities, store formats, product categories, products, quantities, prices, and sales amounts.

---

## 7. Version Control

The project is saved as a Power BI Project (`.pbip`) and maintained using Git and GitHub. 

The development history contains separate commits for:
1. Initial project setup
2. Star schema creation
3. Individual DAX measures
4. Dashboard development
5. Documentation updates

This allows changes to the BI solution to be traced and reviewed throughout development.

---

## 8. Project Structure

```text
brewmetrics-bi/
├── BrewMetrics.pbip
├── BrewMetrics.Report/
├── BrewMetrics.SemanticModel/
├── brewmetrics_sales.csv
├── README.md
└── NOTES.md
```

---

## 9. Conclusion

The BrewMetrics BI project demonstrates how Power BI, dimensional modeling, DAX, and Git-based version control can be combined to create an auditable Business Intelligence solution. 

The final dashboard provides interactive sales analysis through KPIs, charts, slicers, and drill-down functionality while the Git history documents the development of the BI solution step by step.
