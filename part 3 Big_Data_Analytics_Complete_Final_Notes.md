# Big Data Analytics - COMPLETE Notes (Final Part)

---

## UNIT V: TABLEAU - CASE STUDIES & ADVANCED FEATURES (Continued)

### Case Study 3: Supply Chain Analytics

**Objective**: Optimize inventory and reduce costs

**Datasets Required**:
```
1. Orders (Order ID, Date, Product, Quantity, Customer)
2. Inventory (Product, Stock Level, Warehouse)
3. Suppliers (Supplier, Lead Time, Cost)
4. Shipments (Order ID, Ship Date, Delivery Date, Status)
```

**Key Metrics to Calculate**:

**1. Inventory Turnover**:
```
Calculated Field:
[Inventory Turnover] = 
SUM([Quantity Sold]) / AVG([Stock Level])

High turnover (>6): Fast-moving, efficient
Low turnover (<2): Slow-moving, excess inventory

Visualization:
- Bar chart by Product Category
- Color: Red (low), Yellow (medium), Green (high)
- Reference line at industry average
```

**2. Days of Inventory**:
```
[Days of Inventory] = 
AVG([Stock Level]) / (SUM([Quantity Sold]) / 365)

Shows how many days current inventory will last

Visualization:
- Heat map: Product × Warehouse
- Color intensity: Days of inventory
- Highlight products with <30 or >90 days
```

**3. Order Fulfillment Rate**:
```
[On-Time Delivery %] = 
COUNTD(IF [Ship Date] <= [Promised Date] 
       THEN [Order ID] END) / 
COUNTD([Order ID])

Target: >95%

Visualization:
- Line chart: Trend over time
- Reference line at 95% target
- Alert annotation when below threshold
```

**4. Stockout Analysis**:
```
[Stockout Events] = 
IF [Stock Level] = 0 THEN 1 ELSE 0 END

[Lost Revenue] = 
IF [Stock Level] = 0 
THEN [Avg Unit Price] * [Avg Daily Demand] 
ELSE 0 END

Visualization:
- Gantt chart showing stockout periods
- Bar chart: Lost revenue by product
```

**Dashboard Layout**:
```
+------------------------------------------+
| SUPPLY CHAIN DASHBOARD                   |
+------------------------------------------+
| KPI Cards:                               |
| ┌──────┬──────┬──────┬──────┐          |
| │Inven-│Fill  │Stock-│Lead  │          |
| │tory  │Rate  │outs  │Time  │          |
| │Turn  │      │      │      │          |
| └──────┴──────┴──────┴──────┘          |
+------------------------------------------+
| Inventory Turnover by Category (Bar)     |
+------------------------------------------+
| Stock Levels Heat Map (Product×Warehouse)|
+------------------------------------------+
| ┌─────────────────┬──────────────────┐  |
| │ Stockout Events │ Supplier         │  |
| │ Timeline        │ Performance      │  |
| │ (Gantt)         │ (Scatter)        │  |
| └─────────────────┴──────────────────┘  |
+------------------------------------------+
```

**Advanced Features**:

**1. Alerts**:
```
Create calculated field for alert:
[Stock Alert] = 
IF [Stock Level] < [Reorder Point] 
THEN "⚠️ Reorder Now"
ELSEIF [Stock Level] < [Safety Stock]
THEN "⚡ Low Stock"
ELSE "✓ Sufficient"
END

Display in tooltip or as label
Filter to show only alerts
```

**2. What-If Analysis**:
```
Parameters:
- Lead Time Reduction (%)
- Demand Increase (%)
- Target Service Level (%)

Calculated Field:
[Adjusted Reorder Point] = 
[Current Reorder Point] * 
(1 + [Demand Increase %]) * 
(1 - [Lead Time Reduction %])

Show current vs adjusted scenarios
```

**3. ABC Classification**:
```
Calculated Fields:

Cumulative Revenue:
RUNNING_SUM(SUM([Revenue]))

Cumulative % Revenue:
[Cumulative Revenue] / TOTAL(SUM([Revenue]))

ABC Class:
IF [Cumulative % Revenue] <= 0.8 THEN "A"
ELSEIF [Cumulative % Revenue] <= 0.95 THEN "B"
ELSE "C"
END

Visualization:
- Pareto chart (bar + line)
- Bars: Revenue by product (sorted)
- Line: Cumulative percentage
- Color by ABC class
```

### Case Study 4: Marketing Campaign Analysis

**Objective**: Measure campaign effectiveness and ROI

**Data Structure**:
```
Campaigns Table:
- Campaign ID
- Name
- Start Date
- End Date
- Budget
- Channel (Email, Social, PPC, Display)
- Target Audience

Conversions Table:
- Campaign ID
- Conversion Date
- Customer ID
- Revenue
- Conversion Type (Lead, Sale, Sign-up)
```

**Key Metrics**:

**1. ROI (Return on Investment)**:
```
[Campaign ROI] = 
(SUM([Revenue]) - SUM([Budget])) / SUM([Budget])

Display as percentage
Color: Green (positive), Red (negative)

[ROI Category] = 
IF [Campaign ROI] > 2 THEN "Excellent"
ELSEIF [Campaign ROI] > 1 THEN "Good"
ELSEIF [Campaign ROI] > 0 THEN "Break-even"
ELSE "Loss"
END
```

**2. Cost Per Acquisition (CPA)**:
```
[CPA] = 
SUM([Budget]) / COUNTD([Customer ID])

Lower is better

Benchmark:
[vs Target] = 
([CPA] - [Target CPA]) / [Target CPA]

Display with bullet chart
```

**3. Conversion Funnel**:
```
Stages:
1. Impressions
2. Clicks
3. Visits
4. Leads
5. Sales

Conversion Rates:
[Click Rate] = [Clicks] / [Impressions]
[Visit Rate] = [Visits] / [Clicks]
[Lead Rate] = [Leads] / [Visits]
[Sale Rate] = [Sales] / [Leads]

Visualization:
- Funnel chart (custom shape)
- Show count and % at each stage
- Highlight drop-off points
```

**4. Attribution Analysis**:
```
Multi-touch attribution models:

First Touch:
IF [Touch Sequence] = 1 THEN [Revenue] ELSE 0 END

Last Touch:
IF [Touch Sequence] = [Max Sequence] THEN [Revenue] ELSE 0 END

Linear Attribution:
[Revenue] / [Total Touches]

Time Decay (more recent = more credit):
[Revenue] * POWER(2, [Days Since Touch] / 7) / 
SUM(POWER(2, [Days Since Touch] / 7))
```

**Campaign Dashboard**:

**Layout**:
```
+------------------------------------------+
| MARKETING CAMPAIGN PERFORMANCE           |
+------------------------------------------+
| Date Range Filter: [___________] [Apply] |
+------------------------------------------+
| ┌──────┬──────┬──────┬──────┬──────┐   |
| │Total │ROI   │CPA   │Conv  │Active│   |
| │Spend │      │      │Rate  │Camp. │   |
| └──────┴──────┴──────┴──────┴──────┘   |
+------------------------------------------+
| ROI by Channel (Bar)                     |
| [Email] [Social] [PPC] [Display]         |
+------------------------------------------+
| ┌──────────────────┬─────────────────┐  |
| │ Campaign         │ Conversion      │  |
| │ Performance      │ Funnel          │  |
| │ (Scatter)        │                 │  |
| │ Size: Budget     │ [Wide Funnel]   │  |
| │ Color: ROI       │                 │  |
| └──────────────────┴─────────────────┘  |
+------------------------------------------+
| Attribution Model Comparison (Bar)       |
| [First] [Last] [Linear] [Time Decay]     |
+------------------------------------------+
```

**Interactive Elements**:

**1. Filter Actions**:
```
Click on channel → Filter all sheets to that channel
Click on campaign → Show detailed breakdown
Hover on data point → Show campaign details in tooltip
```

**2. Parameter Controls**:
```
Select Attribution Model: [Dropdown]
Date Aggregation: Day/Week/Month [Radio buttons]
Top N Campaigns: [Slider: 5-50]
```

**3. Drill-Down Hierarchy**:
```
Channel → Campaign → Ad Group → Individual Ad

Click + to expand
Click - to collapse
```

**Advanced Analytics**:

**Cohort Analysis**:
```
Group customers by acquisition month
Track behavior over time

Calculated Fields:

Cohort Month:
DATETRUNC('month', {FIXED [Customer ID]: MIN([First Purchase Date])})

Months Since Acquisition:
DATEDIFF('month', [Cohort Month], [Purchase Date])

Retention Rate:
COUNTD([Customer ID]) / 
WINDOW_SUM(COUNTD([Customer ID]), FIRST(), 0)

Visualization:
- Heat map: Cohort Month × Months Since
- Color: Retention rate
- Shows which campaigns brought loyal customers
```

### Case Study 5: Financial Performance Dashboard

**Objective**: Executive-level financial overview

**Data Sources**:
```
1. P&L Statement (monthly)
2. Balance Sheet
3. Cash Flow Statement
4. Budget vs Actual
5. Departmental breakdown
```

**Key Financial Metrics**:

**1. Profitability Metrics**:
```
Gross Profit Margin:
([Revenue] - [COGS]) / [Revenue]

Operating Margin:
([Revenue] - [COGS] - [Operating Expenses]) / [Revenue]

Net Profit Margin:
[Net Income] / [Revenue]

EBITDA:
[Operating Income] + [Depreciation] + [Amortization]

Visualization:
- Waterfall chart: Revenue → COGS → OpEx → Net Income
- Line chart: Margin trends over time
- Bullet chart: Actual vs Budget
```

**2. Liquidity Metrics**:
```
Current Ratio:
[Current Assets] / [Current Liabilities]

Quick Ratio:
([Current Assets] - [Inventory]) / [Current Liabilities]

Cash Conversion Cycle:
[Days Inventory] + [Days Receivable] - [Days Payable]

Working Capital:
[Current Assets] - [Current Liabilities]

Visualization:
- Gauge charts for ratios
- Trend lines for working capital
- Comparison to industry benchmarks
```

**3. Variance Analysis**:
```
Budget Variance:
[Actual] - [Budget]

Variance %:
([Actual] - [Budget]) / [Budget]

Variance Category:
IF [Variance %] > 0.1 THEN "Favorable"
ELSEIF [Variance %] < -0.1 THEN "Unfavorable"
ELSE "On Track"
END

Visualization:
- Bullet chart: Actual vs Budget vs Prior Year
- Heat map: Department × Month (variance %)
- Bar chart: Largest variances (sorted)
```

**4. Growth Metrics**:
```
Revenue Growth:
(SUM([Revenue Current]) - SUM([Revenue Prior])) / 
SUM([Revenue Prior])

YoY Growth:
(LOOKUP(SUM([Revenue]), 0) - LOOKUP(SUM([Revenue]), -12)) / 
LOOKUP(SUM([Revenue]), -12)

CAGR (Compound Annual Growth Rate):
POWER([Revenue End] / [Revenue Start], 1/[Years]) - 1

Visualization:
- Line chart with YoY comparison
- Area chart: Revenue breakdown by segment
- Small multiples: Growth by department
```

**Executive Dashboard Layout**:
```
+------------------------------------------+
| EXECUTIVE FINANCIAL DASHBOARD            |
| Q4 2024                                  |
+------------------------------------------+
| Key Metrics:                             |
| ┌─────┬─────┬─────┬─────┬─────┬─────┐  |
| │Rev  │Gross│Op   │Net  │Cash │EPS  │  |
| │$15M │Mrgn │Mrgn │Inc  │     │     │  |
| │↑8%  │42%  │18%  │$2M  │$5M  │$1.2 │  |
| └─────┴─────┴─────┴─────┴─────┴─────┘  |
+------------------------------------------+
| Revenue Waterfall (Bridge Chart)         |
| [Prior Year] → [Growth] → [Current Year] |
+------------------------------------------+
| ┌──────────────────┬─────────────────┐  |
| │ P&L Trend        │ Budget Variance │  |
| │ (Line chart)     │ (Bullet chart)  │  |
| │                  │                 │  |
| │ Revenue, COGS,   │ By Department   │  |
| │ Expenses, Profit │                 │  |
| └──────────────────┴─────────────────┘  |
+------------------------------------------+
| Top Variances (Bar chart, sorted)        |
| Favorable | Unfavorable                  |
+------------------------------------------+
```

**Advanced Features**:

**1. Scenario Analysis**:
```
Parameters:
- Revenue Growth Assumption: 5%, 10%, 15%
- Cost Reduction Target: 0%, 5%, 10%
- Exchange Rate: 1.0, 1.05, 1.10

Calculated Fields:
[Projected Revenue] = 
[Current Revenue] * (1 + [Revenue Growth %])

[Projected Costs] = 
[Current Costs] * (1 - [Cost Reduction %])

[Projected Profit] = 
[Projected Revenue] - [Projected Costs]

Dashboard:
Show current + 3 scenarios side-by-side
Tornado chart for sensitivity analysis
```

**2. KPI Scorecards**:
```
For each KPI:
- Current value
- Target value
- % of target achieved
- Trend indicator (↑↓→)
- Sparkline (mini trend chart)
- Status (Green/Yellow/Red)

Conditional Formatting:
IF [% of Target] >= 1.0 THEN "🟢"
ELSEIF [% of Target] >= 0.9 THEN "🟡"
ELSE "🔴"
END
```

**3. Drill-Through Pages**:
```
Main dashboard → Click metric → Detail page

Detail pages:
- Revenue detail: By product, region, customer
- Expense detail: By department, category
- Variance detail: Line-by-line analysis

Navigation:
Add button: "Back to Main Dashboard"
Breadcrumb trail: Home > Revenue > Product Details
```

---

## ADVANCED TABLEAU FEATURES

### 1. Dashboard Actions

**Filter Actions**:
```
Purpose: Click on one viz to filter others

Setup:
Dashboard → Actions → Add Action → Filter

Configuration:
- Source sheet: Map
- Target sheets: All (or select specific)
- Run on: Select (click)
- Clearing selection: Show all values

Example:
Click on state in map → Filter all charts to that state
```

**Highlight Actions**:
```
Purpose: Emphasize related data across views

Setup:
Dashboard → Actions → Add Action → Highlight

Configuration:
- Source: Sales by Region
- Target: Product Sales
- Fields: Region (common field)

Example:
Hover on region → Highlights that region's products
```

**URL Actions**:
```
Purpose: Link to external resources

Setup:
Dashboard → Actions → Add Action → Go to URL

URL example:
https://www.google.com/search?q=<Product Name>

Use cases:
- Link to detailed reports
- Open customer profile
- Search product information
```

**Set Actions** (Interactive Sets):
```
Purpose: Dynamically create sets by clicking

Setup:
Dashboard → Actions → Add Action → Change Set Values

Configuration:
- Target Set: Create new set
- Run on: Select
- Clearing selection: Remove all from set

Example:
Click products to add to comparison set
Compare selected products side-by-side
```

### 2. Parameters and Dynamic Calculations

**Dynamic Measure Selector**:
```
Parameter: Select Metric
Values: Sales | Profit | Quantity | Discount

Calculated Field: [Selected Metric]
CASE [Select Metric]
  WHEN "Sales" THEN SUM([Sales])
  WHEN "Profit" THEN SUM([Profit])
  WHEN "Quantity" THEN SUM([Quantity])
  WHEN "Discount" THEN AVG([Discount])
END

Dynamic Title:
[Select Metric] & " by Category"

User Experience:
One chart, multiple metrics
Dropdown to switch
Chart updates automatically
```

**Dynamic Date Range**:
```
Parameter: Time Period
Values: Last 7 Days | Last 30 Days | Last Quarter | Last Year

Calculated Field: [Date Filter]
CASE [Time Period]
  WHEN "Last 7 Days" THEN 
    [Order Date] >= TODAY() - 7
  WHEN "Last 30 Days" THEN 
    [Order Date] >= TODAY() - 30
  WHEN "Last Quarter" THEN 
    [Order Date] >= DATEADD('quarter', -1, TODAY())
  WHEN "Last Year" THEN 
    [Order Date] >= DATEADD('year', -1, TODAY())
END

Add to Filters: [Date Filter] = True
```

**Dynamic Reference Lines**:
```
Parameter: Reference Type
Values: Average | Median | Target | Custom

Calculated Field:
CASE [Reference Type]
  WHEN "Average" THEN AVG([Sales])
  WHEN "Median" THEN MEDIAN([Sales])
  WHEN "Target" THEN [Target Value Parameter]
  WHEN "Custom" THEN [Custom Value Parameter]
END

Add as Reference Line
Label: [Reference Type] & ": " & STR([Reference Value])
```

### 3. Storytelling with Tableau Stories

**Creating a Story**:
```
Story → New Story

Story points = Slides

Example Story: Quarterly Business Review

Point 1: Executive Summary
- KPIs dashboard
- Caption: "Q4 2024 exceeded targets by 12%"

Point 2: Revenue Growth
- Revenue trend chart
- Caption: "Strong holiday season drove 25% increase"

Point 3: Regional Performance
- Map visualization
- Caption: "West region led with 18% growth"

Point 4: Product Analysis
- Top products chart
- Caption: "New product line contributed 15% of revenue"

Point 5: Action Items
- Summary and recommendations
- Caption: "Focus areas for Q1 2025"
```

**Story Best Practices**:
```
Structure:
1. Opening: Context and key question
2. Rising Action: Show trends and patterns
3. Climax: Key insight or finding
4. Resolution: Recommendations and next steps

Tips:
- One key message per story point
- Use annotations to guide viewers
- Progressive disclosure (reveal gradually)
- Clear captions that stand alone
- Consistent formatting
- Logical flow between points
```

### 4. Advanced Chart Types

**Sankey Diagram** (Flow):
```
Use Case: Customer journey, process flow

Create using:
1. Install extension from Extensions Gallery
2. Prepare data with From/To structure

Data Format:
From      | To        | Count
----------|-----------|------
Visitor   | Lead      | 1000
Lead      | Customer  | 300
Customer  | Repeat    | 150

Shows flow and drop-off rates visually
```

**Bullet Graph**:
```
Purpose: Actual vs Target with context

Components:
- Actual: Bar
- Target: Reference line
- Ranges: Background colors (poor/satisfactory/good)

Build:
1. Measure on Columns
2. Dimension on Rows
3. Add reference line (target)
4. Add reference distribution (ranges)

Example: Sales Performance
- Poor: 0-70% of target (red)
- Satisfactory: 70-90% (yellow)
- Good: 90%+ (green)
- Actual: Black bar
- Target: Vertical line
```

**Calendar Heatmap**:
```
Purpose: Activity over time (like GitHub contributions)

Build:
1. Columns: WEEK(Date)
2. Rows: WEEKDAY(Date)
3. Color: Measure
4. Mark type: Square
5. Size: 90% of view

Shows patterns:
- Day of week trends
- Seasonal patterns
- Outliers and gaps
```

**Network Diagram**:
```
Purpose: Relationships and connections

Use Case:
- Social network analysis
- Product associations
- Organizational structure

Requires:
- Network visualization extension
- Node and edge data

Data Format:
Nodes: ID, Name, Group
Edges: From ID, To ID, Weight
```

### 5. Performance Optimization

**Best Practices**:

**1. Data Source Optimization**:
```
Extract instead of Live (when possible)
- Faster query performance
- Reduces database load
- Enable incremental refresh

Extract Filters:
- Reduce extract size
- Filter out old/unnecessary data
- Example: Last 2 years only

Aggregation:
- Pre-aggregate in extract
- Use materialized calculations
- Hidden fields not extracted
```

**2. Calculation Optimization**:
```
Avoid:
- Row-level calcs in views (pre-calculate)
- Complex LOD expressions (simplify)
- Multiple COUNTD on large datasets

Prefer:
- Aggregate calculations
- Simple LOD when necessary
- Database calculations (custom SQL)

Example - Instead of:
IF [Sales] > 1000 THEN "High" ELSE "Low" END
(Calculated in view for every row)

Pre-calculate:
Add field in data source or extract
```

**3. View Design**:
```
Reduce:
- Number of marks (filter, aggregate)
- Number of sheets in dashboard
- Dashboard size (fit to window)

Optimize:
- Use fixed-size dashboards
- Limit quick filters
- Use context filters
- Reduce number of colors

Monitor:
Performance Recording:
Help → Settings and Performance → Start Performance Recording
Perform actions
Stop recording
Review query times
```

**4. Dashboard Performance**:
```
Techniques:
- Use fewer data sources
- Minimize cross-data-source calculations
- Use actions instead of quick filters
- Hide unused sheets
- Optimize images (compress, reduce size)

Dashboard Load Time Target: <5 seconds
```

**Performance Checklist**:
```
□ Using extracts where appropriate
□ Extract filters applied
□ Incremental refresh configured
□ Calculations optimized
□ Hidden fields not extracted
□ Number of marks < 10,000
□ Context filters used
□ Unused fields hidden
□ Dashboard size optimized
□ Images compressed
□ Performance recording checked
```

---

## R PROGRAMMING FOR BIG DATA

### 1. R Basics - Quick Reference

**Data Types**:
```R
# Numeric
x <- 42
y <- 3.14

# Integer
z <- 5L

# Character
name <- "Big Data"

# Logical
flag <- TRUE

# Complex
comp <- 3 + 2i

# Check type
class(x)    # "numeric"
typeof(x)   # "double"
```

**Data Structures**:

**Vectors**:
```R
# Create vector
numbers <- c(1, 2, 3, 4, 5)
names <- c("Alice", "Bob", "Charlie")

# Sequences
seq1 <- 1:10
seq2 <- seq(1, 10, by=2)  # 1, 3, 5, 7, 9

# Repeat
rep1 <- rep(1, 5)  # 1, 1, 1, 1, 1
rep2 <- rep(c(1,2), 3)  # 1, 2, 1, 2, 1, 2

# Access elements
numbers[1]      # First element (1-indexed!)
numbers[c(1,3)] # Elements 1 and 3
numbers[-1]     # All except first

# Operations
numbers * 2     # Element-wise multiplication
sum(numbers)    # Sum of all elements
mean(numbers)   # Average
```

**Matrices**:
```R
# Create matrix
m <- matrix(1:9, nrow=3, ncol=3)
#      [,1] [,2] [,3]
# [1,]    1    4    7
# [2,]    2    5    8
# [3,]    3    6    9

# By row
m2 <- matrix(1:9, nrow=3, byrow=TRUE)

# Access elements
m[1, 2]       # Row 1, Column 2
m[1, ]        # Entire row 1
m[, 2]        # Entire column 2
m[1:2, 2:3]   # Subset

# Operations
t(m)          # Transpose
m %*% m       # Matrix multiplication
```

**Data Frames**:
```R
# Create data frame
df <- data.frame(
  Name = c("Alice", "Bob", "Charlie"),
  Age = c(25, 30, 35),
  Salary = c(50000, 60000, 70000)
)

# View
head(df)      # First 6 rows
tail(df)      # Last 6 rows
str(df)       # Structure
summary(df)   # Summary statistics
dim(df)       # Dimensions (rows, cols)

# Access
df$Name               # Column by name
df[, "Age"]          # Column by name
df[1, ]              # First row
df[df$Age > 25, ]    # Filter rows

# Add column
df$Bonus <- df$Salary * 0.1

# Add row
new_row <- data.frame(Name="David", Age=28, Salary=55000)
df <- rbind(df, new_row)
```

**Lists**:
```R
# Create list (can hold different types)
my_list <- list(
  numbers = c(1, 2, 3),
  name = "Analysis",
  data = df,
  flag = TRUE
)

# Access
my_list$numbers
my_list[[1]]
my_list[["name"]]
```

### 2. Data Manipulation with dplyr

**Installation**:
```R
install.packages("dplyr")
library(dplyr)
```

**Core Functions**:

**1. filter() - Filter rows**:
```R
# Filter by condition
high_earners <- filter(df, Salary > 55000)

# Multiple conditions
young_high_earners <- filter(df, Age < 30, Salary > 50000)

# OR condition
filter(df, Age < 25 | Age > 35)
```

**2. select() - Select columns**:
```R
# Select specific columns
select(df, Name, Salary)

# Select range
select(df, Name:Age)

# Exclude columns
select(df, -Bonus)

# Helper functions
select(df, starts_with("S"))   # Salary
select(df, ends_with("e"))     # Name, Age
select(df, contains("al"))     # Salary
```

**3. mutate() - Create/modify columns**:
```R
# Add new column
df <- mutate(df, 
  Annual_Bonus = Salary * 0.15,
  Total_Comp = Salary + Annual_Bonus
)

# Transform existing
df <- mutate(df, 
  Salary = Salary * 1.05  # 5% raise
)

# Conditional mutation
df <- mutate(df,
  Level = case_when(
    Salary < 55000 ~ "Junior",
    Salary < 65000 ~ "Mid",
    TRUE ~ "Senior"
  )
)
```

**4. arrange() - Sort rows**:
```R
# Ascending
arrange(df, Salary)

# Descending
arrange(df, desc(Salary))

# Multiple columns
arrange(df, Level, desc(Salary))
```

**5. summarize() - Aggregate**:
```R
# Single summary
summarize(df, 
  avg_salary = mean(Salary),
  total_salary = sum(Salary)
)

# Multiple summaries
summarize(df,
  count = n(),
  avg_age = mean(Age),
  max_salary = max(Salary),
  min_salary = min(Salary)
)
```

**6. group_by() - Group operations**:
```R
# Group and summarize
df %>%
  group_by(Level) %>%
  summarize(
    count = n(),
    avg_salary = mean(Salary),
    total_salary = sum(Salary)
  )

# Multiple grouping variables
df %>%
  group_by(Level, Department) %>%
  summarize(avg_salary = mean(Salary))
```

**Pipe Operator %>%** (Chain operations):
```R
# Instead of nested functions:
result <- filter(arrange(select(df, Name, Salary), desc(Salary)), Salary > 50000)

# Use pipe for readability:
result <- df %>%
  select(Name, Salary) %>%
  arrange(desc(Salary)) %>%
  filter(Salary > 50000)

# Complex analysis example:
analysis <- df %>%
  filter(Age >= 25) %>%
  mutate(Tax = Salary * 0.25) %>%
  group_by(Level) %>%
  summarize(
    count = n(),
    avg_salary = mean(Salary),
    avg_tax = mean(Tax),
    total_comp = sum(Salary)
  ) %>%
  arrange(desc(avg_salary))
```

### 3. Data Visualization with ggplot2

**Installation**:
```R
install.packages("ggplot2")
library(ggplot2)
```

**ggplot2 Grammar**:
```
ggplot(data = <DATA>) +
  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>)) +
  <ADDITIONAL_LAYERS>
```

**Basic Plots**:

**1. Scatter Plot**:
```R
ggplot(data = df, aes(x = Age, y = Salary)) +
  geom_point()

# Add color by category
ggplot(df, aes(x = Age, y = Salary, color = Level)) +
  geom_point(size = 3)

# Add size by variable
ggplot(df, aes(x = Age, y = Salary, 
               color = Level, size = Bonus)) +
  geom_point(alpha = 0.7)
```

**2. Bar Chart**:
```R
# Count by category
ggplot(df, aes(x = Level)) +
  geom_bar()

# Aggregated values
ggplot(df, aes(x = Level, y = Salary)) +
  geom_col()  # or geom_bar(stat = "identity")

# Stacked bar
ggplot(df, aes(x = Level, y = Salary, fill = Department)) +
  geom_col()

# Side-by-side (dodged)
ggplot(df, aes(x = Level, y = Salary, fill = Department)) +
  geom_col(position = "dodge")
```

**3. Line Chart**:
```R
# Time series
ggplot(time_data, aes(x = Date, y = Sales)) +
  geom_line()

# Multiple lines
ggplot(time_data, aes(x = Date, y = Sales, color = Region)) +
  geom_line() +
  geom_point()  # Add points
```

**4. Histogram**:
```R
ggplot(df, aes(x = Salary)) +
  geom_histogram(bins = 10)

# With density curve
ggplot(df, aes(x = Salary)) +
  geom_histogram(aes(y = ..density..), bins = 10) +
  geom_density(color = "red")
```

**5. Box Plot**:
```R
ggplot(df, aes(x = Level, y = Salary)) +
  geom_boxplot()

# Add individual points
ggplot(df, aes(x = Level, y = Salary)) +
  geom_boxplot() +
  geom_jitter(width = 0.2, alpha = 0.5)
```

**6. Violin Plot**:
```R
ggplot(df, aes(x = Level, y = Salary)) +
  geom_violin()

# Combined with box plot
ggplot(df, aes(x = Level, y = Salary)) +
  geom_violin() +
  geom_boxplot(width = 0.1)
```

**Customization**:

**Titles and Labels**:
```R
ggplot(df, aes(x = Age, y = Salary)) +
  geom_point() +
  labs(
    title = "Salary vs Age Analysis",
    subtitle = "Employee compensation data",
    x = "Age (years)",
    y = "Annual Salary ($)",
    caption = "Source: HR Database 2024"
  )
```

**Themes**:
```R
# Built-in themes
+ theme_minimal()
+ theme_classic()
+ theme_bw()
+ theme_dark()

# Custom theme
ggplot(df, aes(x = Age, y = Salary)) +
  geom_point() +
  theme(
    plot.title = element_text(size = 16, face = "bold"),
    axis.title = element_text(size = 12),
    panel.background = element_rect(fill = "lightblue"),
    panel.grid = element_line(color = "white")
  )
```

**Colors**:
```R
# Manual colors
+ scale_color_manual(values = c("red", "blue", "green"))

# Brewer palettes
+ scale_color_brewer(palette = "Set1")

# Gradient
+ scale_color_gradient(low = "white", high = "darkblue")
```

**Faceting** (Small Multiples):
```R
# Facet by one variable
ggplot(df, aes(x = Age, y = Salary)) +
  geom_point() +
  facet_wrap(~ Level)

# Facet by two variables
ggplot(df, aes(x = Age, y = Salary)) +
  geom_point() +
  facet_grid(Level ~ Department)
```

**Complete Example**:
```R
ggplot(df, aes(x = Age, y = Salary, color = Level, size = Bonus)) +
  geom_point(alpha = 0.7) +
  geom_smooth(method = "lm", se = FALSE) +  # Add trend line
  scale_color_brewer(palette = "Set1") +
  labs(
    title = "Compensation Analysis by Level",
    x = "Employee Age",
    y = "Annual Salary ($)",
    color = "Job Level",
    size = "Bonus ($)"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 16),
    legend.position = "bottom"
  )

# Save plot
ggsave("salary_analysis.png", width = 10, height = 6, dpi = 300)
```

### 4. Statistical Analysis in R

**Descriptive Statistics**:
```R
# Central tendency
mean(df$Salary)
median(df$Salary)

# Dispersion
sd(df$Salary)       # Standard deviation
var(df$Salary)      # Variance
IQR(df$Salary)      # Interquartile range
range(df$Salary)    # Min and max

# Quantiles
quantile(df$Salary)
quantile(df$Salary, probs = c(0.25, 0.5, 0.75, 0.9, 0.95))

# Summary
summary(df$Salary)
```

**Correlation**:
```R
# Correlation between two variables
cor(df$Age, df$Salary)

# Correlation matrix
numeric_cols <- df %>% select(Age, Salary, Bonus)
cor(numeric_cols)

# Visualization
pairs(numeric_cols)  # Scatter plot matrix

# Or with ggplot
library(GGally)
ggpairs(numeric_cols)
```

**Linear Regression**:
```R
# Simple linear regression
model <- lm(Salary ~ Age, data = df)

# View results
summary(model)

# Coefficients
coef(model)

# Predictions
new_data <- data.frame(Age = c(26, 32, 38))
predict(model, newdata = new_data)

# Multiple regression
model2 <- lm(Salary ~ Age + Bonus + Level, data = df)
summary(model2)

# Diagnostic plots
par(mfrow = c(2, 2))
plot(model)

# Residuals
residuals(model)
```

**Hypothesis Testing**:

**t-test**:
```R
# One-sample t-test
t.test(df$Salary, mu = 60000)  # Test if mean = 60000

# Two-sample t-test
junior <- filter(df, Level == "Junior")$Salary
senior <- filter(df, Level == "Senior")$Salary
t.test(junior, senior)

# Paired t-test
t.test(before, after, paired = TRUE)
```

**ANOVA**:
```R
# One-way ANOVA
anova_result <- aov(Salary ~ Level, data = df)
summary(anova_result)

# Post-hoc test (pairwise comparisons)
TukeyHSD(anova_result)

# Two-way ANOVA
anova2 <- aov(Salary ~ Level + Department, data = df)
summary(anova2)
```

**Chi-Square Test**:
```R
# Create contingency table
table_data <- table(df$Level, df$Department)

# Chi-square test
chisq.test(table_data)
```

### 5. Machine Learning in R

**Data Preparation**:
```R
# Split data (training/testing)
set.seed(123)  # For reproducibility
sample_size <- floor(0.8 * nrow(df))
train_indices <- sample(seq_len(nrow(df)), size = sample_size)

train_data <- df[train_indices, ]
test_data <- df[-train_indices, ]

# Normalize/Scale
train_data$Salary_scaled <- scale(train_data$Salary)
```

**Classification - Decision Tree**:
```R
library(rpart)
library(rpart.plot)

# Build model
tree_model <- rpart(Level ~ Age + Salary + Bonus, 
                    data = train_data,
                    method = "class")

# Visualize tree
rpart.plot(tree_model)

# Predictions
predictions <- predict(tree_model, test_data, type = "class")

# Confusion matrix
table(Predicted = predictions, Actual = test_data$Level)

# Accuracy
accuracy <- mean(predictions == test_data$Level)
```

**Random Forest**:
```R
library(randomForest)

# Build model
rf_model <- randomForest(Level ~ Age + Salary + Bonus, 
                         data = train_data,
                         ntree = 500)

# Variable importance
importance(rf_model)
varImpPlot(rf_model)

# Predictions
rf_predictions <- predict(rf_model, test_data)

# Accuracy
mean(rf_predictions == test_data$Level)
```

**Clustering - K-Means**:
```R
# Prepare data (numeric columns only)
cluster_data <- df %>% select(Age, Salary, Bonus)

# Scale data
cluster_data_scaled <- scale(cluster_data)

# Determine optimal k (elbow method)
wss <- sapply(1:10, function(k) {
  kmeans(cluster_data_scaled, k, nstart = 10)$tot.withinss
})

plot(1:10, wss, type = "b", 
     xlab = "Number of Clusters",
     ylab = "Within Sum of Squares")

# K-means with k=3
set.seed(123)
kmeans_result <- kmeans(cluster_data_scaled, centers = 3, nstart = 25)

# Add cluster labels to data
df$Cluster <- kmeans_result$cluster

# Visualize
ggplot(df, aes(x = Age, y = Salary, color = factor(Cluster))) +
  geom_point(size = 3) +
  labs(color = "Cluster")

# Cluster centers
kmeans_result$centers
```

---

## EXAM PREPARATION GUIDE

### Key Topics Summary

**Unit I - Introduction**:
- ✅ 5 V's of Big Data
- ✅ Hadoop architecture (HDFS, MapReduce, YARN)
- ✅ Spark vs MapReduce
- ✅ AWS setup basics

**Unit II - Data Manipulation**:
- ✅ Heat maps, Tree maps
- ✅ Azure ML workflow
- ✅ Statistical formulas
- ✅ Logical and financial functions

**Unit III - Data Strategy**:
- ✅ Product analytics (ABC, RFM)
- ✅ Market basket analysis
- ✅ Competitive analysis frameworks
- ✅ Seasonality analysis

**Unit IV - Tableau**:
- ✅ Connecting to data
- ✅ Building visualizations
- ✅ Calculated fields
- ✅ Parameters and filters

**Unit V - Advanced**:
- ✅ Case studies (Supply Chain, Marketing, Finance)
- ✅ Dashboard actions
- ✅ R programming
- ✅ Statistical analysis in R

### Practice Problems

**Problem 1: Hadoop Calculation**
```
Given:
- File size: 1GB
- Block size: 128MB
- Replication factor: 3

Calculate:
a) Number of blocks?
b) Total storage used?

Solution:
a) 1024MB / 128MB = 8 blocks
b) 8 blocks × 3 replicas = 24 blocks
   24 × 128MB = 3072MB = 3GB
```

**Problem 2: RFM Scoring**
```
Customer data:
- Last purchase: 15 days ago
- Total purchases: 8
- Total spent: $2,500

Scoring (1-5 scale):
Recency: 5 (very recent)
Frequency: 4 (high)
Monetary: 5 (high value)

RFM Score: 545 → "Loyal Customer"
```

**Problem 3: Tableau Calculation**
```
Create calculated field for profit margin:

[Profit Margin] = ([Sales] - [Cost]) / [Sales]

Format as percentage
Color by: Green (>20%), Yellow (10-20%), Red (<10%)
```

### Final Checklist

```
□ Understand Big Data 5 V's with examples
□ Know Hadoop components and their roles
□ Can explain MapReduce with word count example
□ Understand HDFS read/write process
□ Know Spark advantages over MapReduce
□ Can create basic Hive queries
□ Understand Pig Latin syntax
□ Know data visualization best practices
□ Can build Tableau dashboards
□ Understand calculated fields and LOD expressions
□ Know R data structures
□ Can perform statistical analysis in R
□ Understand machine learning basics
□ Can interpret case study requirements
□ Know performance optimization techniques
```

---

**All the Best for Your Exam! 📊🎓**

You now have COMPLETE notes covering:
- All 5 units in full detail
- Complete case studies
- Advanced Tableau features
- R programming guide
- Statistical analysis
- Machine learning basics
- Practice problems
- Exam tips
