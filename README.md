# Emergency Medical Services Response Analytics

## Project Data Dump

**Project type:** End-to-end data analytics and data warehousing
project\
**Domain:** Emergency Medical Services (EMS) / Emergency Incident
Dispatch\
**Primary stakeholder:** NYC EMS / FDNY Management\
**Data source:** NYC Open Data --- EMS Incident Dispatch Data\
**Dataset ID:** `76xm-jjuj`\
**Planned tools and flow:** Python → SQL Data Warehouse → Power BI

------------------------------------------------------------------------

## 1. Project Overview

The project aims to analyze NYC Emergency Medical Services (EMS)
incident dispatch data to understand emergency-call demand,
response-time patterns, and operational pressure across different times
and locations.

The intended audience is EMS / FDNY management. The project should
provide management-oriented insights that can support operational
decisions, rather than simply describing the dataset.

The central idea is to investigate when and where EMS demand is highest,
how response times behave under different demand conditions, and whether
certain periods or areas may require a review of resource allocation.

### Main project question

**How can NYC EMS management better understand incident demand and
response-time patterns to identify periods and locations where
operational resources may be under pressure?**

### Supporting questions

-   At what times of day, days of the week, and months is EMS demand
    highest?
-   Which areas experience the highest incident volume?
-   How do response times vary by time, day, month, and location?
-   Are high-demand periods associated with longer response times?
-   Are there incident categories or patterns that contribute
    significantly to workload?
-   Are there recurring demand patterns that could inform staffing or
    resource-allocation reviews?

These are analytical questions to investigate. They are not assumed
findings; conclusions must be supported by the data.

------------------------------------------------------------------------

## 2. Stakeholder and Business Value

**Primary stakeholder:** NYC EMS / FDNY Management.

The project is designed around management needs, not citizen-facing
service recommendations.

Potential value includes:

-   Understanding when EMS demand is highest.
-   Identifying locations with consistently high incident volumes.
-   Monitoring response-time patterns and variations.
-   Comparing demand and response times to identify possible pressure
    points.
-   Providing evidence that may support staffing, deployment, or
    resource-allocation reviews.
-   Making operational patterns easier to monitor through interactive
    Power BI dashboards.

The project will not claim that demand causes longer response times
unless the analysis provides sufficient evidence. Relationships and
correlations should be described carefully.

------------------------------------------------------------------------

## 3. Dataset and Scope

**Dataset:** EMS Incident Dispatch Data\
**Publisher:** NYC Open Data\
**Dataset identifier:** `76xm-jjuj`\
**API host:** `data.cityofnewyork.us`

### Planned date range

-   Start: `2023-10-01 00:00:00`
-   End: `2026-10-01 00:00:00` (exclusive)

The intended filter is:

``` sql
incident_datetime >= '2023-10-01T00:00:00'
AND incident_datetime < '2026-10-01T00:00:00'
```

An earlier count query returned **4,862,291 records** for this period.
This should be treated as the expected count until it is verified
against the downloaded export.

### Data extraction status

A filtered CSV export was successfully downloaded through the SODA3
export endpoint.

-   HTTP status: `200`
-   File: `nyc_ems_2023_2026.csv`
-   Local path used: `D:\Graduation_Project\nyc_ems_2023_2026.csv`
-   Reported file size: approximately `1.395 GB`
-   Reported download duration: approximately `7.42 minutes`

**Important:** HTTP 200 confirms that the request succeeded, but does
not by itself confirm that every expected record was exported. Verify
the CSV row count and date boundaries before treating the extract as
complete.

------------------------------------------------------------------------

## 4. End-to-End Project Architecture

``` text
NYC Open Data — SODA3 API
          |
          v
Python — Data Extraction
          |
          v
Raw CSV / Raw Data
          |
          v
Python — Data Quality Checks & Cleaning
          |
          v
Prepared / Cleaned Data
          |
          v
SQL Data Warehouse
          |
          v
SQL Transformations / Analytical Views
          |
          v
Power BI — Data Model & Measures
          |
          v
Interactive Dashboards & Business Insights
          |
          v
Management Recommendations
```

### Main technology flow

1.  **Python:** Extract data, inspect it, profile data quality, clean
    and prepare it, and load it into the warehouse.
2.  **SQL Data Warehouse:** Store structured data in a suitable
    analytical model and perform warehouse transformations and
    analytical queries.
3.  **Power BI:** Connect to the warehouse, build the data model and
    measures, and create interactive reports and dashboards.

Python, SQL, and Power BI may all be used for analysis at different
stages. The exact division between Python-based analysis and SQL-based
analysis should be agreed by the team to avoid duplicated work.

------------------------------------------------------------------------

## 5. Project Phases and Tasks

### Phase 1 --- Project Understanding and Data Exploration

-   Understand the dataset, its columns, definitions, and available time
    and location fields.
-   Confirm the date range and expected record count.
-   Inspect data types, missing values, duplicates, invalid values, and
    unusual records.
-   Identify fields relevant to incident demand, dispatch, response
    times, incident categories, and geography.
-   Document the meaning and limitations of important fields.

**Deliverables:** - Dataset overview. - Data dictionary / column
descriptions. - Initial data profiling report. - Confirmed extraction
scope.

### Phase 2 --- Data Extraction and Raw Data Management (Python)

-   Connect to the NYC Open Data SODA3 API.
-   Extract the required date range.
-   Save and preserve the original raw export.
-   Record extraction parameters, date range, extraction time, and row
    count.
-   Verify that the downloaded file is readable and complete.

**Deliverables:** - Raw CSV dataset. - Reproducible extraction script or
notebook. - Extraction and validation notes.

### Phase 3 --- Data Cleaning and Preparation (Python)

Cleaning decisions must be based on the actual dataset and documented.
Do not delete records or columns simply because they appear
inconvenient.

Potential tasks:

-   Inspect and correct data types, especially dates, times, numeric
    values, and identifiers.
-   Handle missing values according to the meaning and importance of
    each field.
-   Identify duplicate records and determine whether they are genuine
    duplicates.
-   Check invalid, inconsistent, or impossible values.
-   Standardize text categories and inconsistent labels where
    appropriate.
-   Investigate outliers in relevant numeric fields, especially
    response-time measures.
-   Check date and time consistency.
-   Create useful analytical fields, where justified, such as:
    -   Date
    -   Year
    -   Month
    -   Day of week
    -   Hour of day
    -   Time-of-day period
-   Derive response-time measures only when the required source
    timestamps are available and their definitions are understood.
-   Keep a record of cleaning rules and changes.

**Deliverables:** - Cleaned and prepared dataset. - Cleaning script /
notebook. - Data quality and cleaning report. - Documented assumptions
and exclusions.

### Phase 4 --- Exploratory Data Analysis (Python and/or SQL)

Explore the data to identify patterns and guide the final analytical
questions.

Potential analysis:

-   Incident volume over time.
-   Demand by hour and day of week.
-   Monthly and seasonal patterns.
-   Incident distribution by geographic area.
-   Response-time distribution and variation.
-   Response times across time periods and locations.
-   Differences between incident categories, where relevant fields
    exist.
-   Comparison of incident volume with response-time patterns.

**Deliverables:** - Exploratory analysis notebook or SQL queries. -
Charts and summary tables. - Initial findings and questions for deeper
analysis.

### Phase 5 --- SQL Data Warehouse Design and Implementation

Design a warehouse that supports efficient reporting and analysis.

Potential tasks:

-   Define the warehouse requirements and analytical grain.
-   Design fact and dimension tables based on the actual dataset.
-   Identify keys and relationships.
-   Create appropriate date, time, location, and incident-related
    dimensions where supported by the source data.
-   Design the incident/dispatch fact table at a clearly documented
    grain.
-   Create staging tables for loading prepared data.
-   Build SQL scripts for table creation and data loading.
-   Apply transformations and data-quality checks in SQL where
    appropriate.
-   Create analytical views or aggregate tables for Power BI.
-   Validate row counts, keys, relationships, and important metrics
    after loading.

**Possible warehouse components** (final design depends on the source
fields):

-   Fact table: EMS incident / dispatch records.
-   Date dimension.
-   Time dimension.
-   Geographic or area dimension.
-   Incident-type or category dimension.
-   Other dimensions only where the source data supports them.

Do not assume that every suggested dimension is available or necessary.
The team should choose the warehouse grain and schema after examining
the data.

**Deliverables:** - Warehouse architecture / schema diagram. - SQL
table-creation scripts. - Data-loading and transformation scripts. -
Analytical SQL views. - Warehouse validation results.

### Phase 6 --- Business Analysis (SQL and/or Python)

Use the cleaned data and warehouse to answer the project's business
questions.

Potential analyses:

-   Identify peak demand periods.
-   Rank areas by incident volume.
-   Compare response-time performance across areas and time periods.
-   Identify periods with both high incident volume and slower response
    times.
-   Examine recurring patterns and exceptions.
-   Quantify differences between groups and time periods.
-   Translate supported findings into management-relevant insights.

For each finding, record:

1.  The business question.
2.  The metric and its definition.
3.  The relevant time period and population.
4.  The result.
5.  The operational interpretation.
6.  Any limitations or alternative explanations.

**Deliverables:** - Final analytical queries / notebooks. - Key findings
supported by metrics. - Management-oriented recommendations and
limitations.

### Phase 7 --- Power BI Data Modeling and Visualization

Connect Power BI to the SQL Data Warehouse and build a clear analytical
model.

Potential tasks:

-   Connect to the warehouse.
-   Configure relationships and the semantic model.
-   Create DAX measures and calculated fields where needed.
-   Design report pages around management questions.
-   Add useful filters and drill-down capabilities.
-   Ensure consistent metric definitions across visuals.
-   Validate Power BI totals against SQL results.

Potential dashboard pages:

1.  **Executive Overview**
    -   Total incidents.
    -   Overall response-time indicators, if reliably defined.
    -   Demand trends.
    -   Key management insights.
2.  **Demand Analysis**
    -   Incidents by hour, weekday, month, and year.
    -   Peak demand periods.
    -   Demand trends over time.
3.  **Geographical Analysis**
    -   Incident distribution by area.
    -   Highest-volume locations.
    -   Geographic comparisons, subject to location-data quality.
4.  **Response-Time Analysis**
    -   Response-time distribution.
    -   Response-time trends.
    -   Comparisons by time and location.
5.  **Demand vs. Response**
    -   Compare incident volume with response-time behavior.
    -   Highlight possible operational pressure periods.
    -   Avoid implying causation without supporting analysis.

The final number and design of dashboard pages should be based on the
strongest findings, not on filling a predetermined number of pages.

**Deliverables:** - Power BI report (`.pbix`). - Dashboard pages and
measures. - Validated visuals and metric definitions.

### Phase 8 --- Final Documentation and Presentation

-   Explain the business problem and stakeholder.
-   Describe the dataset and extraction scope.
-   Present the end-to-end architecture.
-   Explain cleaning decisions and warehouse design.
-   Show the most important analyses and dashboard findings.
-   Present recommendations supported by the analysis.
-   State data limitations, assumptions, and possible future
    improvements.

**Deliverables:** - Final project documentation. - Architecture and
warehouse diagrams. - Power BI report. - Final presentation. -
Reproducible code and SQL scripts.

------------------------------------------------------------------------

## 6. Team Members

  Name                  Role
  --------------------- -------------
  Mahmoud Abdelrahman   Team Leader
  Mahmoud Mohammed      Team Member
  Engy Ahmed            Team Member
  Omar Ibrahim          Team Member
  Pola Said             Team Member
  Ahmed Hamed           Team Member

------------------------------------------------------------------------

## 7. Work Division --- To Be Assigned

The following workstreams define the responsibilities that need to be
covered. **No person has been assigned to a workstream yet.**
Assignments will be decided by the team later.

  -----------------------------------------------------------------------
  Workstream              Main responsibilities   Assigned member(s)
  ----------------------- ----------------------- -----------------------
  Project Coordination &  Coordinate progress,    TBD
  Documentation           track tasks and         
                          milestones, maintain    
                          project documentation,  
                          and prepare the final   
                          presentation.           

  Data Extraction &       Work with the NYC Open  TBD
  Initial Exploration     Data API, extract the   
                          required data, verify   
                          completeness, inspect   
                          the schema, and         
                          document the dataset.   

  Data Cleaning &         Profile data quality,   TBD
  Preparation             handle missing and      
                          duplicate records,      
                          validate fields,        
                          prepare analytical      
                          columns, and document   
                          cleaning decisions.     

  Exploratory Data        Explore demand and      TBD
  Analysis & Business     response-time patterns, 
  Questions               define metrics,         
                          investigate trends, and 
                          identify meaningful     
                          findings.               

  SQL Data Warehouse      Design the warehouse    TBD
                          schema, create fact and 
                          dimension tables,       
                          implement staging and   
                          loading, and validate   
                          the warehouse.          

  SQL Analysis &          Develop SQL queries,    TBD
  Analytical Views        transformations,        
                          aggregations, and views 
                          that support the        
                          business analysis and   
                          Power BI reporting.     

  Power BI Data Modeling  Connect to the          TBD
  & DAX                   warehouse, build the    
                          semantic model, define  
                          measures, and validate  
                          calculations.           

  Power BI Dashboard &    Design report pages,    TBD
  Visualization           visuals, filters,       
                          drill-downs, and the    
                          executive overview.     

  Quality Assurance &     Check consistency       TBD
  Final Integration       across Python, SQL, and 
                          Power BI; validate      
                          metrics; review         
                          deliverables; and       
                          ensure the project      
                          tells one coherent      
                          story.                  
  -----------------------------------------------------------------------

### Notes on assigning work

-   Workstreams can be shared by more than one member.
-   A member may contribute to more than one workstream.
-   Some tasks depend on earlier tasks. For example, warehouse loading
    depends on the extraction and preparation process, while dashboard
    development depends on the warehouse model and agreed metric
    definitions.
-   The team leader coordinates integration and progress, but technical
    responsibilities should be distributed clearly.
-   Final ownership should be agreed upon by the team rather than
    assumed from the table.

------------------------------------------------------------------------

## 8. Suggested Key Metrics

The team should confirm the exact definitions and source fields before
implementing these metrics.

  -----------------------------------------------------------------------
  Metric                              Purpose
  ----------------------------------- -----------------------------------
  Total Incident Count                Measure overall EMS demand.

  Incidents by Hour                   Identify intraday demand peaks.

  Incidents by Day of Week            Identify recurring weekday
                                      patterns.

  Incidents by Month                  Understand longer-term and seasonal
                                      patterns.

  Incidents by Geographic Area        Identify high-demand locations.

  Average Response Time               Summarize response-time
                                      performance, if a valid
                                      response-time definition is
                                      available.

  Median Response Time                Provide a measure less affected by
                                      extreme values.

  Response-Time Percentiles           Understand the distribution and
                                      slower-response tail.

  Incidents During Peak Periods       Quantify workload during
                                      high-demand periods.

  Demand and Response-Time Comparison Investigate whether high-demand
                                      periods coincide with slower
                                      response times.
  -----------------------------------------------------------------------

**Metric caution:** Response time must be calculated or selected using
the correct timestamp fields and a documented definition. Do not assume
that any duration field in the dataset represents the full EMS response
time.

------------------------------------------------------------------------

## 9. Data Quality, Validation, and Governance

The team should maintain a clear record of:

-   Original source and dataset identifier.
-   Extraction date and time.
-   Exact date filter used.
-   Number of records extracted.
-   Number of records loaded into staging and warehouse tables.
-   Cleaning rules and records excluded.
-   Missing-value handling.
-   Duplicate-handling logic.
-   Definitions of response-time and other key metrics.
-   Known limitations in the source data.
-   Reconciliation results between Python, SQL, and Power BI.

The original raw export should remain unchanged. Cleaning and
transformations should be reproducible through code or SQL scripts.

------------------------------------------------------------------------

## 10. Suggested Project Folder Structure

``` text
Emergency_Medical_Services_Response_Analytics/
│
├── data/
│   ├── raw/
│   │   └── nyc_ems_2023_2026.csv
│   └── processed/
│       └── cleaned_ems_data/
│
├── python/
│   ├── data_extraction.ipynb
│   ├── data_exploration.ipynb
│   ├── data_cleaning.ipynb
│   └── exploratory_data_analysis.ipynb
│
├── sql/
│   ├── 01_create_database.sql
│   ├── 02_create_staging_tables.sql
│   ├── 03_create_dimensions.sql
│   ├── 04_create_fact_tables.sql
│   ├── 05_load_data.sql
│   ├── 06_transformations.sql
│   └── 07_analytical_views.sql
│
├── power_bi/
│   └── ems_response_analytics.pbix
│
├── documentation/
│   ├── data_dictionary.md
│   ├── data_cleaning_report.md
│   ├── warehouse_design.md
│   ├── analysis_findings.md
│   └── project_documentation.md
│
└── presentation/
    └── final_presentation/
```

This is a suggested structure. It can be adjusted to the team's actual
tools and workflow.

------------------------------------------------------------------------

## 11. Current Status and Next Steps

### Known progress

-   [x] Project theme selected: Emergency Medical Services Response
    Analytics.
-   [x] Primary stakeholder identified: NYC EMS / FDNY Management.
-   [x] Main technology flow selected: Python → SQL Data Warehouse →
    Power BI.
-   [x] NYC EMS Incident Dispatch Data selected.
-   [x] SODA3 filtered CSV export successfully returned HTTP 200.
-   [x] CSV file downloaded locally (reported size approximately 1.395
    GB).

### Still to do

-   [ ] Verify the exact number of rows in the downloaded CSV.
-   [ ] Confirm the extracted date boundaries and inspect the dataset
    schema.
-   [ ] Create a data dictionary and initial data profiling report.
-   [ ] Define cleaning rules and produce the prepared dataset.
-   [ ] Agree on the business questions and metric definitions.
-   [ ] Design and implement the SQL Data Warehouse.
-   [ ] Load the prepared data and validate warehouse totals.
-   [ ] Complete exploratory and business analysis.
-   [ ] Build the Power BI model, measures, and dashboards.
-   [ ] Assign team members to workstreams.
-   [ ] Complete documentation and final presentation.

------------------------------------------------------------------------

## 12. Project Success Criteria

The project should be considered successful when it delivers:

1.  A verified and documented dataset extract for the agreed period.
2.  Reproducible Python extraction, profiling, and cleaning work.
3.  A functioning SQL Data Warehouse with a documented schema and
    validated data.
4.  Analysis that answers meaningful EMS management questions.
5.  A Power BI report with accurate, understandable, and useful
    visualizations.
6.  Evidence-based findings and practical recommendations.
7.  Clear documentation of assumptions, metric definitions, and data
    limitations.
8.  A balanced team contribution and a coherent final presentation.

------------------------------------------------------------------------

**Working principle:** The objective is not simply to build a pipeline
or produce attractive charts. The objective is to turn EMS incident data
into reliable analysis and management-relevant insights.
