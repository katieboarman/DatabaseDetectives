# COSC 315 Database Detectives
Name: Katie Boarman
Date: February 4, 2026

## Task Results
For this assignment, I will be using the COSC315DD database on the CS Development Server. Before exploring the database, I assume it contains multiple related tables with primary and foreign key relationships. I also assume the data represents a realistic scenario which may contain some inconsistencies or missing values in the data set. Additionally, I expect that meaningful insights will require joining tables rather than relying on a singular table.

### Task 2 - Inventory the Database
<br> How many tables exist?
<br> Which tables appear to represent core entities vs. supporting or join tables?
<br> Are there naming patterns that suggest relationships?

<br> Link to Finalized DrawSQL ERD: 

### Task 3 - Hypothesize and Test Relationships
<br> Which columns appear to function as foreign keys?
<br> What cardinality do you suspect (one-to-many, many-to-many)?
<br> What evidence supports or contradicts each hypothesis?
<br> Do joins behave as expected?
<br> Are there orphaned records?
<br> Do joins unexpectedly multiply rows?

### Task 4 - Aggregation & Uniqueness Analysis
<br> What do aggregates reveal that simple inspection did not?
<br> Where do assumptions break down?

### Task 5 - Anomalies & Design Issues
<br> What design issues might exist?
<br> What evidence supports your claim?
<br> What redesign would you propose?

### Task 6 - Explore the Data
<br> 1. 
<br> Fact Statement
<br> SQL Evidence
<br> Explanation of Significance
<br> 2. 
<br> Fact Statement
<br> SQL Evidence
<br> Explanation of Significance
<br> 3.
<br> Fact Statement
<br> SQL Evidence
<br> Explanation of Significance

## Appendices

### AI Traceability Log

AI Used: 

### SQL Log
```sql
USE COSC315DD;

SHOW TABLES;
```
