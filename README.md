# fecom-e-com-dashboard-Business-Central-Data-Reporting-in-Power-BI
Fecom Inc. (E Com) Dashboard 

Prepared by : Aman Kr Verma (Microsoft Intern)
Project : Business Central to Power BI Reporting

Objective:-
The objective of this dashboard is to provide a comprehensive view of business performance by analyzing revenue, sales, cost, customer behavior, and operational trends.
The dashboard helps stakeholders to:
•	Monitor revenue and cost trends 
•	Analyze category-wise performance 
•	Understand geographic market distribution 
•	Validate revenue vs cost relationship 

 Dashboard Overview:-
This page presents a consolidated view of business performance using key indicators, trend analysis, and order distribution. It highlights both operational performance and customer engagement.

Initial Data Received:
The dataset for Fecom Inc. (E-Commerce) was provided by the project lead, containing multiple related tables such as Orders, Order Items, Payments, Products, Customers, and Reviews.
Data Understanding & Research:
•	Performed initial analysis to understand structure and relationships between tables 
•	Identified key entities like: 
o	Orders 
o	Customers (Subscriber_ID) 
o	Products 
o	Payments 
o	Reviews 
•	Analyzed how revenue, cost, and extra charges are derived from different tables 
              Data Modeling (Power BI Model View):
•	Established relationships between tables using primary and foreign keys 
•	Connected tables such as: 
o	Orders ↔ Order Items 
o	Orders ↔ Payments 
o	Orders ↔ Customers (Subscriber_ID) 
o	Products ↔ Order Items 
•	Ensured correct relationship direction and cardinality (One-to-Many) 

Purpose of Data Modeling:
•	To enable accurate KPI calculation 
•	To ensure proper filtering and cross-visual interaction 
•	To support business-level insights across multiple dimensions

Data Sources & Structure:-
 The dashboard is built using a relational dataset consisting of multiple tables.
🔹 Tables Used:
•	Orders → Order_ID, Order_Status 
•	Order Items → Product_ID, Freight_Value 
•	Order Payments → Payment_Value (Revenue) 
•	Customers → Customer_ID / Subscriber_ID, Location 
•	Products → Product_Category_Name 
•	Geolocation → Country mapping 

  Data Model Relationships:-
•	Orders ↔ Order Items (via Order_ID) 
•	Orders ↔ Payments (via Order_ID) 
•	Orders ↔ Customers (via Customer_ID / Subscriber_ID) 
•	Products ↔ Order Items (via Product_ID) 
•	 This creates a star-schema-like analytical model, enabling efficient aggregation and filtering.

Key KPIs
•	Total Customers: Total number of unique customers 
•	Total Order Amount: Overall sales generated 
•	Total Cost to Customer: Cost incurred per order 
•	Total Revenue: Total payments received 
•	Sales Growth Rate: Trend of business growth over time 
•	Total Reviews: Number of customer reviews indicating engagement and satisfaction 
•	Extra Charge: Additional amount paid by customer beyond base cost
•	Top Country Sales: Geolocation insights how Top country name


Page 1: Executive Summary $
 Overview:•	This page provides a high-level summary of business performance including customers, orders, sales value, cost, revenue, growth, and customer engagement. It enables stakeholders to quickly assess overall business performance and trends.
 <img width="940" height="518" alt="image" src="https://github.com/user-attachments/assets/b953a744-d35f-4a49-b5e5-3d1a217a059d" />

KPIs Implemented:
1. Total Customers
Logic:
Distinct count of customers using Customer_Trx_ID
DAX:
TotalCustomers = DISTINCTCOUNT('Customer List'[Customer_Trx_ID])
Challenge:
•	Customer name not available in dataset 
•	Duplicate entries across orders 
Solution:
•	Used Customer_Trx_ID as unique identifier 
•	Applied DISTINCTCOUNT to remove duplication 
Business Meaning:
Represents total number of unique customers.

2. Total Order Amount (Product Sales Value)
Logic:
Sum of product prices from Order Items
DAX:
TotalOrderAmount = SUM('Order Items'[Price])
Challenge:
•	Data spread across multiple tables 
•	Risk of confusion with revenue 
Solution:
•	Used Order Items table as source of product value 
Business Meaning:
Represents total value of products sold (excluding additional charges).

3. Total Cost to Customer
Logic:
Product Cost + Freight Cost
DAX:
TotalCostToCustomer = 
SUM('Order Items'[Price]) + SUM('Order Items'[Freight_Value])
Challenge:
•	Cost not available as a single column 
Solution:
•	Combined product price and freight cost 
Business Meaning:
Represents total cost incurred to fulfill orders including logistics.

4. Total Revenue
Logic:
Total payments received from customers
DAX:
TotalRevenue = SUM('Order Payments'[Payment_Value])
Challenge:
•	Confusion between order value and revenue 
•	Initial blank values due to relationship issues 
Solution:
•	Used Order Payments as single source of truth 
•	Fixed relationships with Orders table 
Business Meaning:
Represents actual revenue collected, including extra charges.

5. Sales Growth Rate
Logic:
Current period vs previous period comparison
DAX (concept):
SalesGrowthRate = 
DIVIDE(
    [Current Period Sales] - [Previous Period Sales],
    [Previous Period Sales]
)
Challenge:
•	Incorrect or 0% values 
•	Time intelligence issues 
Solution:
•	Created Calendar table 
•	Linked with Orders 
•	Applied time-based calculations 
Business Meaning:
Shows how sales are growing or declining over time.

 Visuals Used
•	KPI Cards (All key metrics) 
•	Line + Column Chart (Sales & Growth Trend) 
•	Donut Chart (Order Status Distribution) 


Page 2: Category Analysis
Overview:
This page focuses on analyzing business performance across different product categories. It helps identify which categories contribute most to revenue, cost, freight, and operational 
performance.
<img width="940" height="538" alt="image" src="https://github.com/user-attachments/assets/a49acf9c-365c-42ae-a2a9-9e853ce42e72" />

KPIs Implemented
1. Freight Cost
Logic:
Sum of freight (shipping/logistics) cost
DAX:
FreightCost = SUM('Order Items'[Freight_Value])
Challenge:
•	Freight cost distributed across item-level data 
•	Needed aggregation at category level 
Solution:
•	Used Order Items table 
•	Aggregated freight by Product_Category 
Business Meaning:
Represents total logistics cost incurred for delivering products.

2. Total Reviews
Logic:
Count of customer reviews
DAX:
TotalReviews = COUNT('Order Reviews'[Review_ID])
Challenge:
•	Reviews not directly linked to product category 
Solution:
•	Connected Reviews → Orders → Order Items 
•	Enabled category-level filtering 
Business Meaning:
Shows customer engagement and feedback per category.

3. Total Revenue
Logic:
Sum of payment values
DAX:
TotalRevenue = SUM('Order Payments'[Payment_Value])
Challenge:
•	Revenue comes from payments, not items 
•	Needed correct relationships for category filtering 
Solution:
•	Linked Payments → Orders → Order Items 
•	Ensured filter flow works correctly 
Business Meaning:
Represents total revenue generated across categories.



 Visuals Used
1. Category-wise Sales and Cost Analysis
Chart Type: Clustered Bar Chart
Fields Used:
•	Axis: Product_Category_Name 
•	Values: 
o	Total Order Amount (Product Sales) 
o	Total Cost to Customer 
o	Freight Cost 
Purpose:
Compare revenue vs cost across categories.

2. Freight by Product Category
Chart Type: Horizontal Bar Chart
Fields Used:
•	Axis: Product_Category_Name 
•	Values: Freight Cost 
Purpose:
Identify which categories have high logistics cost.

Challenges Faced:
1.	Category Mapping Issue 
•	Product category present only in Product table 
•	Needed correct relationship with Orders 
✔ Solution:
•	Connected Products → Order Items → Orders 
________________________________________
2.	Freight Allocation Complexity 
•	Freight at item level, not order level 
✔ Solution:
•	Aggregated freight values using SUM 
________________________________________
3.	Revenue vs Cost Confusion 
•	Revenue from payments 
•	Cost from items 
✔ Solution:
•	Clearly separated: 
o	Revenue → Payments 
o	Cost → Order Items



Page 3: Geographic Insights (Top Markets Performance)
Overview:
This page analyzes business performance across different countries. It helps identify top-performing markets, revenue contribution by geography, and regional distribution of sales.
<img width="940" height="521" alt="image" src="https://github.com/user-attachments/assets/d0c08c08-a5a3-490b-922c-8db91bed058a" />

KPIs Implemented
1. Top Country Sales
Logic:
Maximum sales value among all countries
DAX (concept):
Top Country Sales = 
CALCULATE(
    [Total Order Amount],
    TOPN(1, ALL(Geolocations[Geo_Country]), [Total Order Amount], DESC)
)
Challenge:
•	Need to aggregate sales at country level 
•	Country data stored in Geolocation table 
Solution:
•	Linked Geolocation → Orders → Order Items 
•	Aggregated sales using Total Order Amount 
Business Meaning:
Represents highest revenue generated by a single country.

2. Top Country % Contribution
Logic:
Top country sales divided by total sales
DAX (concept):
Top Country % = 
DIVIDE(
    [Top Country Sales],
    CALCULATE(
        [Total Order Amount],
        ALL(Geolocations[Geo_Country])
    )
)
Challenge:
•	Required correct total vs filtered context 
Solution:
•	Used DIVIDE with proper aggregation 
•	Ensured correct filter context 
Business Meaning:
Shows how much one country contributes to total business.

3. Top Country Name
Logic:
Country with highest sales
DAX (concept):
TopCountryName = 
TOPN(1, VALUES(Geolocations[Geo_Country]), [TotalOrderAmount], DESC)
Challenge:
•	Extracting top country dynamically 
Solution:
•	Used TOPN logic based on sales 
Business Meaning:
Identifies the best-performing country (e.g., Germany).

Visuals Used
1. Sales Distribution Map
Chart Type: Map Visual
Fields Used:
•	Location: Geo_Country / Latitude / Longitude 
•	Values: Total Order Amount 
Purpose:
Shows geographic spread of sales across countries.

2. Top 10 Countries by Sales
Chart Type: Table
Fields Used:
•	Geo_Country 
•	Total Order Amount 
Purpose:
Ranks countries based on revenue contribution.

Challenges Faced
1. Geolocation Mapping Issue
•	Country data stored separately in Geolocation table 
✔ Solution:
•	Created relationship between Geolocation and Orders 
2. Map Visualization Accuracy
•	Incorrect mapping due to missing geo fields 
✔ Solution:
•	Used proper country field (Geo_Country) 
•	Ensured correct data category (Country/Region) 
3. Top Country Calculation
•	Needed dynamic identification of top country 
✔ Solution:
•	Used MAXX / TOPN logic 
4. Percentage Calculation Issue
•	Incorrect % due to filter context 
✔ Solution:
•	Used DIVIDE with correct total context 



Page 4: Revenue Reconciliation
Overview: This page explains the relationship between Total Cost, Total Revenue, and Extra Charges. It provides a detailed breakdown at order level and category level to justify why revenue differs from cost.
<img width="940" height="531" alt="image" src="https://github.com/user-attachments/assets/bcf78c9b-fee4-4f96-8dec-f6ba1de76e50" />

 KPIs Implemented
1. Total Cost to Customer
•	Logic:
Product Price + Freight Cost
•	DAX:
•	TotalCostToCustomer = 
SUM('Order Items'[Price]) + SUM('Order Items'[Freight_Value])
•	Business Meaning:
Represents total cost incurred to fulfill orders including product and logistics.
  
            2. Total Revenue
•	Logic:
Sum of all customer payments
•	DAX:
•	TotalRevenue = SUM('Order Payments'[Payment_Value])
•	Business Meaning:
Represents total money received from customers.

3. Extra Charges
•	Logic:
Difference between Revenue and Cost
•	DAX:
•	ExtraCharge = [payment amount] - [TotalCostToCustomer]
•	Challenge:
•	Initially values were not matching 
•	Some rows showing blank 
•	Solution:
•	Fixed relationships between Orders, Payments, and Order Items 
•	Ensured correct aggregation 
•	Business Meaning:
Represents additional charges such as:
•	Shipping margins 
•	Platform/service fees 
•	Pricing adjustments 

 Visuals Used
1. Extra Charge by Product Category
•	Chart Type: Horizontal Bar Chart
•	Purpose:
Shows which product categories generate the most extra charges.

2. Order-Level Reconciliation Table
•	Chart Type: Table
•	Columns:
•	Order_ID 
•	Total Cost to Customer 
•	Extra Charge 
•	Total Revenue 
•	Purpose:
Provides detailed proof of revenue calculation at order level.

 Challenges Faced
•	1. Revenue vs Cost Mismatch
•	Values were not aligning initially 
•	✔ Solution:
•	Created clear formula:
Revenue = Cost + Extra Charges 
•	________________________________________
•	2. Blank Revenue Values
•	Revenue column showing blank 
•	✔ Solution:
•	Fixed relationship between Orders and Payments 
•	Ensured proper filter context 
•	________________________________________
•	3. Data Granularity Issue
•	Cost at item level 
•	Revenue at order/payment level 
•	✔ Solution:
•	Aggregated both at order level


Conclusion:
This dashboard provides a clear and complete view of business performance across customers, categories, geography, and revenue.
Key outcomes:
•	Identified total customers and transaction volume 
•	Clearly distinguished product sales and actual revenue 
•	Analyzed category-wise performance and logistics impact 
•	Highlighted top-performing countries and markets 
•	Explained revenue composition using reconciliation 
The dashboard is designed to help stakeholders quickly understand business performance and take informed decisions. It ensures clarity, consistency, and transparency in reporting.

 Overall Summary:
This project involved building a complete Power BI dashboard using the Fecom Inc. e-commerce dataset.
Key Work Done:
•	Performed data understanding and preprocessing 
•	Created relationships across multiple tables (Orders, Customers, Products, Payments, Reviews, Geolocation) 
•	Designed KPIs based on business logic 
•	Built interactive dashboards across 4 pages: 
o	Executive Summary 
o	Category Analysis 
o	Geographic Insights 
o	Revenue Reconciliation 

Key Learnings:
•	Importance of correct data modeling 
•	Difference between similar metrics (Revenue vs Sales vs Cost) 
•	Handling multi-table relationships 
•	Applying DAX for business calculations 
•	Building dashboards with clear storytelling for stakeholders 

Final Outcome:
A fully functional and business-ready dashboard that:
•	Provides actionable insights to stakeholders 
•	Ensures accurate and reliable reporting 
•	Supports data-driven decision-making










