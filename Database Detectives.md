# COSC 315 Database Detectives
Name: Katie Boarman
Date: February 4, 2026

## Task Results
For this assignment, I will be using the COSC315DD database on the CS Development Server. Before exploring the database, I assume it contains multiple related tables with primary and foreign key relationships. I also assume the data represents a realistic scenario which may contain some inconsistencies or missing values in the data set. Additionally, I expect that meaningful insights will require joining tables rather than relying on a singular table.

### Task 2 - Inventory the Database
<br> **How many tables exist?**
<br> There are eleven tables in the database: Client, Collection, CollectionItem, Creator, Format, Recording, Release, Sale, SaleItem, Staff, Style

<br> **What columns do each table contain?**
Each table in the data set contains: Field, Type, Null, Key, Default, and Extra columns.

<br> **Column data types and nullability**
<br> **Client:** The Client table stores information about customers. Each client is uniquely identified by ClientId, which is the primary key. The table includes required name and email fields, along with optional company, address, and contact information. In the Client table, ClientId and AccountRepId are defined as integer values, while all remaining attributes are stored as variable-length character strings.

**Collection:** The Collection table contains an integer primary key, CollectionId, which uniquely identifies each collection and does not allow null values. The Name attribute is stored as a variable-length character string and may be null.

**CollectionItem:** The CollectionItem table consists entirely of integer attributes, CollectionId and RecordingId, which together form a composite primary key and do not allow null values.

**Creator:** Each creator is uniquely identified by the integer primary key CreatorId, which is not nullable. The Name attribute is stored as a variable-length character string and allows null values. 

**Format:** The Format table includes an integer primary key, FormatId, which does not allow null values. The Name attribute is stored as a variable-length character string and may be null. 

**Recording:** The Recording table is uniquely identified by the integer primary key RecordingId, which is not nullable. Descriptive attributes such as Name and Writer are stored as variable-length character strings. Numeric attributes such as ReleaseId, FormatId, StyleId, DurationMs, and SizeBytes use integer data types, with only FormatId and DurationMs being required. The ItemPrice key uses decimal data dype and is not nullable.

**Release:** Each release is uniquely identified by the integer primary key ReleaseId, which does not allow null values. The Title attribute is stored as a variable-length character string and is required, while the CreatorId attribute is an integer field and does not allow null values.

**Sale:** Each sale is uniquely identified by the integer primary key SaleId, which does not allow null values. The table includes temporal data (InvoiceDate) stored as a datetime value that is required, as well as billing address attributes stored as variable-length character strings, which allows null values. The total value of a sale is stored in the SaleTotal column using a decimal data type.

**SaleItem:** Each record is uniquely identified by the integer primary key SaleItemId, while SaleId and RecordingId are integer fields that reference the associated sale and recording, respectively. Pricing and quantity information is stored using decimal and integer data types, and none of the columns allow null values.

**Staff:** Each staff member is uniquely identified by the integer primary key StaffId, which does not allow null values. Personal and contact attributes such as names, address, and email are stored as variable-length character strings, only requiring inputs for LastName and FirstName. Dates such as BirthDate and HireDate are stored as datetime values and allow null values. The ManagerId field is an integer that may be null.

**Style:** Each style is uniquely identified by the integer primary key StyleId, which does not allow null values. The Name attribute is stored as a variable-length character string and may be null. 

<br> **Which tables appear to represent core entities vs. supporting or join tables?**
<br> **Core Entities:** Client, Collection, Creator, Format, Recording, Release, Sale, Staff, Style
<br> **Supporting:** Format and Style
<br> **Join:** CollectionItem and SaleItem,

<br> **Are there naming patterns that suggest relationships?**
<br> Yes, there is a repeated use of <EntityName>Id as the primary key for the tables. It suggests that each table has a clearly defined primary key that is intended to be referenced by other tables. Additionally, the table names listed as <Entity>Item indicate a join table. 

<br> **Link to Finalized DrawSQL ERD:** https://drawsql.app/teams/katie-workspace/diagrams/database-detectives-graph 

### Task 3 - Hypothesize and Test Relationships
<br> **Which columns appear to function as foreign keys?**
<br> **Client:** AccountRepId
<br> **CollectionItem:** CollectionId and RecordingId
<br> **Recording:** ReleaseId, FormatId, and StyleId
<br> **Release:** CreatorId
<br> **Sale:** ClientId
<br> **SaleItem:** SaleId and RecordingId
<br> **Staff:** ManagerId

<br> **What cardinality do you suspect (one-to-many, many-to-many)?**
<br>

<br> **What evidence supports or contradicts each hypothesis?**
<br> **Do joins behave as expected?**
<br> **Are there orphaned records?**
<br> **Do joins unexpectedly multiply rows?**

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

AI Used: ChatGPT

### SQL Log
```sql
USE COSC315DD;

SHOW TABLES;
```
