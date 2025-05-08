https://colab.research.google.com/drive/1Vbe2QDtr1afRscTPKjYChc-ib6iUDlo2


◇ NAME:《 SANJEEVAN M  》
◇ REGISTER NUMBER: 《611823104074》
◇ DEPARTMENT:《BE.CSE 》

Github Link: https://github.com/sanjeevans408/data-analystic


Project Title: Analyzing customer purchasing behavior using association rule mining for retail optimization

Student Name: Sanjeevan.M
Register Number: 611823104074
Institution: P.S.V. College of Engineering and Technologies
Department: B.E–COMPUTER SCIENCE AND ENGINEERING 
Date of Submission:8 MAY 2025
PHASE-2
Problem Statement

In today’s highly competitive retail environment, understanding customer purchasing behavior is critical for optimizing sales, enhancing customer satisfaction, and improving inventory management. However, traditional retail decision-making often relies on intuition rather than data-driven insights. Retailers face challenges such as inefficient product placement, ineffective promotional strategies, and missed cross-selling opportunities due to the lack of deep insights into customer buying patterns.
There is a pressing need for advanced data analytics techniques to uncover hidden patterns and associations in customer transactions. Association rule mining, a key technique in data mining, provides a powerful method for discovering meaningful relationships between items frequently purchased together. By leveraging this approach, retailers can make informed decisions to optimize store layout, personalize marketing, develop effective product bundling strategies, and manage inventory more efficiently.
This project aims to analyze customer purchasing behavior using association rule mining to generate actionable insights that support strategic retail optimization.
Project Objectives
To analyze historical transactional data from a retail store to understand customer purchasing patterns and behaviors.
To apply association rule mining techniques (such as Apriori or FP-Growth) to identify frequent itemsets and discover meaningful association rules between products.
To evaluate and rank association rules based on key metrics such as support, confidence, and lift, in order to ensure the reliability and significance of the findings.
To uncover product combinations that are frequently bought together and can be used to inform cross-selling and upselling strategies.

Flowchart of the Project Workflow


Data Description
The dataset used for this project consists of retail transactional data, typically collected from point-of-sale (POS) systems. Each record in the dataset represents a transaction made by a customer and contains the following key attributes:
Attribute
Description

Transaction ID -
A unique identifier for each transaction

Customer ID   -
(Optional) Anonymized ID representing the customer who made the purchase

Date/Time     -
Timestamp of when the transaction occurred

Items Purchased
 -  A list of products bought in a single transaction

Item Code/Name  
The name or code of each individual product

Quantity
Number of units purchased for each item (optional for rule mining)

Price
Price per unit of each item (used for extended analysis like revenue patterns)






Data Preprocessing
Effective data preprocessing is a critical step before applying association rule mining. It ensures the data is clean, consistent, and in the appropriate format for mining algorithms like Apriori or FP-Growth.
Steps Involved:

1. Data Cleaning
Remove duplicates: Ensure each transaction is unique.
Handle missing values: Drop or impute records with missing transaction or item data.
Standardize item names: Convert all item names to lowercase and remove unnecessary whitespace or punctuation for consistency (e.g., "Milk ", "milk" → "milk").

2. Data Transformation
Group items by transaction: Convert raw logs into a format where each row contains the list of items purchased in a transaction.
Example:
less
CopyEdit
Transaction ID | Items
T001           | [milk, bread, butter]
T002           | [diapers, beer, chips]

3. One-Hot Encoding (Binary Matrix Creation)
Transform transactions into a format required for association rule mining:
Use one-hot encoding where each item is represented by a column.
Each row (transaction) contains 1 if the item was purchased, 0 otherwise.
Example:
milk
bread
butter
diapers
beer
chips

1
1
1
0
0
0

0
0
0
1
1
1

This format allows algorithms to compute support and confidence efficiently.

4. Filtering Rare Items (Optional)
To reduce noise and computation, items purchased in very few transactions can be filtered out (e.g., items with support < 1%).

5. Final Check
Ensure no missing values after transformation.
Validate that the total number of transactions and items match expectations.


Exploratory Data Analysis (EDA)
Exploratory Data Analysis helps in understanding the structure, patterns, and key characteristics of the transaction data before applying association rule mining. The insights gained from EDA guide preprocessing decisions and help in setting appropriate thresholds for support and confidence.

1. Overview of Transactions
Total number of transactions
Total number of unique items
Average number of items per transaction
Example Findings:
5,000 transactions
120 unique products
Avg. items/transaction: 3.6

2. Top Frequently Purchased Items
Identify the most commonly purchased products.
Plot a bar chart of top N items by frequency.
Insight Example:
Top 3 items: milk (1,200 times), bread (950), eggs (880)

3. Transaction Size Distribution
Analyze how many items are usually purchased per transaction.
Plot a histogram of transaction sizes.
Insight Example:
Most transactions contain 2–5 items.

4. Item Co-occurrence Matrix (Optional)
Show how frequently items appear together using a heatmap or co-occurrence matrix.

5. Time-Based Patterns (If Timestamps Are Available)
Analyze purchasing trends by day of the week, month, or hour.
Identify peak shopping times or seasonal demand shifts.

6. Customer Behavior (If Customer IDs Are Available)
Number of unique customers
Average basket size per customer
Repeat purchase patterns

Visualizations Recommended:
Bar plots for item frequencies
Pie chart for category distribution (if item categories are present)
Histogram for transaction size
Heatmap for item co-occurrence


Feature Engineering
Feature engineering involves transforming raw transactional data into structured formats that are suitable for mining meaningful patterns. Since association rule mining primarily works on item presence (not numerical features), the focus is on converting transactions into a usable itemset matrix and optionally enhancing it for deeper analysis.

1. Basket Creation
Group individual item records by Transaction ID to form item baskets.
Format:
less
CopyEdit
Transaction ID | Items
T001           | [milk, bread, butter]
T002           | [diapers, beer, chips]

2. One-Hot Encoding
Transform basket data into a binary matrix (also called transaction-item matrix):
Each row represents a transaction.
Each column represents a product.
1 = item present in transaction, 0 = item absent.
Example:
milk
bread
butter
diapers
beer
chips

1
1
1
0
0
0

0
0
0
1
1
1


3. Item Frequency Features (Optional)
Calculate item-level features:
Total frequency (how often each item is purchased).
Relative frequency (percentage of transactions containing the item).
These can be used to:
Filter out rare items (below a minimum support threshold).
Prioritize high-impact rules.

4. Temporal Features (If Using Timestamps)
Add time-based features such as:
Day of week
Hour of purchase
Seasonal tag (e.g., winter/summer)
This allows analysis of time-dependent purchase patterns.

5. Customer-Level Features (If Customer ID is Available)
Total number of transactions per customer
Average basket size
Frequency of purchase
Most commonly purchased items per customer
Useful for personalized rule mining or customer segmentation. 


Model Building
In this stage, we apply association rule mining algorithms to discover frequent itemsets and generate rules that reveal how products are purchased together. These rules can then be used to inform strategic decisions in retail operations.

1. Selecting the Algorithm
Two popular algorithms for association rule mining:
Apriori Algorithm
Generates candidate itemsets and filters by minimum support.
Easy to interpret but slower on large datasets.
FP-Growth Algorithm
More efficient than Apriori for large datasets.
Uses a compact data structure (FP-tree) to find frequent patterns without generating all candidates.
Toolkits Used:
Python libraries such as mlxtend, apyori, or Orange3.

2. Generating Frequent Itemsets
Use the chosen algorithm with a minimum support threshold to identify commonly co-purchased items.
python
CopyEdit
from mlxtend.frequent_patterns import apriori

frequent_itemsets = apriori(data_encoded, min_support=0.01, use_colnames=True)

3. Generating Association Rules
Generate rules from the frequent itemsets using metrics like confidence, lift, and leverage.
python
CopyEdit
from mlxtend.frequent_patterns import association_rules

rules = association_rules(frequent_itemsets, metric="lift", min_threshold=1.0)

4. Filtering the Rules
Apply thresholds to retain only strong and meaningful rules:
Minimum Confidence: e.g., ≥ 0.6
Minimum Lift: e.g., > 1.2
Optional: filter by consequents or specific product combinations.

5. Interpreting Sample Rules
Each rule is in the format:
{A, B} → {C}
"If a customer buys A and B, they are likely to also buy C"
Metrics Explained:
Support: % of transactions containing {A, B, C}
Confidence: % of transactions with A and B that also contain C
Lift: How much more likely C is bought with A and B vs. randomly

Example Output:
Rule
Support
Confidence
Lift

{bread, butter} → milk
0.08
0.72
1.5

{diapers} → beer
0.05
0.6
1.3



Visualization of Results & Model Insights
Visualization is a key part of interpreting and communicating the results of association rule mining. It helps identify strong rules, understand item relationships, and uncover patterns that inform actionable retail strategies.

1. Frequent Itemsets Bar Chart
Purpose: Identify the most commonly bought products or product pairs.
Tool: matplotlib or seaborn
Example: Top 10 frequent itemsets by support.
python
CopyEdit
import seaborn as sns
sns.barplot(x='support', y='itemsets', data=frequent_itemsets.sort_values(by='support', ascending=False).head(10))

2. Scatter Plot of Rules (Support vs Confidence)
Purpose: Spot strong rules with high support and confidence.
Tool: matplotlib or plotly
python
CopyEdit
import matplotlib.pyplot as plt

plt.scatter(rules['support'], rules['confidence'], alpha=0.6)
plt.xlabel('Support')
plt.ylabel('Confidence')
plt.title('Support vs Confidence of Association Rules')

3. Lift vs Confidence Heatmap
Purpose: Identify high-impact rules by combining lift and confidence.
Tool: seaborn, plotly
python
CopyEdit
sns.heatmap(rules.pivot_table(index='antecedents', columns='consequents', values='lift'), annot=True, cmap="YlGnBu")

4. Network Graph of Rules
Purpose: Visually map item relationships.
Tool: networkx, pyvis, or graphviz
Each node represents an item, and edges show strong associations.

5. Interactive Dashboards (Optional)
Tools like Tableau, Power BI, or Plotly Dash can create interactive dashboards showing:
Top association rules
Filterable item relationships
Time-based purchasing patterns

Key Model Insights
High-confidence rules: Bread + Butter → Milk (confidence = 0.72, lift = 1.5)
Cross-selling opportunity: Diapers → Beer (confidence = 0.6, lift = 1.3)
Frequently purchased bundles: Milk, Bread, Butter
Low-performing items: Items with very low support (< 1%) rarely co-occur; may be candidates for discontinuation or promotion.
Tools and Technologies Used
This project utilized a combination of data analytics, programming, and visualization tools to perform association rule mining and extract actionable retail insights.
Category
Tool / Technology
Purpose

Programming Language
Python
Main language for data analysis and model building

Data Manipulation
pandas, numpy
Data cleaning, transformation, and preprocessing

Association Rule Mining
mlxtend, apyori
Implementing Apriori and generating association rules

Visualization
matplotlib, seaborn, plotly, networkx
Creating bar charts, scatter plots, heatmaps, and rule graphs

Development Environment
Jupyter Notebook / Google Colab / VS Code
Writing and executing Python code interactively

Optional Dashboarding
Tableau / Power BI / Plotly Dash (optional)
Creating interactive dashboards for insights presentation

Version Control
Git (optional)
Tracking code versions and collaborating on development




Team Members 

SANJEEVAN .M  - Leader
THARUNKUMAR.S
YARAB.A
SIVANEESVARAN.S

Github Link: https://github.com/sanjeevans408/data-analystic


Project Title: Analyzing customer purchasing behavior using association rule mining for retail optimization

Student Name: Sanjeevan.M
Register Number: 611823104074
Institution: P.S.V. College of Engineering and Technologies
Department: B.E–COMPUTER SCIENCE AND ENGINEERING 
Date of Submission:8 MAY 2025
PHASE-2
Problem Statement

In today’s highly competitive retail environment, understanding customer purchasing behavior is critical for optimizing sales, enhancing customer satisfaction, and improving inventory management. However, traditional retail decision-making often relies on intuition rather than data-driven insights. Retailers face challenges such as inefficient product placement, ineffective promotional strategies, and missed cross-selling opportunities due to the lack of deep insights into customer buying patterns.
There is a pressing need for advanced data analytics techniques to uncover hidden patterns and associations in customer transactions. Association rule mining, a key technique in data mining, provides a powerful method for discovering meaningful relationships between items frequently purchased together. By leveraging this approach, retailers can make informed decisions to optimize store layout, personalize marketing, develop effective product bundling strategies, and manage inventory more efficiently.
This project aims to analyze customer purchasing behavior using association rule mining to generate actionable insights that support strategic retail optimization.
Project Objectives
To analyze historical transactional data from a retail store to understand customer purchasing patterns and behaviors.
To apply association rule mining techniques (such as Apriori or FP-Growth) to identify frequent itemsets and discover meaningful association rules between products.
To evaluate and rank association rules based on key metrics such as support, confidence, and lift, in order to ensure the reliability and significance of the findings.
To uncover product combinations that are frequently bought together and can be used to inform cross-selling and upselling strategies.

Flowchart of the Project Workflow


Data Description
The dataset used for this project consists of retail transactional data, typically collected from point-of-sale (POS) systems. Each record in the dataset represents a transaction made by a customer and contains the following key attributes:
Attribute
Description

Transaction ID -
A unique identifier for each transaction

Customer ID   -
(Optional) Anonymized ID representing the customer who made the purchase

Date/Time     -
Timestamp of when the transaction occurred

Items Purchased
 -  A list of products bought in a single transaction

Item Code/Name  
The name or code of each individual product

Quantity
Number of units purchased for each item (optional for rule mining)

Price
Price per unit of each item (used for extended analysis like revenue patterns)






Data Preprocessing
Effective data preprocessing is a critical step before applying association rule mining. It ensures the data is clean, consistent, and in the appropriate format for mining algorithms like Apriori or FP-Growth.
Steps Involved:

1. Data Cleaning
Remove duplicates: Ensure each transaction is unique.
Handle missing values: Drop or impute records with missing transaction or item data.
Standardize item names: Convert all item names to lowercase and remove unnecessary whitespace or punctuation for consistency (e.g., "Milk ", "milk" → "milk").

2. Data Transformation
Group items by transaction: Convert raw logs into a format where each row contains the list of items purchased in a transaction.
Example:
less
CopyEdit
Transaction ID | Items
T001           | [milk, bread, butter]
T002           | [diapers, beer, chips]

3. One-Hot Encoding (Binary Matrix Creation)
Transform transactions into a format required for association rule mining:
Use one-hot encoding where each item is represented by a column.
Each row (transaction) contains 1 if the item was purchased, 0 otherwise.
Example:
milk
bread
butter
diapers
beer
chips

1
1
1
0
0
0

0
0
0
1
1
1

This format allows algorithms to compute support and confidence efficiently.

4. Filtering Rare Items (Optional)
To reduce noise and computation, items purchased in very few transactions can be filtered out (e.g., items with support < 1%).

5. Final Check
Ensure no missing values after transformation.
Validate that the total number of transactions and items match expectations.


Exploratory Data Analysis (EDA)
Exploratory Data Analysis helps in understanding the structure, patterns, and key characteristics of the transaction data before applying association rule mining. The insights gained from EDA guide preprocessing decisions and help in setting appropriate thresholds for support and confidence.

1. Overview of Transactions
Total number of transactions
Total number of unique items
Average number of items per transaction
Example Findings:
5,000 transactions
120 unique products
Avg. items/transaction: 3.6

2. Top Frequently Purchased Items
Identify the most commonly purchased products.
Plot a bar chart of top N items by frequency.
Insight Example:
Top 3 items: milk (1,200 times), bread (950), eggs (880)

3. Transaction Size Distribution
Analyze how many items are usually purchased per transaction.
Plot a histogram of transaction sizes.
Insight Example:
Most transactions contain 2–5 items.

4. Item Co-occurrence Matrix (Optional)
Show how frequently items appear together using a heatmap or co-occurrence matrix.

5. Time-Based Patterns (If Timestamps Are Available)
Analyze purchasing trends by day of the week, month, or hour.
Identify peak shopping times or seasonal demand shifts.

6. Customer Behavior (If Customer IDs Are Available)
Number of unique customers
Average basket size per customer
Repeat purchase patterns

Visualizations Recommended:
Bar plots for item frequencies
Pie chart for category distribution (if item categories are present)
Histogram for transaction size
Heatmap for item co-occurrence


Feature Engineering
Feature engineering involves transforming raw transactional data into structured formats that are suitable for mining meaningful patterns. Since association rule mining primarily works on item presence (not numerical features), the focus is on converting transactions into a usable itemset matrix and optionally enhancing it for deeper analysis.

1. Basket Creation
Group individual item records by Transaction ID to form item baskets.
Format:
less
CopyEdit
Transaction ID | Items
T001           | [milk, bread, butter]
T002           | [diapers, beer, chips]

2. One-Hot Encoding
Transform basket data into a binary matrix (also called transaction-item matrix):
Each row represents a transaction.
Each column represents a product.
1 = item present in transaction, 0 = item absent.
Example:
milk
bread
butter
diapers
beer
chips

1
1
1
0
0
0

0
0
0
1
1
1


3. Item Frequency Features (Optional)
Calculate item-level features:
Total frequency (how often each item is purchased).
Relative frequency (percentage of transactions containing the item).
These can be used to:
Filter out rare items (below a minimum support threshold).
Prioritize high-impact rules.

4. Temporal Features (If Using Timestamps)
Add time-based features such as:
Day of week
Hour of purchase
Seasonal tag (e.g., winter/summer)
This allows analysis of time-dependent purchase patterns.

5. Customer-Level Features (If Customer ID is Available)
Total number of transactions per customer
Average basket size
Frequency of purchase
Most commonly purchased items per customer
Useful for personalized rule mining or customer segmentation. 


Model Building
In this stage, we apply association rule mining algorithms to discover frequent itemsets and generate rules that reveal how products are purchased together. These rules can then be used to inform strategic decisions in retail operations.

1. Selecting the Algorithm
Two popular algorithms for association rule mining:
Apriori Algorithm
Generates candidate itemsets and filters by minimum support.
Easy to interpret but slower on large datasets.
FP-Growth Algorithm
More efficient than Apriori for large datasets.
Uses a compact data structure (FP-tree) to find frequent patterns without generating all candidates.
Toolkits Used:
Python libraries such as mlxtend, apyori, or Orange3.

2. Generating Frequent Itemsets
Use the chosen algorithm with a minimum support threshold to identify commonly co-purchased items.
python
CopyEdit
from mlxtend.frequent_patterns import apriori

frequent_itemsets = apriori(data_encoded, min_support=0.01, use_colnames=True)

3. Generating Association Rules
Generate rules from the frequent itemsets using metrics like confidence, lift, and leverage.
python
CopyEdit
from mlxtend.frequent_patterns import association_rules

rules = association_rules(frequent_itemsets, metric="lift", min_threshold=1.0)

4. Filtering the Rules
Apply thresholds to retain only strong and meaningful rules:
Minimum Confidence: e.g., ≥ 0.6
Minimum Lift: e.g., > 1.2
Optional: filter by consequents or specific product combinations.

5. Interpreting Sample Rules
Each rule is in the format:
{A, B} → {C}
"If a customer buys A and B, they are likely to also buy C"
Metrics Explained:
Support: % of transactions containing {A, B, C}
Confidence: % of transactions with A and B that also contain C
Lift: How much more likely C is bought with A and B vs. randomly

Example Output:
Rule
Support
Confidence
Lift

{bread, butter} → milk
0.08
0.72
1.5

{diapers} → beer
0.05
0.6
1.3



Visualization of Results & Model Insights
Visualization is a key part of interpreting and communicating the results of association rule mining. It helps identify strong rules, understand item relationships, and uncover patterns that inform actionable retail strategies.

1. Frequent Itemsets Bar Chart
Purpose: Identify the most commonly bought products or product pairs.
Tool: matplotlib or seaborn
Example: Top 10 frequent itemsets by support.
python
CopyEdit
import seaborn as sns
sns.barplot(x='support', y='itemsets', data=frequent_itemsets.sort_values(by='support', ascending=False).head(10))

2. Scatter Plot of Rules (Support vs Confidence)
Purpose: Spot strong rules with high support and confidence.
Tool: matplotlib or plotly
python
CopyEdit
import matplotlib.pyplot as plt

plt.scatter(rules['support'], rules['confidence'], alpha=0.6)
plt.xlabel('Support')
plt.ylabel('Confidence')
plt.title('Support vs Confidence of Association Rules')

3. Lift vs Confidence Heatmap
Purpose: Identify high-impact rules by combining lift and confidence.
Tool: seaborn, plotly
python
CopyEdit
sns.heatmap(rules.pivot_table(index='antecedents', columns='consequents', values='lift'), annot=True, cmap="YlGnBu")

4. Network Graph of Rules
Purpose: Visually map item relationships.
Tool: networkx, pyvis, or graphviz
Each node represents an item, and edges show strong associations.

5. Interactive Dashboards (Optional)
Tools like Tableau, Power BI, or Plotly Dash can create interactive dashboards showing:
Top association rules
Filterable item relationships
Time-based purchasing patterns

Key Model Insights
High-confidence rules: Bread + Butter → Milk (confidence = 0.72, lift = 1.5)
Cross-selling opportunity: Diapers → Beer (confidence = 0.6, lift = 1.3)
Frequently purchased bundles: Milk, Bread, Butter
Low-performing items: Items with very low support (< 1%) rarely co-occur; may be candidates for discontinuation or promotion.
Tools and Technologies Used
This project utilized a combination of data analytics, programming, and visualization tools to perform association rule mining and extract actionable retail insights.
Category
Tool / Technology
Purpose

Programming Language
Python
Main language for data analysis and model building

Data Manipulation
pandas, numpy
Data cleaning, transformation, and preprocessing

Association Rule Mining
mlxtend, apyori
Implementing Apriori and generating association rules

Visualization
matplotlib, seaborn, plotly, networkx
Creating bar charts, scatter plots, heatmaps, and rule graphs

Development Environment
Jupyter Notebook / Google Colab / VS Code
Writing and executing Python code interactively

Optional Dashboarding
Tableau / Power BI / Plotly Dash (optional)
Creating interactive dashboards for insights presentation

Version Control
Git (optional)
Tracking code versions and collaborating on development




Team Members 

SANJEEVAN .M  - Leader
THARUNKUMAR.S
YARAB.A
SIVANEESVARAN.S

