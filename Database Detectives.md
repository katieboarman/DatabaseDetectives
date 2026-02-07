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
<br> **Join:** CollectionItem and SaleItem

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
<br> The schema uses one-to-many relationships.

<br> **What evidence supports or contradicts each hypothesis?**
<br> **H1:** Styles differ by how often they are sold.
This table provides strong evidence that styles differ in how often they are sold due to the large spread, from hundreds of sales for some styles to none for others.

<br> **H2:** Formats differ in how many recordings exist in each format.
The results strongly support the idea that formats differ in how many recordings exist in each format because the number of recordings varies dramatically by format.

<br> **H3:** Collections contain multiple recordings.
This table clearly supports the hypothesis that collections contain multiple recordings because every collection contains more than one recording, with counts ranging from 15 up to 3,290.

<br> **Do joins behave as expected?**
Yes

<br> **Are there orphaned records?**
No

<br> **Do joins unexpectedly multiply rows?**
There was row multiplication for H1 and H2 due to the one-to-many relationships, such as Style to Recording to SaleItem and Collection to CollectionItem. However, no unexpected row multiplication occured in the Format to Recording join, since each recording belongs to exactly one format.

### Task 4 - Aggregation & Uniqueness Analysis
**Use aggregation to better understand:**
**Entity counts**
The database contains 59 clients, 412 sales, and 3,503 recordings, indicating that the catalog of recordings is much larger than the customer base and transactional data. The number of sales compared to the number of clients implies that many clients have made multiple purchases, rather than sales being evenly distributed one-to-one across clients. Additionally, the large disparity between recordings and sales suggests that only a subset of the available recordings are purchased.

**Attribute uniqueness**
The query checking duplicates of ClientId returned an empty set, supporting that ClientId is unique and functions as a primary key. In contrast, grouping by Collection.Name revealed duplicates, indicating that collection names are descriptive rather than unique identifiers.

**Relationship multiplicity**
Relationship multiplicity was examined by grouping tables by their foreign keys: grouping recordings by FormatId showed that each format is associated with many recordings, and grouping CollectionItem by CollectionId showed that collections contain multiple items, with substantial variation in collection size.

<br> **What do aggregates reveal that simple inspection did not?**
Aggregates revealed differences in scale, confirmed which attributes are truly unique, and exposed one-to-many and many-to-many relationship patterns that are not visible through simple inspection of individual tables or rows.

<br> **Where do assumptions break down?**
The assumptions break down when descriptive attributes are treated as identifiers, when relationships are assumed to be evenly distributed, and when the existence of records is assumed to imply participation in downstream relationships.

### Task 5 - Anomalies & Design Issues
<br> **Redundant data**
Contact and address information is duplicated across Client and Staff tables. While this may be intentional, it represents redundant storage of the same type of data and could lead to inconsistent formatting.

<br> **Repeating Groups**
Repeated collection names indicate repeating groups where the name is descriptive rather than unique. The database correctly uses CollectionId to distinguish collections, but the repetition shows that names alone cannot identify a collection.

<br> **Inconsistent Values**
Although no direct contradictions appear, format names combine different concepts, which risks inconsistent labeling and complicates querying.

<br> **Columns that Encode Multiple Facts**
The Format.Name column encodes multiple facts within a single attribute, violating atomicity. Separating these into distinct columns would improve normalization and reduce ambiguity.

### Task 6 - Explore the Data
<br> **1. One format accounts for the vast majority of recordings**
<br> **Fact Statement**
Over 85% of all recordings in the database use a single format: MPEG audio file.

<br> **SQL Evidence**
MPEG audio file: 3034 recordings
Total recordings: 3503

<br> **Explanation of Significance**
At first glance, the presence of multiple formats suggests a diverse catalog. Aggregation reveals that the dataset is actually heavily dominated by one format, with all other formats making up a small minority. This has implications for storage, compatibility, and sales analysis.

<br> **2. Styles differ dramatically in how often they are sold**
<br> **Fact Statement**
Sales are highly uneven across musical styles, and at least one style has zero recorded sales.

<br> **SQL Evidence**
Rock: 835 sales
Latin: 386
Jazz: 80
Opera: 0

<br> **Explanation of Significance**
Simply inspecting the Style table shows many genres, but aggregation reveals that not all styles participate in sales at all. The presence of styles with zero sales demonstrates that inclusion in the catalog does not imply demand. This highlights a disconnect between content availability and consumer behavior, which is not visible without joining and aggregating across tables.


<br> **3. Collections vary enormously in size**
<br> **Fact Statement**
Collections in the database range from containing a single recording to containing over 3,000 recordings, indicating extreme variation in collection scope.

<br> **SQL Evidence**
Largest collection: 3290 recordings
Medium collection: 213 recordings
Smallest collection: 1 recording

<br> **Explanation of Significance**
Without aggregation, collections appear conceptually similar. Aggregation shows that collections serve very different roles: some act as broad catch-all groupings while others are highly specialized. This reveals that “collection” is not a uniform concept in the data, but a flexible structure used at vastly different scales.

## Appendices

### AI Traceability Log

AI Used: ChatGPT

![Alt text](path/to/image1.png "Question About Aggregation")

![Alt text](path/to/image2.png "Question About Terminal")

![Alt text](path/to/image3.png "Troubleshooting the Release Table")

![Alt text](path/to/image4.png "Question About Column Types")

![Alt text](path/to/image5.png "Examples About Hypotheses")

Overall, AI was an incredibly helpful tool for this project. I felt confident in using the skills we have learned in class, but there was some disconnect on how and when to apply them. It was also my first time using a terminal on Samford's network to access a database, so there were some questions concerning that. With that being said, there were some issues with its use, mainly with my promots not being specific enough or not providing the proper amount of information. The first four prompts I have listed were fairly straightforward; however, the final question I asked ChatGPT came with some complications. I originally asked "What hypotheses could we make about this database?" It did provide a list of interesting ideas, but they were not specific to the relationships between tables. When I added that specification, it generated another list, but those could not be tested by joining tables. I went back another time to clarify, but the new list required techniques that were much more complex than what we were doing in class. Finally, once I told ChatGPT what course I was in, it gave my results that were specfic to the relationships between tables, could be tested by joining them, and were on level for our course.
### SQL Log
```sql
USE COSC315DD;

SHOW TABLES;

DESCRIBE Client;
DESCRIBE Collection;
DESCRIBE CollectionItem;
DESCRIBE Creator;
DESCRIBE Format;
DESCRIBE Recording;
DESCRIBE Release;;
DESCRIBE Sale;
DESCRIBE SaleItem;
DESCRIBE Staff;
DESCRIBE Style;

### Testing H1
SELECT st.StyleId,
       st.Name AS StyleName,
       COUNT(si.SaleItemId) AS TimesSold
FROM Style st
JOIN Recording r
  ON r.StyleId = st.StyleId
LEFT JOIN SaleItem si
  ON si.RecordingId = r.RecordingId
GROUP BY st.StyleId, st.Name
ORDER BY TimesSold DESC;

### Testing H2
SELECT f.FormatId,
       f.Name AS FormatName,
       COUNT(r.RecordingId) AS NumRecordings
FROM Format f
LEFT JOIN Recording r
  ON r.FormatId = f.FormatId
GROUP BY f.FormatId, f.Name
ORDER BY NumRecordings DESC;

### Testing H3
SELECT c.CollectionId,
       c.Name,
       COUNT(ci.RecordingId) AS NumRecordings
FROM Collection c
JOIN CollectionItem ci
  ON ci.CollectionId = c.CollectionId
GROUP BY c.CollectionId, c.Name
HAVING COUNT(ci.RecordingId) >= 2
ORDER BY NumRecordings DESC;

##Orphan Check for H1
-- Orphan recordings 
SELECT r.RecordingId, r.StyleId
FROM Recording r
LEFT JOIN Style s ON s.StyleId = r.StyleId
WHERE r.StyleId IS NOT NULL
  AND s.StyleId IS NULL;

-- Orphan sale items
SELECT si.SaleItemId, si.RecordingId
FROM SaleItem si
LEFT JOIN Recording r ON r.RecordingId = si.RecordingId
WHERE r.RecordingId IS NULL;

### Orphan Check for H2
-- Orphan recordings (FormatId not found)
SELECT r.RecordingId, r.FormatId
FROM Recording r
LEFT JOIN Format f ON f.FormatId = r.FormatId
WHERE f.FormatId IS NULL;

### Orphan Check for H3
-- Orphan collection items (CollectionId not found)
SELECT ci.CollectionId, ci.RecordingId
FROM CollectionItem ci
LEFT JOIN Collection c ON c.CollectionId = ci.CollectionId
WHERE c.CollectionId IS NULL;

### Entity Counts
SELECT COUNT(*) FROM Client;
SELECT COUNT(*) FROM Recording;
SELECT COUNT(*) FROM Sale;

### Attribute Uniqueness
SELECT ClientId, COUNT(*)
FROM Client
GROUP BY ClientId
HAVING COUNT(*) > 1;

SELECT Name, COUNT(*)
FROM Collection
GROUP BY Name
HAVING COUNT(*) > 1;

### Relationship Multiplicity
-- Format -> Recording
SELECT FormatId, COUNT(*)
FROM Recording
GROUP BY FormatId;

-- Collection -> CollectionItem
SELECT CollectionId, COUNT(*)
FROM CollectionItem
GROUP BY CollectionId;

### Interesting Fact 1
SELECT FormatId, COUNT(*) AS NumRecordings
FROM Recording
GROUP BY FormatId
ORDER BY NumRecordings DESC;

### Interesting Fact 2
SELECT st.StyleId,
       st.Name AS StyleName,
       COUNT(si.SaleItemId) AS TimesSold
FROM Style st
LEFT JOIN Recording r ON r.StyleId = st.StyleId
LEFT JOIN SaleItem si ON si.RecordingId = r.RecordingId
GROUP BY st.StyleId, st.Name
ORDER BY TimesSold DESC;

### Interesting Fact 3
SELECT CollectionId, COUNT(*) AS NumRecordings
FROM CollectionItem
GROUP BY CollectionId
ORDER BY NumRecordings DESC;

```
