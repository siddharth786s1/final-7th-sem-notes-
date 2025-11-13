# Big Data Analytics - COMPLETE Detailed Notes (Continuation)

---

## UNIT II: DATA MANIPULATION USING FUNCTION (Continued)

### 5. Azure Machine Learning (Continued from previous)

**What is Azure Machine Learning?**

**Definition**: Azure Machine Learning (Azure ML) is a cloud-based service for building, training, deploying, and managing machine learning models at scale.

**Key Features**:
```
1. Automated ML (AutoML):
   - Automatically tries different algorithms
   - Selects best model
   - Hyperparameter tuning

2. Designer (Drag-and-Drop):
   - Visual interface
   - No code required
   - Pre-built modules

3. Notebooks:
   - Jupyter integration
   - Python/R support
   - GPU support

4. MLOps:
   - Version control for models
   - CI/CD for ML
   - Model monitoring
   - Deployment management
```

**Azure ML Workspace Components**:

**1. Compute**:
```
Compute Instances:
- Development VMs
- Jupyter notebooks
- RStudio

Compute Clusters:
- Scalable clusters
- Auto-scaling
- Training large models

Inference Clusters (AKS):
- Deploy models
- Production workloads
- High availability

Attached Compute:
- Use existing resources
- On-premises integration
```

**2. Datastores**:
```
Connect to data sources:
- Azure Blob Storage
- Azure Data Lake
- Azure SQL Database
- Azure Databricks
- Local files

Benefits:
- Centralized data access
- Credential management
- Version control
```

**3. Datasets**:
```
Types:
- Tabular: Structured data (CSV, Parquet)
- File: Images, videos, unstructured

Features:
- Versioning
- Monitoring for drift
- Profiling
- Lineage tracking
```

**4. Experiments**:
```
Track ML runs:
- Parameters
- Metrics
- Outputs
- Models

Compare runs:
- Side-by-side comparison
- Visualization
- Best model selection
```

**5. Models**:
```
Model Registry:
- Version control
- Tagging
- Deployment tracking

Model Management:
- Store trained models
- Package dependencies
- Deployment metadata
```

**6. Endpoints**:
```
Real-time:
- REST API
- Low latency
- Single predictions

Batch:
- Large datasets
- Scheduled runs
- Cost-effective
```

**Azure ML Workflow**:

**Step 1: Prepare Data**:
```python
from azureml.core import Workspace, Dataset

# Connect to workspace
ws = Workspace.from_config()

# Register dataset
dataset = Dataset.Tabular.from_delimited_files(
    path='https://data.csv'
)
dataset = dataset.register(
    workspace=ws,
    name='sales_data',
    description='Sales dataset'
)
```

**Step 2: Train Model (AutoML)**:
```python
from azureml.train.automl import AutoMLConfig
from azureml.core.experiment import Experiment

# AutoML configuration
automl_config = AutoMLConfig(
    task='classification',
    primary_metric='accuracy',
    training_data=dataset,
    label_column_name='target',
    n_cross_validations=5,
    enable_early_stopping=True,
    experiment_timeout_hours=1
)

# Submit experiment
experiment = Experiment(ws, 'automl_experiment')
run = experiment.submit(automl_config)
run.wait_for_completion(show_output=True)

# Get best model
best_run, best_model = run.get_output()
```

**Step 3: Train Model (Custom)**:
```python
from azureml.core import ScriptRunConfig, Environment
from azureml.core.compute import ComputeTarget

# Get compute target
compute_target = ComputeTarget(ws, 'my-cluster')

# Create environment
env = Environment.from_conda_specification(
    name='ml-env',
    file_path='environment.yml'
)

# Configure training script
config = ScriptRunConfig(
    source_directory='./src',
    script='train.py',
    arguments=['--learning-rate', 0.001],
    compute_target=compute_target,
    environment=env
)

# Submit run
run = experiment.submit(config)
run.wait_for_completion(show_output=True)
```

**Step 4: Register Model**:
```python
# Register model
model = run.register_model(
    model_name='sales_predictor',
    model_path='outputs/model.pkl',
    tags={'accuracy': 0.95}
)
```

**Step 5: Deploy Model**:
```python
from azureml.core.webservice import AciWebservice
from azureml.core.model import InferenceConfig

# Inference configuration
inference_config = InferenceConfig(
    entry_script='score.py',
    environment=env
)

# Deployment configuration
deployment_config = AciWebservice.deploy_configuration(
    cpu_cores=1,
    memory_gb=1
)

# Deploy
service = Model.deploy(
    workspace=ws,
    name='sales-predictor-service',
    models=[model],
    inference_config=inference_config,
    deployment_config=deployment_config
)

service.wait_for_deployment(show_output=True)
print(service.scoring_uri)
```

**Step 6: Consume Model**:
```python
import requests
import json

# Prepare data
data = {
    'data': [
        [10, 20, 30],
        [15, 25, 35]
    ]
}

# Make prediction
headers = {'Content-Type': 'application/json'}
response = requests.post(
    service.scoring_uri,
    data=json.dumps(data),
    headers=headers
)

predictions = response.json()
print(predictions)
```

**Azure ML Designer (Visual)**:

**Modules Available**:
```
Data Transformation:
- Select Columns
- Clean Missing Data
- Normalize Data
- Split Data

Machine Learning:
- Train Model
- Score Model
- Evaluate Model
- Tune Model Hyperparameters

Algorithms:
- Classification (Logistic Regression, Decision Tree, etc.)
- Regression (Linear Regression, Neural Network)
- Clustering (K-Means)

Statistical Functions:
- Summarize Data
- Compute Statistics
- Correlation

Model Scoring:
- Score Model
- Cross Validate Model
```

**Example Pipeline**:
```
Dataset → Select Columns → Clean Missing Data
    ↓
Split Data (70-30)
    ↓              ↓
Train Data    Test Data
    ↓              ↓
Train Model ----→ Score Model
                    ↓
              Evaluate Model
```

**Azure ML Benefits**:

**1. Scalability**:
- Auto-scaling compute
- Distributed training
- Handle large datasets

**2. Collaboration**:
- Team workspaces
- Shared experiments
- Version control

**3. MLOps**:
- Model versioning
- Automated deployment
- Monitoring and retraining

**4. Cost Management**:
- Pay-per-use
- Auto-shutdown
- Reserved instances

**5. Security**:
- Virtual networks
- Private endpoints
- Role-based access control (RBAC)

**Azure ML vs Traditional ML**:

| Aspect | Traditional | Azure ML |
|--------|------------|----------|
| Infrastructure | Manage yourself | Fully managed |
| Scaling | Manual | Auto-scaling |
| Collaboration | Difficult | Built-in |
| Deployment | Complex | One-click |
| Monitoring | Setup required | Integrated |
| Cost | Fixed (own servers) | Pay-as-you-go |

**Azure ML Pricing**:
```
Free Tier:
- Limited compute hours
- Good for learning

Standard Tier:
- Compute: Per hour (VM-based)
- Storage: Per GB
- Inference: Per request

Enterprise (Deprecated):
- Advanced features
- Replaced by Standard features
```

**Best Practices**:

**1. Data Management**:
- Use datastores for centralization
- Version datasets
- Monitor data drift
- Profile data regularly

**2. Experiment Tracking**:
- Log all parameters
- Track metrics
- Tag experiments
- Document findings

**3. Model Management**:
- Register all models
- Version models
- Tag with metadata
- Test before deployment

**4. Deployment**:
- Start with dev/test endpoint
- Monitor performance
- A/B testing
- Gradual rollout

**5. Cost Optimization**:
- Auto-shutdown compute instances
- Use low-priority VMs for training
- Batch inference when possible
- Delete unused resources

---

## UNIT III: DATA STRATEGY & CONSUMER BEHAVIOR ANALYTICS

### 1. Data Strategy Overview

**What is Data Strategy?**

**Definition**: A comprehensive plan for leveraging data as a strategic asset to achieve business objectives.

**Components of Data Strategy**:

**1. Data Governance**:
```
Policies and procedures for:
- Data quality standards
- Data ownership
- Access control
- Compliance (GDPR, HIPAA)
- Metadata management

Example Framework:
- Data stewards: Responsible for data quality
- Data catalog: Inventory of data assets
- Data lineage: Track data flow
- Data policies: Rules and standards
```

**2. Data Architecture**:
```
Technical infrastructure:

Data Sources → Data Ingestion → Data Storage
                                      ↓
                              Data Processing
                                      ↓
                              Data Analytics
                                      ↓
                            Data Visualization
                                      ↓
                          Business Insights

Layers:
- Raw data layer (Data Lake)
- Processed data layer (Data Warehouse)
- Analytics layer (OLAP cubes, aggregations)
- Presentation layer (Dashboards, reports)
```

**3. Data Quality**:
```
Dimensions:
- Accuracy: Correct data
- Completeness: No missing values
- Consistency: Same across systems
- Timeliness: Up-to-date
- Validity: Conforms to rules
- Uniqueness: No duplicates

Data Quality Framework:
1. Define quality metrics
2. Measure current state
3. Identify issues
4. Implement improvements
5. Monitor continuously
```

**4. Analytics Capabilities**:
```
Maturity Levels:

Level 1 - Descriptive:
- What happened?
- Reports, dashboards
- Historical data

Level 2 - Diagnostic:
- Why did it happen?
- Drill-down analysis
- Root cause analysis

Level 3 - Predictive:
- What will happen?
- Forecasting
- Machine learning

Level 4 - Prescriptive:
- What should we do?
- Optimization
- Recommendations

Level 5 - Cognitive:
- Autonomous decisions
- AI-driven actions
```

**Data Strategy Development Process**:

**Phase 1: Assessment**:
```
Current State Analysis:
- Data inventory
- Technology assessment
- Skills gap analysis
- Process review

SWOT Analysis:
- Strengths: What data assets do we have?
- Weaknesses: What's missing or poor quality?
- Opportunities: How can data drive value?
- Threats: What risks exist?
```

**Phase 2: Vision & Objectives**:
```
Define:
- Business goals
- Data vision
- Success metrics
- Priorities

Example Objectives:
- Improve customer retention by 20%
- Reduce operational costs by 15%
- Increase data-driven decisions by 50%
- Achieve 99% data quality
```

**Phase 3: Roadmap**:
```
Prioritize initiatives:

Quick Wins (3-6 months):
- Dashboard for key metrics
- Data quality improvements
- Self-service analytics pilot

Medium-term (6-12 months):
- Data warehouse implementation
- Advanced analytics capabilities
- Data governance framework

Long-term (12-24 months):
- AI/ML implementation
- Real-time analytics
- Data monetization
```

**Phase 4: Implementation**:
```
Execute roadmap:
- Assemble team
- Allocate budget
- Build infrastructure
- Train users
- Deploy solutions

Governance:
- Steering committee
- Regular reviews
- KPI tracking
- Adjust as needed
```

**Phase 5: Monitor & Evolve**:
```
Continuous improvement:
- Track metrics
- Gather feedback
- Identify gaps
- Update strategy
- Scale successes
```

### 2. Product & Category Analytics

**Understanding Product Performance**:

**Key Metrics**:
```
Sales Metrics:
- Revenue
- Units sold
- Average selling price (ASP)
- Sales growth rate

Profitability:
- Gross margin
- Profit margin
- Contribution margin

Market Metrics:
- Market share
- Competitive position
- Price elasticity
```

**Product Analytics Techniques**:

**1. ABC Analysis** (Pareto Principle):
```
Classify products by value:

A Items (80% revenue, 20% products):
- High value
- Tight control
- Frequent review

B Items (15% revenue, 30% products):
- Moderate value
- Standard control

C Items (5% revenue, 50% products):
- Low value
- Minimal control

Example:
Product | Revenue | % Total | Cumulative | Class
--------|---------|---------|------------|------
Prod A  | $50K    | 50%     | 50%        | A
Prod B  | $30K    | 30%     | 80%        | A
Prod C  | $10K    | 10%     | 90%        | B
Prod D  | $5K     | 5%      | 95%        | B
Prod E  | $5K     | 5%      | 100%       | C
```

**2. RFM Analysis** (Customer-Product):
```
Recency: How recently purchased?
Frequency: How often purchased?
Monetary: How much spent?

Scoring (1-5 scale):
Customer | R | F | M | RFM Score | Segment
---------|---|---|---|-----------|----------
Cust A   | 5 | 5 | 5 | 555       | Champions
Cust B   | 5 | 4 | 5 | 545       | Loyal
Cust C   | 1 | 1 | 1 | 111       | Lost

Actions:
- Champions (555): Upsell, reward
- Loyal (545): Engage, retain
- At Risk (344): Re-engage campaign
- Lost (111): Win-back offer
```

**3. Market Basket Analysis**:
```
Find products frequently bought together

Association Rules:
Support: P(A and B) / Total transactions
Confidence: P(B|A) = P(A and B) / P(A)
Lift: Confidence / P(B)

Example:
Rule: {Bread} → {Butter}
Support: 0.3 (30% transactions have both)
Confidence: 0.75 (75% who buy bread also buy butter)
Lift: 2.5 (2.5x more likely to buy butter with bread)

Applications:
- Product placement
- Bundling strategies
- Recommendations
- Promotions
```

**4. Product Lifecycle Analysis**:
```
Stages:

1. Introduction:
   - Low sales
   - High marketing costs
   - Negative profit

2. Growth:
   - Rapid sales increase
   - Increasing profit
   - Competition enters

3. Maturity:
   - Sales peak
   - Profit maximization
   - Market saturation

4. Decline:
   - Falling sales
   - Declining profit
   - Exit decisions

Metrics by Stage:
Stage        | Sales Growth | Profit | Strategy
-------------|--------------|--------|----------
Introduction | Slow         | -      | Build awareness
Growth       | Fast         | +      | Maximize share
Maturity     | Slow         | High   | Defend share
Decline      | Negative     | Low    | Harvest/Divest
```

**Category Analytics**:

**Category Performance Dashboard**:
```
Metrics to Track:
- Category sales trend
- Market share within category
- Price index vs competitors
- Promotion effectiveness
- Inventory turnover
- Out-of-stock rate
- Customer penetration
```

**Category Management**:
```
Four Roles:

1. Destination Category:
   - Primary reason to shop
   - High traffic driver
   - Example: Milk in grocery

2. Routine Category:
   - Regular purchases
   - Moderate traffic
   - Example: Cleaning supplies

3. Seasonal Category:
   - Time-based demand
   - Promotional opportunities
   - Example: Holiday decorations

4. Convenience Category:
   - Impulse purchases
   - High margin
   - Example: Checkout items

Strategy by Role:
- Destination: Low prices, wide selection
- Routine: Moderate prices, adequate stock
- Seasonal: Timely promotions, displays
- Convenience: Premium pricing, placement
```

### 3. Competitive Analysis

**Competitor Intelligence**:

**Data Sources**:
```
Public Sources:
- Company websites
- Financial reports (10-K, 10-Q)
- Press releases
- Industry reports
- Social media
- Customer reviews

Market Data:
- Nielsen data
- IRI data
- Market research firms
- Trade publications

Primary Research:
- Mystery shopping
- Customer surveys
- Industry conferences
```

**Competitive Metrics**:
```
Market Position:
- Market share (units and value)
- Share of voice (advertising)
- Brand awareness
- Customer satisfaction

Product Comparison:
- Features comparison matrix
- Price positioning
- Quality ratings
- Innovation rate

Financial Performance:
- Revenue growth
- Profit margins
- R&D spending
- Marketing spend
```

**Competitive Analysis Framework**:

**1. Porter's Five Forces**:
```
1. Competitive Rivalry:
   - Number of competitors
   - Industry growth rate
   - Product differentiation
   - Switching costs

2. Threat of New Entrants:
   - Barriers to entry
   - Capital requirements
   - Brand loyalty
   - Regulations

3. Threat of Substitutes:
   - Alternative products
   - Price-performance ratio
   - Switching costs

4. Bargaining Power of Suppliers:
   - Supplier concentration
   - Switching costs
   - Forward integration threat

5. Bargaining Power of Buyers:
   - Buyer concentration
   - Price sensitivity
   - Backward integration threat

Analysis Output:
Force               | Strength | Implication
--------------------|----------|-------------
Rivalry             | High     | Price pressure
New Entrants        | Medium   | Market share risk
Substitutes         | Low      | Limited threat
Supplier Power      | Medium   | Cost pressure
Buyer Power         | High     | Margin pressure
```

**2. SWOT Analysis** (Competitor):
```
Competitor: CompanyX

Strengths:
- Strong brand recognition
- Large distribution network
- Economies of scale
- Patent portfolio

Weaknesses:
- Aging product line
- Higher prices
- Limited online presence
- Bureaucratic structure

Opportunities (Our advantages):
- Digital marketing
- Product innovation
- Emerging markets
- Strategic partnerships

Threats (Their potential):
- Price war
- New product launches
- Acquisitions
- Market expansion
```

**3. Perceptual Mapping**:
```
Position brands on two dimensions:

           High Quality
                |
         Brand A|
                |
  Low     ------+------ High
  Price         |       Price
                |Brand B
                |
           Low Quality

Insights:
- Identify gaps (opportunities)
- Understand positioning
- Track movement over time
- Competitive clustering
```

**Competitive Pricing Analysis**:

**Price Index**:
```
Calculate relative pricing:

Price Index = (Your Price / Competitor Price) × 100

Example:
Your Product: $100
Competitor A: $120
Competitor B: $90

Index vs A: 83 (you're cheaper)
Index vs B: 111 (you're more expensive)

Market Position:
< 95: Price leader
95-105: At parity
> 105: Premium pricing
```

**Price Elasticity**:
```
% Change in Quantity / % Change in Price

Elastic (>1):
- Demand sensitive to price
- Price decrease → Large sales increase

Inelastic (<1):
- Demand not sensitive to price
- Price increase → Small sales decrease

Example:
Price change: +10%
Quantity change: -5%
Elasticity: -0.5 (inelastic)

Implication: Can increase price without losing much volume
```

### 4. Market Share Analysis

**Market Share Calculation**:

**Unit Market Share**:
```
Your Units Sold / Total Market Units Sold × 100

Example:
Your sales: 10,000 units
Total market: 100,000 units
Market share: 10%
```

**Revenue Market Share**:
```
Your Revenue / Total Market Revenue × 100

Example:
Your revenue: $1.5M
Total market: $10M
Market share: 15%

Note: Revenue share > Unit share = Premium pricing
```

**Market Share Analysis**:

**Share Metrics**:
```
1. Overall Market Share:
   Total sales / Total market

2. Served Market Share:
   Sales in target segment / Target segment size

3. Relative Market Share:
   Your share / Largest competitor's share
   
   >1: Market leader
   =1: Tied for lead
   <1: Follower

Example:
Your share: 20%
Competitor A: 25%
Relative share: 20/25 = 0.8 (follower)
```

**Share of Wallet**:
```
Your sales to customer / Customer's total category spending

Example:
Customer spends $1000/year on category
Spends $300 with you
Share of wallet: 30%

Goals:
- Increase share of wallet from existing customers
- Cross-selling opportunities
- Customer loyalty programs
```

**Market Share Trends**:
```
Track over time:

Quarter | Your Share | Comp A | Comp B | Comp C
--------|------------|--------|--------|-------
Q1 2023 | 18%        | 22%    | 15%    | 12%
Q2 2023 | 19%        | 21%    | 15%    | 11%
Q3 2023 | 20%        | 20%    | 14%    | 12%
Q4 2023 | 21%        | 19%    | 14%    | 13%

Insights:
- Gaining share (+3 points)
- Taking from leader (Comp A)
- Growing faster than market
```

**Geographic Market Share**:
```
Analyze by region:

Region    | Your Share | Opportunity
----------|------------|------------
Northeast | 25%        | Strong
Southeast | 15%        | Growth
Midwest   | 20%        | Maintain
West      | 10%        | Invest

Strategy:
- Protect strong markets
- Invest in weak markets
- Expand in growth markets
```

### 5. Market Potential Index & Seasonality

**Market Potential Index (MPI)**:

**Formula**:
```
MPI = Weighted average of factors

Factors:
1. Market Size (weight: 40%)
2. Market Growth Rate (weight: 30%)
3. Market Intensity (weight: 20%)
4. Market Consumption (weight: 10%)

Example:
Region A:
- Size score: 8/10 (40% weight) = 3.2
- Growth score: 7/10 (30% weight) = 2.1
- Intensity score: 6/10 (20% weight) = 1.2
- Consumption score: 9/10 (10% weight) = 0.9
MPI = 7.4/10

Ranking regions by MPI for resource allocation
```

**Market Attractiveness**:
```
Factors to Consider:

Market Factors:
- Size and growth
- Profitability
- Competitive intensity
- Barriers to entry

Economic Factors:
- GDP growth
- Disposable income
- Infrastructure
- Regulatory environment

Social Factors:
- Demographics
- Cultural fit
- Education levels
- Urbanization

Technological Factors:
- Digital adoption
- Innovation rate
- Tech infrastructure
```

**Seasonality Analysis**:

**Seasonal Index Calculation**:
```
Seasonal Index = (Period Average / Overall Average) × 100

Example: Monthly Sales
Month | Sales | Average | Index
------|-------|---------|------
Jan   | 80    | 100     | 80
Feb   | 90    | 100     | 90
Mar   | 100   | 100     | 100
Apr   | 110   | 100     | 110
May   | 120   | 100     | 120
Jun   | 130   | 100     | 130
Jul   | 130   | 100     | 130
Aug   | 120   | 100     | 120
Sep   | 110   | 100     | 110
Oct   | 100   | 100     | 100
Nov   | 90    | 100     | 90
Dec   | 120   | 100     | 120

Insights:
- Peak: Jun-Aug (summer)
- Low: Jan-Feb (winter)
- Holiday boost: Dec
```

**Seasonal Adjustment**:
```
Deseasonalize data to see true trend:

Adjusted Value = Actual Value / (Seasonal Index / 100)

Example:
January actual sales: 80
Seasonal index: 80
Adjusted: 80 / 0.8 = 100

Use adjusted values to:
- Identify true growth
- Compare periods
- Forecast accurately
```

**Forecasting with Seasonality**:
```
Steps:
1. Calculate trend (regression or moving average)
2. Calculate seasonal indices
3. Apply seasonal pattern to trend

Example:
Trend forecast for next January: 110
January seasonal index: 80
Final forecast: 110 × 0.8 = 88
```

**Sales-Trending Analysis**:

**Moving Average**:
```
3-Month Moving Average:

Month | Sales | 3-MA
------|-------|------
Jan   | 100   | -
Feb   | 110   | -
Mar   | 120   | 110
Apr   | 115   | 115
May   | 125   | 120
Jun   | 130   | 123.3

Smooths fluctuations
Shows underlying trend
```

**Exponential Smoothing**:
```
Forecast = α × Actual + (1-α) × Previous Forecast

Where α (smoothing constant) = 0 to 1

α = 0.3 (gives more weight to history)
α = 0.7 (more responsive to recent changes)

Example:
Previous forecast: 100
Actual: 110
α = 0.3
New forecast: 0.3×110 + 0.7×100 = 103
```

---

## UNIT IV: TABLEAU SOFTWARE & VISUALIZATION

### 1. Getting Started with Tableau

**What is Tableau?**

**Definition**: Tableau is a powerful business intelligence and data visualization tool used to simplify raw data into easily understandable visualizations.

**Tableau Products**:
```
1. Tableau Desktop:
   - Windows/Mac application
   - Create visualizations
   - Connect to data sources
   - Build dashboards

2. Tableau Server:
   - On-premises server
   - Share dashboards
   - Collaboration
   - Enterprise deployment

3. Tableau Online:
   - Cloud-based (SaaS)
   - Similar to Server
   - No infrastructure management

4. Tableau Public:
   - Free version
   - Publish to public web
   - Limited data sources

5. Tableau Prep:
   - Data preparation tool
   - Clean and shape data
   - Visual data pipeline

6. Tableau Mobile:
   - iOS/Android apps
   - View dashboards
   - Touch-optimized
```

**Tableau Interface**:
```
+------------------------------------------+
| Menu Bar: File, Data, Worksheet, etc.   |
+------------------------------------------+
| Toolbar: Undo, Redo, Save, etc.         |
+------------------------------------------+
| Data Pane | Canvas Area                  |
| - Tables  | - Shelves (Rows, Columns)    |
| - Fields  | - Marks Card                 |
|           | - Visualization              |
+------------------------------------------+
| Show Me Panel (chart types)              |
+------------------------------------------+
```

**Key Components**:

**1. Data Pane**:
```
Dimensions (Blue):
- Categorical data
- Qualitative
- Examples: Category, Region, Product Name

Measures (Green):
- Numerical data
- Quantitative
- Examples: Sales, Profit, Quantity

Created Fields:
- Calculated fields
- Groups
- Sets
- Parameters
```

**2. Shelves**:
```
Columns: Horizontal axis
Rows: Vertical axis
Filters: Filter data
Pages: Animate through dimension
Marks: Visual properties
```

**3. Marks Card**:
```
Properties:
- Color
- Size
- Text (labels)
- Detail
- Tooltip
- Shape (for scatter plots)
- Path (for maps/lines)
```

**4. Show Me Panel**:
```
Suggests appropriate chart types based on fields selected

Examples:
- Bar chart
- Line chart
- Scatter plot
- Map
- Tree map
- Heat map
```

### 2. Connecting to Data Sources

**Supported Data Sources**:

**Files**:
```
- Excel (.xlsx, .xls)
- Text (.csv, .txt)
- JSON
- PDF
- Spatial files (.shp, .kml)
- Statistical files (.sav, .sas7bdat)
```

**Databases**:
```
Relational:
- MySQL
- PostgreSQL
- SQL Server
- Oracle
- Amazon Redshift

Cloud:
- Google BigQuery
- Snowflake
- Azure SQL

NoSQL:
- MongoDB
- Google Analytics
```

**Cloud Applications**:
```
- Salesforce
- Google Sheets
- OneDrive
- Dropbox
```

**Connection Types**:

**Live Connection**:
```
Pros:
- Always up-to-date
- No data duplication
- Direct queries to database

Cons:
- Requires database connectivity
- Performance depends on database
- Network latency

Use when:
- Data changes frequently
- Large datasets
- Real-time requirements
```

**Extract**:
```
Pros:
- Fast performance
- Offline access
- Reduced database load

Cons:
- Not real-time (refresh needed)
- Data duplication
- Storage required

Use when:
- Slow database
- Mobile access needed
- Want to reduce database queries

Creating Extract:
Data → Extract Data → Configure options
```

**Data Source Page**:
```
Components:
1. Connections: Data sources
2. Canvas: Join/union tables
3. Data Grid: Preview data
4. Metadata: Field types, aliases

Operations:
- Join tables
- Union tables
- Pivot data
- Split fields
- Create calculated fields
```

### 3. Building Basic Visualizations

**Creating Your First Viz**:

**Example: Sales by Category**:
```
Steps:
1. Connect to data (e.g., Superstore sample)
2. Drag Category to Columns
3. Drag Sales to Rows
4. Tableau creates bar chart automatically

Result: Bar chart showing sales by category
```

**Chart Types and Usage**:

**1. Bar Chart** (Horizontal/Vertical):
```
When to use:
- Compare categories
- Show rankings
- Display magnitude

Build:
- Dimension → Rows/Columns
- Measure → Columns/Rows

Example: Sales by Region
Drag: Region → Rows, Sales → Columns
```

**2. Line Chart**:
```
When to use:
- Trends over time
- Continuous data
- Multiple series comparison

Build:
- Date → Columns
- Measure → Rows

Example: Sales Trend
Drag: Order Date → Columns, Sales → Rows
Change date level: Year, Quarter, Month
```

**3. Scatter Plot**:
```
When to use:
- Correlation between two measures
- Outlier detection
- Clustering analysis

Build:
- Measure 1 → Columns
- Measure 2 → Rows
- Dimension → Color/Detail

Example: Profit vs Sales
Columns: Sales
Rows: Profit
Color: Category
```

**4. Pie Chart**:
```
When to use:
- Part-to-whole (limit to 5-7 slices)
- Percentage distribution

Build:
- Dimension → Color
- Measure → Angle (automatic in pie)

Example: Sales by Category
Mark type: Pie
Drag: Category → Color, Sales → Angle

Note: Often better alternatives exist (bar chart)
```

**5. Map**:
```
When to use:
- Geographic data
- Regional analysis
- Location-based insights

Build:
- Geographic field (auto-detected)
- Tableau generates latitude/longitude

Example: Sales by State
Drag: State → Detail
Drag: Sales → Color (fills map)
```

**6. Tree Map**:
```
When to use:
- Hierarchical data
- Part-to-whole with hierarchy
- Space-efficient comparison

Build:
Mark type: Automatic (or select Tree Map)
- Dimension(s) → Color, Label
- Measure → Size

Example: Sales by Category and Sub-category
Drag: Category → Color
Drag: Sub-Category → Label
Drag: Sales → Size
```

**7. Heat Map**:
```
When to use:
- Two dimensions with measure
- Pattern identification
- Density visualization

Build:
- Dimension 1 → Columns
- Dimension 2 → Rows
- Measure → Color
- Optional: Measure → Label

Example: Sales by Month and Category
Columns: MONTH(Order Date)
Rows: Category
Color: SUM(Sales)
```

### 4. Tableau Workspace Deep Dive

**Understanding Dimensions and Measures**:

**Dimensions**:
```
Characteristics:
- Categorical fields
- Typically strings or dates
- Create row/column headers
- Used for grouping and filtering

Operations:
- Groups
- Hierarchies
- Sets
- Bins (for continuous dimensions)

Examples:
- Customer Name
- Product Category
- Region
- Order Date
```

**Measures**:
```
Characteristics:
- Numerical fields
- Aggregated by default
- Create axes

Default Aggregations:
- SUM
- AVG
- MIN
- MAX
- COUNT
- COUNTD (distinct count)
- MEDIAN
- STDEV

Change aggregation:
Right-click measure → Measure → Select aggregation

Examples:
- Sales
- Profit
- Quantity
- Discount
```

**Discrete vs Continuous**:
```
Discrete (Blue pills):
- Individual, separate values
- Creates headers
- Examples: Category, Year

Continuous (Green pills):
- Continuous range of values
- Creates axes
- Examples: Sales, Date (as timeline)

Convert:
Right-click field → Convert to Discrete/Continuous
```

### 5. Data Types and Tableau Workspace

**Tableau Data Types**:

**Icons in Data Pane**:
```
# - Number (whole number)
#.# - Number (decimal)
Abc - String (text)
📅 - Date
📅🕐 - Date & Time
🌐 - Geographic role
T/F - Boolean
```

**Changing Data Types**:
```
Method 1: Data Source page
Click icon → Select new type

Method 2: Data pane
Right-click field → Change Data Type

Common issues:
- Dates recognized as strings
- Numbers as strings (leading zeros, formats)
- Geographic fields not recognized
```

**Geographic Roles**:
```
Assign geographic role:
Right-click field → Geographic Role → Select type

Roles:
- Country/Region
- State/Province
- City
- ZIP Code/Postcode
- Latitude
- Longitude
- Airport
- Congressional District

Tableau automatically generates:
- Latitude (generated)
- Longitude (generated)
```

### 6. Dimensions and Measures

**Working with Dates**:

**Date Levels**:
```
Discrete (Blue):
- Year: 2023, 2024
- Quarter: Q1, Q2, Q3, Q4
- Month: Jan, Feb, Mar
- Week: Week 1, Week 2
- Weekday: Mon, Tue, Wed

Continuous (Green):
- Year: Timeline axis
- Quarter: Continuous quarters
- Month: Continuous months
- Day: Continuous days

Change level:
Right-click date → Select date level
+ icon: Drill down (add detail)
- icon: Roll up (remove detail)
```

**Custom Dates**:
```
Calculated Field examples:

Fiscal Year (starts April):
IF MONTH([Order Date]) >= 4 THEN
    YEAR([Order Date])
ELSE
    YEAR([Order Date]) - 1
END

Week Start (Monday):
DATETRUNC('week', [Order Date])

Days Since Order:
DATEDIFF('day', [Order Date], TODAY())
```

**Hierarchies**:

**Creating Hierarchy**:
```
Method 1:
Drag dimension onto another → Create Hierarchy

Method 2:
Right-click dimension → Hierarchy → Create Hierarchy

Example: Location Hierarchy
- Country
  ↳ State
    ↳ City
      ↳ Postal Code

Usage:
Drag hierarchy to view
+ to expand level
- to collapse level
```

**Date Hierarchy** (Built-in):
```
Year → Quarter → Month → Day

Auto-created for date fields
```

### 7. Data Types & Properties

**Calculated Fields**:

**Basic Calculations**:
```
Syntax:
[Field Name] operator [Field Name]

Operators:
+ - * / (arithmetic)
= < > <= >= != (comparison)
AND OR NOT (logical)

Example: Profit Ratio
[Profit] / [Sales]

Example: Discount Amount
[Sales] * [Discount]

Example: Above Average
[Sales] > AVG([Sales])
```

**String Functions**:
```
UPPER([Customer Name])
- Convert to uppercase

LOWER([Product Name])
- Convert to lowercase

LEFT([Customer Name], 5)
- First 5 characters

RIGHT([Order ID], 4)
- Last 4 characters

CONTAINS([Product Name], "Chair")
- Check if contains text

SPLIT([Customer Name], " ", 1)
- Split and take first part

LEN([Customer Name])
- Length of string
```

**Number Functions**:
```
ROUND([Sales], 2)
- Round to 2 decimals

CEILING([Sales])
- Round up

FLOOR([Sales])
- Round down

ABS([Profit])
- Absolute value

POWER([Sales], 2)
- Square

SQRT([Sales])
- Square root
```

**Date Functions**:
```
DATEADD('month', 3, [Order Date])
- Add 3 months

DATEDIFF('day', [Order Date], [Ship Date])
- Days between dates

DATEPART('month', [Order Date])
- Extract month number

DATENAME('month', [Order Date])
- Extract month name

DATETRUNC('month', [Order Date])
- First day of month

TODAY()
- Current date

NOW()
- Current date and time
```

**Aggregation Functions**:
```
SUM([Sales])
- Total sales

AVG([Sales])
- Average sales

MIN([Sales])
- Minimum

MAX([Sales])
- Maximum

COUNT([Order ID])
- Count of records

COUNTD([Customer ID])
- Distinct count

MEDIAN([Sales])
- Median value

STDEV([Sales])
- Standard deviation
```

**LOD (Level of Detail) Expressions**:

**FIXED**:
```
{FIXED [Category] : SUM([Sales])}
- Sales by category regardless of view filters

Example: Sales per Customer
{FIXED [Customer ID] : SUM([Sales])}
```

**INCLUDE**:
```
{INCLUDE [Sub-Category] : AVG([Sales])}
- Include additional dimension in aggregation
```

**EXCLUDE**:
```
{EXCLUDE [Region] : SUM([Sales])}
- Exclude dimension from aggregation
```

### 8. Saving and Sharing Your Work

**Saving Options**:

**Save Workbook**:
```
File → Save As

Formats:
- .twb (Tableau Workbook): 
  - No data included
  - Points to data source
  - Small file size

- .twbx (Tableau Packaged Workbook):
  - Includes data extract
  - Self-contained
  - Larger file size
  - Portable
```

**Publishing to Tableau Server/Online**:
```
Steps:
1. File → Publish Workbook
2. Sign in to Server/Online
3. Select project
4. Set permissions
5. Configure refresh schedule (for extracts)

Options:
- Sheets to publish
- Show/hide sheets
- External files to include
```

**Export Options**:
```
1. Image:
   Worksheet → Export → Image (PNG)
   - Static snapshot
   - For presentations

2. PDF:
   Worksheet → Export → PDF
   - Paginated dashboards
   - Multiple sheets

3. PowerPoint:
   - Export as images to slides
   - Tableau plugin available

4. Data:
   Worksheet → Export → Data
   - Export to CSV
   - Underlying or aggregated data

5. Crosstab:
   Worksheet → Export → Cross Tab to Excel
   - Table format in Excel
```

---

## UNIT V: TABLEAU - BUILDING VIEWS & REPORTS

### 1. Building Basic Views

**Creating Calculated Fields**:

**Row-Level Calculations**:
```
Calculate for each row before aggregation

Example: Full Customer Name
[First Name] + " " + [Last Name]

Example: Discount Amount
[Sales] * [Discount]

Example: Days to Ship
DATEDIFF('day', [Order Date], [Ship Date])
```

**Aggregate Calculations**:
```
Perform aggregation

Example: Average Sales
AVG([Sales])

Example: Total Profit Ratio
SUM([Profit]) / SUM([Sales])

Example: Running Total
RUNNING_SUM(SUM([Sales]))
```

**Table Calculations**:
```
Compute on aggregated data in the view

Running Total:
Right-click measure → Quick Table Calculation → Running Total

Percent of Total:
Right-click measure → Quick Table Calculation → Percent of Total

Difference:
Right-click measure → Quick Table Calculation → Difference

Percent Difference:
Right-click measure → Quick Table Calculation → Percent Difference

Moving Average:
Right-click measure → Quick Table Calculation → Moving Average
```

**Parameters**:

**Creating Parameters**:
```
Right-click Data pane → Create Parameter

Properties:
- Name
- Data type (String, Integer, Float, Date, Boolean)
- Current value
- Allowable values:
  - All
  - List
  - Range

Example: Top N Parameter
Name: Top N
Data type: Integer
Current value: 10
Range: 1 to 100
```

**Using Parameters**:
```
Example: Dynamic Top N

Create calculated field:
[Sales] > WINDOW_MAX(ATTR([Sales]), -ATTR([Top N]), 0)

Add to Filters

Show parameter control:
Right-click parameter → Show Parameter Control

User can adjust N in the view
```

**Example: Measure Selector**:
```
Parameter: Select Measure
Values: Sales, Profit, Quantity

Calculated Field:
CASE [Select Measure]
WHEN "Sales" THEN [Sales]
WHEN "Profit" THEN [Profit]
WHEN "Quantity" THEN [Quantity]
END

Drag to view, show parameter control
```

### 2. Case Studies and Advanced Visualizations

**Case Study 1: Sales Dashboard**

**Requirements**:
```
1. Executive summary (KPIs)
2. Sales trend over time
3. Sales by region (map)
4. Top products
5. Category performance
```

**Building the Dashboard**:

**Sheet 1 - KPIs**:
```
Create 4 separate sheets:

Total Sales:
- SUM([Sales])
- Format as currency
- Large font, no axes

Total Profit:
- SUM([Profit])
- Color by positive/negative

Order Count:
- COUNTD([Order ID])

Average Order Value:
- SUM([Sales]) / COUNTD([Order ID])
```

**Sheet 2 - Sales Trend**:
```
- Columns: MONTH([Order Date])
- Rows: SUM([Sales])
- Add trend line
- Add reference line for average
- Forecast: Analytics pane → Forecast
```

**Sheet 3 - Regional Map**:
```
- Geographic field: State
- Color: SUM([Sales])
- Size: SUM([Profit])
- Tooltips: Sales, Profit, Order Count
- Labels for top states
```

**Sheet 4 - Top 10 Products**:
```
- Rows: Product Name
- Columns: Sales
- Sort descending
- Filter: Top 10 by Sales
- Color by Profit
```

**Sheet 5 - Category Tree Map**:
```
- Mark type: Tree Map
- Color: Category
- Size: Sales
- Label: Category, Sales
```

**Assembling Dashboard**:
```
Dashboard → New Dashboard

Layout:
+--------------------------------+
| KPI1  | KPI2  | KPI3  | KPI4  |
+--------------------------------+
| Sales Trend (full width)       |
+--------------------------------+
| Regional  | Top      | Category|
| Map       | Products | Tree Map|
+--------------------------------+

Add:
- Title
- Filters (global filters)
- Actions (click region filters other sheets)
- Instructions/annotations
```

**Case Study 2: Customer Analytics**

**Customer Segmentation (RFM)**:

**Calculated Fields**:
```
Recency:
DATEDIFF('day', {FIXED [Customer ID]: MAX([Order Date])}, TODAY())

Frequency:
{FIXED [Customer ID]: COUNTD([Order ID])}

Monetary:
{FIXED [Customer ID]: SUM([Sales])}

RFM Score:
IF [Recency] <= 30 AND [Frequency] >= 5 AND [Monetary] >= 1000
THEN "Champion"
ELSEIF [Recency] <= 90 AND [Frequency] >= 3 AND [Monetary] >= 500
THEN "Loyal"
ELSEIF [Recency] > 180 AND [Frequency] < 2
THEN "At Risk"
ELSE "Other"
END
```

**Visualization**:
```
Scatter Plot:
- Columns: Frequency
- Rows: Monetary
- Color: RFM Score
- Size: Recency (inverse - smaller = more recent)
- Tooltips: Customer details

Insights:
- Champions: Top-right (high freq, high value)
- At Risk: Bottom-left (low freq, low value, old)
```

**Customer Lifetime Value**:
```
Calculated Field:
{FIXED [Customer ID]: SUM([Sales])} / 
DATEDIFF('month', {FIXED [Customer ID]: MIN([Order Date])}, TODAY())

This estimates monthly value
Multiply by expected customer lifetime
```

---

*Would you like me to continue with more advanced Tableau features, the complete case studies section, or move on to other units? There's still more content to cover!*
