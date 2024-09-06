# Data Warehouse Notes
Sources:
- [Data Warehouse Fundamentals for Beginners](https://www.udemy.com/course/data-warehouse-fundamentals-for-beginners/)

## Data Warehouse Concepts
A Data Warehouse is a data management system that stores current and historical data from multiple sources in a business-friendly manner for easier insights and reporting. 

Data warehouses exist to support data-driven decision making.

The data comes from elsewhere and is not created in the data warehouse.

Data is copied, not moved. It remains in the source systems and is copied to the data warehouse.

There are rules governing how data warehouses are built and how the data is organized and stored. Bill Inmon (1990):

- Integrated - A data warehouse is an integrated environment. Data from a number of different source systems is sent into a data warehouse.
- Subject-oriented - data is reorganized by subjects, not by the original systems
- Time variant - data warehouses contain historical, not just current data
- Non-volatile - data is periodically loading into the data warehouse in batches (refreshing the data)/ The data warehouse remains stable in between refreshes.

As data is brought into the data warehouse it is typically restructured and reorganized to make it more useful for analysis.

ETL (Extract Transform Load) moves data from source systems to the data warehouse. Sometimes, the data will be copied again into smaller environments called data marts. Think of the data warehouse as a wholesaler and data marts as retailers.

#### Reasons for Building A Data Warehouse
Data warehouses:
- Support making decisions in a data-driven manner
- Provide a one-stop-shop for all needed data

The data provides insights into the past and present of the business, as well as surfacing unknown but interesting and important insights.

Business Intelligence and data warehousing both emerged c.1990 and provide tremendous synergy.

Without data warehouses providing a one-stop-shop, data would need to be sourced from individual applications or extract files, making analysis slower and more difficult. Data warehouses let you focus on analysing the data, rather than repeatedly gathering it.

#### Comparing a Data Warehouse to a Data Lake
A Data Warehouse is often built on top of a relational database management system, such as MS SQL Server. 

Data warehouses are sometimes built on top of a multi-dimensional database (cube).

A Data Lake in contrast is built on top of some sort of big data environment, rather than a traditional RDBMS.

Big data:
- Volume - helps you to manage extremely large volumes of data
- Velocity - supports very rapid intake of new and changed data
- Variety - supports structured, semi-structured, and unstructured data

While data lakes are sometimes presented as the next generation of data warehouses, most large organisations have both data warehousing and data lake environments.

The lines between data warehouses and data lakes are increasingly blurred. SQL can be layered on top of a big data environment, allowing traditional BI to be performed against either a data warehouse or data lake.

#### Data Virtualization
Before data warehousing, data-driven decision making was often done by building extract files without any coordination of data structures and rules.

In the late 1980s most of the major computer systems companies were working on distributed database management systems (DDBMS), using an index of disparate applications and their data. 

The technology wasn't there to support this, which led to two divergent paths: data virtualization and data warehousing. 

Data virtualization can be thought of as a read-only RDBMS. Its key point was that data was accessed from its original location at time of need, and not duplicated.

Data virtualization is useful in certain niche uses cases:
- Data that requires simple (or no) transformations
- A small number of data sources
- Relaxed response time


## Data Warehousing Architecture
### Centralized Data Warehouse
A single data warehousing environment. This should be the default architecture. 

All data sources feed into a single database. 

The most obvious advantage of a centralized data warehouse is support for one stop shopping for reporting, business intelligence and analytics.

Traditionally centralized data warehouses faced technological challenges as relational database management systems were maturing. Work processes needed to be developed. The centralization of data warehousing required a high degree of organizational and inter-personal cooperation.

### Data Mart
A data warehouse doesn't necessarily need to be the endpoint for the flow of data from your applications. This can be a data mart, which you can think of as a small-scale data warehouse with its data organized internally in the same way. A data mart acts as a data retailer to the data warehouse's wholesaler.

Data marts can be dependent on or independent of data warehouses. Independent data marts draw data directly from one or more source applications. Independent data marts are similar to old fashioned data extract files but their data is usually organized dimensionally, rather than by report.

|Dependent Data Mart| Independent Data Mart|
|---|---|
|Sourced from data warehouse|Sourced directly from applications and systems|
|(Mostly) uniform data across marts|Little or no uniformity across marts|
|Architecturally straightforward| "Spaghetti" architecture|

|Data Warehouse| Independent Data Mart|
|--|--|
|Many sources|One or more sources|
|ETL from sources|ETL from sources|
|Probably large data volumes|Possibly large data volumes|
|Dimensionally organized data|Dimensionally organized data|

### Data Warehousing Architectural Options
First Decision: Centralized or Component-Based

Centralized:
- Default option
- One stop shop
- Modern technology
- Requires high cross-org cooperation
- High data governance
- Ripple effects

Component-based:
- Decomposition
- Mix and match technology
- "Bolt together" components
- Overcome org challenges
- Often inconsistent data
- Difficult to cross-integrate

Within Centralized architecture, could be EDW or Data Lake:

EDW - Enterprise Data Warehouse - default approach for centralized environment. Within EDW, could be relational or specialized databases (such as columnar)

Data Lake, could use specialized databases, Hadoop, AWS S3

Within component-based option, could use architected or non-architected approach.

Architected approach: data warehouses + data marts or data marts only

Data marts can be back-end, behind the data warehouse, or front-end, in front of the data warehouse.

Data warehouse bus - all data marts follow principle of "conformed dimensions", subject matter can be compared across data marts even if the data marts are built on different platforms.

In a non-architected approach, a collection of independent data marts can be considered a federated data warehouse where you "agree to disagree" about business rules, data models and structures. This is something of a last resort.

### Cubes in Data Warehouse Environment
A Cube is a multidimensional database, not a relational database. It is a specialized "dimensionally aware" database. Cubes were a leading alternative to first generation RDBMSs. Today, cubes are best for small scale data warehouse and data marts.

Advantages:
- Fast query response time
- Good for "modest" data volumes

Disadvantages:
- Less flexible data structures than RDBMS
- More vendor variation than with RDBMS

Sometimes data warehouses take advantage of both RDBMS and cubes. For example the data warehouse could be built on a RDBMS and the data marts built on cubes. OR the marts could mix and match relational databases and cubes.

### Including Operational Data Stores
An Operational Data Store (ODS) integrates data from multiple sources into a single store. Unlike a data warehouse, an ODS focuses primarily on **current** operational data.

Often ODS receive real-time feeds rather than batch-oriented ETL feeds. The focus is on "tell me what is happening right now".

ODSs were popular in the late 90s/early 2000s. One option was for ODSs and data warehouses to coexist, with parallel rapid ETLs to the ODs and batch ETLs to the data warehouse. A second option was to first send all data via rapid ETL to the ODS and then via batch ETL to the data warehouse. Users went to the ODs for operational queries and the data warehouse for strategic queries.

ODSs are less popular as current data warehouses are faster and more current. Superseded by big data. You will sometimes find an ODS component within a data lake. Traditional ODSs are still sometimes used for mission-critical situations.

### The Role of the Staging Layer
Data warehouses contain two layers: the staging layer and the user access layer.

The staging area is a landing zone for incoming data. Part of the E in ETL. The user access layer is where users go. 

Inside the staging layer, the data from the source application is mirrored as closely as possible.

Your objective with the staging layer is to focus on the E in ETL, not the T. The transformations should take place in the data warehouse environment.

There are two types of staging layers: **non-persistent** and **persistent**.

With a non-persistent staging layer, it is empty prior to each data warehouse update/refresh and is emptied after data has been moved for the staging layer to the user access layer.

With a persistent staging layer, there is already some data in the staging layer when each update/refresh starts. It is not emptied out when the data is moved to the user access layer.

Non-persistent staging layer:
- Less storage space needed
- Data already moved to user access layer
- If you need to rebuild user layer, you need to go back to source systems to 
- Data QA needs to check source systems

Persistent staging layer:
- Can rebuild user layer without source systems
- Data QA can compare staging layer and user access layer
- More storage space is needed
- Risk of "ungoverned" access

## Bringing Data into a Data Warehouse
### ETL and ELT
ETL (Extract Transform Load) is a data flow technique for bringing data from source applications into a data warehouse.

**Extract** - quickly pull data from source application. Traditionally done in batches. You get raw data, errors and all. The data lands in the data warehouse staging layer.

**Transform** - prepare the data to be uniform in the user access layer. This can be very complex.

**Load** - store uniform data in user access layer

Challenges with traditional ETL:
- Significant business analysis required before storing data
- Significant data modeling before storing data

ELT (Extract Load Transform) blasts data into a big data environment. The rawest form of the data, structured or unstructured, is brought into Hadoop, S3 or other low-level storage. Use big data environment computing power to transform when needed. 

Schema on write (data warehouse) - schema and data structure already set when data is loaded before being queried. 

Schema on read (data lake) - defer creating the schema until needed for analytics. 

Two varieties of ETL: Initial and Incremental

Initial ETL:
- Normally one time only
- Right before the data warehouse goes live
- All **relevant** data necessary for BI and analytics is brought in
- Redo if data warehouse "blows up"

In a data warehouse environment, you do not bring in **all** possible source data. Bring in data that is definitely, or probably, needed for BI and analytics. You will also bring in historical data, if any exists when data warehouse set up.

Incremental ETL:
- Incrementally refreshes the data warehouse
- Brings in new and modified data
- Special handling for deleted data (soft deletion)

The purpose of an incremental ETL is to bring the data warehouse up to date.

Incremental ETL patterns:
- Append - new data appended on to existing data
- In-place update - insert new data into existing data
- Complete replacement - replace all data in a portion of the data warehouse, even if change is only to some part
- Rolling append - new data is appended and oldest data is deleted, only a certain duration of history is maintained

Today, almost all incremental ETL uses append or in-place update.

Any given incremental ETL feed may be running on a different interval and using a different pattern. This can happen at the table level. Design and build what makes sense for your environment.

### Data Transformation
Data transformation has two main goals: data uniformity and data restructuring.

Common transformation models:
- Data value unification
- Data type and size unification
- De-duplication
- Dropping columns (vertical slicing)
- Value-based row filtering (horizontal slicing)
- Correcting known errors

## Dimensional Modeling
Ask yourself how your data warehouse will be used.

Business Intelligence categories drive data models.

|BI Category| Data Model|
|--|--|
|Basic reporting|Dimensional|
|Online analytical processing (OLAP| Dimensional|
|Predictive analysis|Data mining / specialized|
|Exploratory analytics| Data mining / specialized|

### Principles of Dimensionality
Making data-driven decisions in a dimensional sense requires: one or more measurements and dimensional context for each measurement.

Dimensional context is brought using "by" and "for":
- What is the average annual faculty salary by rank, by department, by academic year?
- What is the average student loan balance by major, by class year, by campus?
- What is the average annual faculty salary for assistant professors, by department, by academic year?
- What is the average student loan balance for engineering majors, by class year, by campus?

By - slice and group by values of the entire dimension
For - One or more specific values from within the entire dimension

By or for could be implicit - What is the average annual faculty salary for this academic year for assistant professors, by department?

### Compare Facts, Fact Tables, Dimensions, and Dimension Tables
In a data warehousing context, measurements are **facts**, dimensional contexts are **dimensions**.

Facts are numeric and quantifiable, a measurement or metric, such as: salary, number of credits, dollar amount, years worked. A data warehousing fact is not the same as a logical fact.

When you use a relational database for your data warehouse, you store facts in a **fact table**.

A dimension is the context for a fact, such as: an academic department, a major, an employee. You store dimensions in a **dimension table**.

Terminology around dimensions and dimension tables can be imprecise. In a star schema, a hierarchy of product, product family, product category results in 1 dimensional table (the product dimension table) which is actually 3 dimensions from the same hierarchy in 1 table. In a snowflake schema, these 3 dimensions would result in 3 dimension tables.

### Different Forms of Additivity in Facts
A data warehousing fact can be additive, non-additive or semi-additive.

An additive fact can be added under all circumstances, such as an employee's salary or hours worked.

Non-additive facts can be stored but not added up - margins, ratios, percentages, calculated average - for example grade point average.

With non-additive facts, store the underlying components in fact tables. You might also store non-additive facts at an individual row-level for easy access. Calculate aggregate averages, ratios, percentages from totals of underlying components.

Semi-additive facts can sometimes be added, but not always. These are typically used in periodic snapshot fact tables.

### Star Schema vs Snowflake Schema
|Star Schema|Snowflake Schema|
|---|--|
|All dimensions along a given hierarchy in one dimension table|Each dimension/dimensional level in its own table|
|Only one level away from fact table along each hierarchy|One or more levels away from fact table along each hierarchy|
|With one fact table, visually resembles a star|With one fact table, visually resembles a snowflake|
|Overall fewer database joins for drilling up/down|Overall more database joins for drilling up/down|
|Database primary -> foreign key relationship is straightforward|Database primary -> foreign key relationship is more compex|
|Typically more database storage needed for dimensional data|Typically less database storage needed for dimensional data|
|"Denormalized" dimension table data|"Normalized" dimension table data|

Star schemas and snowflake schemas have the same dimensions just different table representations.

### Database Keys for Data Warehousing
#### Primary and Foreign Keys for Dimension Tables
Star and snowflake schemas are implemented in RDBMSs. RDBMSs use logical relationships to relate data across tables. Technically any data can be related to any other data. "Official" relationships are handled through keys.

A primary key is a unique identifier for each row in a database table. It could be a single database column (field) or might require more than one database column.

A foreign key is another table's primary key, used to officially indicate logical relationships across tables. This helps to enforce data integrity and improve query performance.

A natural key might be cryptic (1234_abc_5678) or understandable (marketing_001). They "travel" from source systems with the rest of the data. 

In data warehousing, it is a best practice to use surrogate keys to relate data across tables. These are often suffixed with `_key`.

Surrogate keys:
- Have no business meaning
- Differ from a cryptic natural key
- Generated by the database itself or a supplemental "key management" system.

|Question/Decision|Guidance|
|--|--|
|Use surrogate or natural keys as primary and foreign keys?|Add surrogate keys as data brought into data warehouse|
|Keep or discard natural keys in dimension tables?|Keep as "secondary keys"|
|Keep or discard natural keys in fact tables?|Experts differ, but guidance is to discard|

## Design Facts, Fact Tables, Dimensions, and Dimension Tables
Dimension tables:
- Key data warehousing subject areas
- Subject areas that provide context to measurements
- All (or most) meaningful information
- "One stop shop" for that dimensional subject
- Often suffixed `_dim`.

#### Hierarchical vs Flat Dimension
In a star schema, hierarchical dimensions are represented in a single table with one surrogate key. In a snowflake schema, hierarchical dimensions are represented in multiple tables, each with a surrogate key.

### Types of Fact Tables
- Transaction fact tables
- Periodic snapshot fact tables
- Accumulating snapshot fact tables
- Factless fact tables

|Fact table type|Usage|
|--|--|
|Transaction|Record facts (measurements) from transactions|
|Periodic Snapshot|Track a given measurement at regular intervals|
|Accumulating Snapshot|Track the progress of a business process through formally defined steps|
|Factless|1 - Record occurrences of a transaction that has no measurements 
||2- Record coverage or eligibility relationships|

#### Transaction Fact Tables
Formally known as transaction-grained fact tables, these are the heart of dimensional models. Often suffixed with `_fact`.

Literally a table where you store facts from your transactions. The context is provided by one or more dimension tables liked via surrogate keys.

For example, for a `tuition_payment_fact` table, you might include: `tuition_payment`, `student_key`, `date_key`.

One or more facts can be stored together in a single fact table. There are rules for how many facts can be in any given fact table.

You can store 2 or more facts together in the same fact table if:

1. Facts are available at the same grain (level of detail)
2. Facts occur simultaneously

For example, don't store a tuition bill and tuition payment in the same fact table, since the payment will likely occur at a different time than the bill.

Don't violate these rules:
- Complicates data analysis
- Requires SQL workarounds
- Business processes belong in their own separate fact tables (such as, billing and payments)

#### Primary and Foreign Keys for Fact Tables
In a fact table, the primary key is the **combination** of all foreign keys relating back to dimension tables. Even if the fact table has a natural key.

#### Periodic Snapshot Fact Tables
A periodic snapshot fact table is used to take and record regular periodic measurements, usually some sort of level. They provide much easier, more direct access for certain types of business questions.

Two types:
- Aggregated result of "regular" transactions
- Levels not related to "regular" transactions

Both are structurally the same. The difference is that the second type has no transactions from which the levels are built. The levels "just exist" and can be measured, but aren't constructed from transactions.

Semi-additive facts are used in periodic snapshot fact tables. While you can't perform addition, you can:
- Perform other numeric operations along the time dimension (such as averages)
- "Lock" the time dimension to a specific value and then add values (such as a particular week)

#### Accumulating Snapshot Fact Tables
Accumulating snapshot fact tables track the progress of a business process when there are formally defined stages.

- Measure elapsed time spent in each phase
- Include both completed and in-progress phases
- Can also track other measurements as process proceeds
- Introduces the concept of multiple relationships from a fact table back to single dimensional table

For example, a financial aid accumulative snapshot fact table might relate to a date dimensional table via day_submitted_key, day_screened_key, day_preliminary_decision_key, day_final_decision_key, and day_payment_key.

#### Factless Fact Tables
A factless fact table is used when you have an event that occurs that you want to track but there is nothing significant to measure about this event.

For example, a fact table for webinar registrations ("How many students registered for all of our webinars held last semester?") might have keys relating to dimensional tables for webinars, students and dates but doesn't itself contain any "facts".

1st type of factless fact table:
- The "measurement" is actually the occurrence of the event
- The presence of a row in the factless fact table is therefore the measurement
- You can count rows with or without filters
- Factless fact table = PK/FK columns only

You can have multiple factless fact tables in the same schema, such as webinar_registration_fact and webinar_attendance_fact.

You can also combinar factless and transaction fact tables. For example, webinar_attendance_fact might contain a column for attendance_duration.

Sometimes a factless fact table can use a "tracking fact".

Tracking fact:
- Value is always set to 1
- Allows you to use SQL SUM() rather than COUNT() (more readable SQL but possibly less efficient execution)

2nd type of factless fact table:
- Recording a particular relationship or association among multiple parties even if no transactions actually occurred
- Typically (but not always) between a starting and ending date or time

For example, for tracking advisers assigned to students:
- If a student visits an adviser, you could use the first type of factless fact table to record that occurrence
- If you record the duration of the visit, you could use a regular transaction-grained fact table
- If a student never visited their adviser but you still want a record of the assignment, you could use the second type of factless fact table

#### Comparing the Structure of Fact Tables in Star and Snowflake Schemas
In a star schema, you only need one column in a fact table to relate to the entire hierarchy that exists in a single dimensional table. 

In a snowflake schema, you could have PK-FK links to every level of every hierarchy but it gets unwieldy and complicated ("centipede fact tables" - Kimball).

Instead, for a snowflake schema, only store the lowest level PK-FK relationship in the fact table. The lowest dimension table in the hierarchy already contains a foreign key to dimension table at the next level up. Use SQL (joins, GROUP BY) to access higher levels.

#### SQL for Dimension and Fact Tables
Star schema dimensional table:

```
CREATE TABLE Faculty_DIM (
faculty_key           INT NOT NULL,
faculty_id            VARCHAR(10) NOT NULL,
faculty_last_name     VARCAHR(40) NOT NULL,
faculty_first_name    VARCHAR(40) NOT NULL,
year_joined           INT,
faculty_rank          VARCHAR(20) NOT NULL,
dept_id               VARCHAR(10) NOT NULL,
dept_name             VARCHAR(40) NOT NULL,
dept_year_founded     INT,
college_id            VARCHAR(10) NOT NULL,
college_name          VARCHAR(40) NOT NULL,
colleage_year_founded INT,
dean                  VARCHAR(50) NOT NULL,
...
PRIMARY KEY(faculty_key));
```

Snowflake schema non-terminal dimension table

```
CREATE TABLE Faculty_DIM (
faculty_key           INT NOT NULL,
faculty_id            VARCHAR(10) NOT NULL,
faculty_last_name     VARCAHR(40) NOT NULL,
faculty_first_name    VARCHAR(40) NOT NULL,
year_joined           INT,
faculty_rank          VARCHAR(20) NOT NULL,
department_key        VARCHAR(20) NOT NULL,

...
PRIMARY KEY(faculty_key),
FOREIGN KEY(department_key) REFERENCES Department_DIM(department_key));
```

Transaction-grained fact table:

```
CREATE TABLE Tuition_Bill_FACT (
student_key            INT NOT NULL,
date_kkey              INT NOT NULL,
tuition_bill_amount    DECIMAL(8,2) NOT NULL,
activities_bill_amount DECIMAL(8,2)
PRIMARY KEY(student_key, date_key),
FOREIGN KEY(student_key) REFERENCES Student_DIM(student_key), 
FOREIGN KEY(date_key) REFERENCES Date_DIM (date_key));
```

Periodic snapshot fact table:

```
CREATE TABLE Meal_Card_Payment_FACT (\
student_key INT NOT NULL,
date_key INT NOT NULL,
campus_food_key INT NOT NULL,
meal_card_balance DECIMAL(8,2) NOT NULL,
PRIMARY KEY(student_key, date_key, campus_food_key),
FOREIGN KEY(student_key) REFERENCES Student_DIM(student_key),
FOREIGN KEY(date_key) REFERENCES Date_DIM(date_key),
FOREIGN KEY(campus_food_key) REFERENCES Campus_Food_DIM(campus_food_key));
```

## Slowly Changing Dimensions
Slowly Changing Dimensions (SCDs) are techniques used to manage history within a data warehouse. There are multiple techniques based on various historical data policies. SCDs enable data warehouses to appropriately manage history regardless of policies in transactional applications.

Three main policies for historical data:
- Type 1 - Overwrite old data; no retention of history
- Type 2- Maintain unlimited history
- Type 3 - Maintain limited history 

|SCD Type|Technique|Implications|
|--|--|--|
|Type 1|"In Place Update" ETL Pattern|Simplest, but no history maintained|
|Type 2|Create new dimension table row for each new "version" of history|Most architecturally complex, but robust representation of history|
|Type 3|Small number of dimension table columns for multiple "versions" of history|Easily switch back and forth between "as-is" and "as-was" reporting, but limited use cases|

- Type 0 - always retain original value
- Type 1 - overwrite
- Type 2 - new row
- Type 3 - new column
- Type 4 - mini-dimension (split out more frequently changed columns)
- Types 5, 6, 7 - various hybrid techniques

### Type 1 SCD
- Replace old value with new value
- Same row and column in database table
- Common use case - correct errors
- History is lost forever

|Advantages|Disadvantages|
|--|--|
|Simplest and most straightforward|Might still want history of errors for auditing purposes|
|Data warehouse content errors are purged forever|Reporting before and after Type 1 change could vary|
|Best for error correction and any other "don't need any history" situations|Tendency to overuse Type 1 changes since much simpler than Type 2|

### Type 2 SCD
- Existing row remains as-is
- New row added to the dimension table
- New row reflects the current state of all attributes
- Complications with reporting and analytics
- New row gets new PK (surrogate key)
- Historical data is available for historical reporting. Reporting and analytics before the type 2 change needs to accurately reflect data values.

#### Correct Reporting with Type 2 SCDs
- Best handled through careful "multi-step" SQL between dimension tables and fact tables
- First: find all Surrogate Keys for desired student
- Next: retrieve all fact table rows for **all** of the relevant surrogate keys

#### Maintain Correct Data Order with Type 2 SCDs
The base version of a Type 2 change maintains limitless versions but doesn't indicate the order of those versions.

3 main solutions:
- current_flag set to Yes/No
- Effective and Expiration dates for every row (data is current from eff_date to exp_date)
- Use a combination of both

### Type 3 SCD
- Add new **column** rather than new row to reflect changes
- Results in an "old value" column and a "new value" column
- Supports back-and-forth switching for flexible reporting and analytics
- Best used for when **all** data in a dimension being "reorganized"

For example, a publisher has North and South divisions, reorganizes to have East, Central and West divisions, but wants to be able to report earlier sales data sliced by old North and South divisions and new sales data by East, Central and West divisions. They also want to be able to view older data as if sliced into new East, Central and West divisions, or combine old and new data, sliced by either old or new division structures.

Limitations:
- Use cases where **every** row in a dimensional table is (or might) change **at the same time**. Classic use case is reorganization
- Not suited for random changes such as a student's current address

Type 3 SCDs are not limited to only 2 columns (previous vs current). You could have multiple columns, reflecting multiple reorganizations over time. But a large number of changes eventually becomes unwieldy and is better handled with Type 2 SCDs. 

## Designing ETLs
### Best Practices
- Limit the amount of incoming data to be processed (Change Data Capture - limit to new/modified data)
- Process dimension tables before fact tables
- Look for opportunities for parallel processing

### Change Data Capture
Change Data Capture is a way to detect in source systems whether a piece of data is new or changed. This is achieved via:

- Transactional data timestamps
- Database logs
- Database scan and compare (last resort)

### Dimension Table ETL
- Step 1 - data preparation
- Step 2 - data transformation
- Step 3 - process new dimension rows
- Step 4 - process Type 1 SCD changes
- Step 5 - process Type 2 SCD changes

For each row:
- If new, add to dimension table
- If not new, process any Type 1 and Type 2 changes

#### SCD Type 1 Complication
- You might need to apply any given Type 1 change (in-place update) to more than 1 row because
- If a Type 2 change occurred first, you will have more than one version of the data you need to change
- Look for all dimension table rows for that natural key

#### SCD Type 2
- Basic append with new surrogate key
- Use the natural key as a guide

### Fact Table ETL
For fact tables, ETL is the basic append model with a slight twist.

The transactional system provides natural keys. After a Type 2 SCD change, there will be multiple versions of an entity. 

You need natural keys and a "relevant" date to know which version of an entity you should be using. Perform a lookup into the dimensional table and look for date between eff_date and exp_date.

Watch out for complications with **all** dimension and fact table data during ETL.

## Selecting Data Warehouse Environments
Data warehousing has traditionally been on-premise but cloud computing increasingly has platform and hosting implications for data warehousing.

#### Advantages to cloud-based data warehousing
- Offload routine system maintenance and upgrades
- (Likely) lower initial platform investment
- Disaster recovery
- Data lake / big data synergies

#### Challenges to cloud-based data warehousing
- Security (two-edged sword)
- Migrating existing data warehouses from on-premise to cloud
- Cloud to cloud data transport
- Data transport between corporate data center and cloud
- Different cost models for data ingress and egress

#### Data egress cost challenges
- Costs for large data volumes
- Not just operational data egress costs
- Development, testing, QA
- Experimental or exploratory analytics

### Architectural and Design Implications
- Data warehousing is shifting towards the cloud
- Data lakes are either sitting alongside or, in some cases, succeeding data warehouses.
- RDBMS are still prevalent but are being supplemented by OLAP/BI

Regardless of the underlying platform and physical data model, the user view of that data when they're doing classic business intelligence is going to be heavily dimensional.

You are going to want to have some type of dimensional view, even if using a specialized database or cloud service, to make that data look relational.
