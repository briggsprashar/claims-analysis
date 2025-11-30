
# Healthcare Claims Data Analysis

## Project

<details>

<summary> Project details</summary>

<br />

This Python-Pandas based analysis that simulates work by healthcare data analysts, medical billing specialists, and health informaticians and involves understanding claims data. This analysis is based on data from May 2024.

Domains highlighted:
- Revenue cycle management (cost centers and revenues)
- Coding accuracy audits
- Payer contract analysis
- Clinical quality reporting
- Healthcare operations optimization

Data concepts incorporated:
- Relational healthcare data
- Claims data structure (HEADER, LINE, CODE files)
- Joins and aggregations
- Provider billing patterns
- Diagnosis-procedure combinations

Concepts supporting operational decision-making:
- Billing patterns
- Most common diagnoses and procedures 
- Payer-mix 

</details>

<br />

<details>

<summary> Data Source </summary>

<br />

Dataset consists of: 
- 3 CSV files that form a relational database structure

### 1. HEADER File (`STONYBRK_20240531_HEADER.csv`)
Contains one row per claim with high-level information:
- **ProspectiveClaimId**: Unique claim identifier
- **Provider NPIs**: Billing, Attending, Rendering, Referring, Operating providers
- **ServiceFromDate/ServiceToDate**: Service dates
- **PrimaryPayerName/PrimaryPayerCode**: Primary insurance payer
- **PlaceOfService**: Where services were rendered
- **WorkQueName**: Audit/review queue assignment

### 2. LINE File (`STONYBRK_20240531_LINE.csv`)
Contains one row per service line (procedures/services):
- **ProspectiveClaimId**: Links to HEADER file
- **LinePos**: Line number on the claim
- **HCPCS**: CPT/HCPCS procedure code
- **Modifier1-4**: Procedure modifiers
- **DxMapDelim**: Shows which diagnosis codes justify this procedure
- **Charges**: Dollar amount billed for this line
- **Units**: Quantity of service

### 3. CODE File (`STONYBRK_20240531_CODE.csv`)
Contains diagnosis codes (ICD-10):
- **ProspectiveClaimId**: Links to HEADER file
- **CodePos**: Position/order of diagnosis on claim
- **CodeValue**: ICD-10 diagnosis code
- **CodeQualifier**: Code type (ABF, ABK, etc.)

</details>

## Project steps

<details>
<summary> Part 1: Data Loading and Exploration </summary>

<br />

In Google Colab, load the .ipynb notebook, upload the 3 csv files, install required libraries and dependencies. The following are part of the code that helps analyze the dataset.

**Load the three CSV files**
   - Use appropriate variable names for dataframes: `df_header`, `df_line`, `df_code`

**Explore each file** 

   - Shape (number of rows and columns)

         HEADER SHAPE: (388, 43)
         CODE SHAPE: (1536, 9)
         LINE SHAPE: (520, 28)

   - First 5 rows (In .ipynb file)
   - Column names and data types (In .ipynb file)
   - Explore files with `head`, `iloc` etc. to see structure, columns names, etc. (In .ipynb file)
   - Missing value counts (In .ipynb file)
   - Basic descriptive statistics for numeric columns; not useful in most cases. (In .ipynb file)

**Analysis**

   - Unique claims in the dataset

         Unique claims in HEADER: 388
         Unique claims in CODE: 388
         Unique claims in LINE: 388

   - Date range of the claims

         Service Date Range from: 2023-09-25 to 2024-05-29
   
   - Avg number of service lines per claim 

         Average number of service lines per claim: 1.34
   
   - Avg number of diagnosis codes per claim 
   
         Average diagnosis codes per claim: 3.96

</details>

<br />

<details>
<summary>Part 2: Relational Data Analysis </summary>

<br />

Uses Pandas operations (merge, groupby, value_counts, etc.):

**1: Provider Analysis**

*Results below are copied from the .ipynb file (instead of images)*

- Top 5 billing providers by number of claims (In .ipynb file)
- Provider name, NPI, and claim count (In .ipynb file)
- Bar chart visualization of top 5 billing providers (In .ipynb file)

**2: Payer Mix Analysis**
See Section 2 in .ipynb file
*Results below are copied from the .ipynb file (instead of images)*

- Top 5 primary payers by claim volume (In .ipynb file)
- % of total claims for each payer (In .ipynb file)
- Bar chart visualization showing payer distribution (In .ipynb file)

**3: Common Diagnoses - ICD**
See Section 3 in .ipynb file
- Top 10 most frequently appearing diagnosis codes 
- ICD-10 code and frequency count
- Mapped to ICD 10 descriptions

**4: Common Procedures - HCPCS**
See Section 4 in .ipynb file
- Top 10 most frequently billed procedure codes (HCPCS)
- HCPCS code, description and frequency
- Bar chart visualization showing top 10 procedures

**5: Service Location Analysis**
See Section 5 in .ipynb file
- Number of claims submitted for each PlaceOfService
- % of claims for "INPATIENT" vs "DOCTOR'S OFFICE"?

</details>

<br />

<details>
<summary>Part 3: Advanced Analysis with Joins  </summary>

<br />

**6: Claims with High Service Line Counts**
- (Merge the HEADER and LINE files)
- Total number of service lines per claim
- Claims with 5 or more service lines
- ClaimId with Distribution by Provider name, number of lines, and total charges

**7: Diagnosis-Procedure Combinations**
- Merged dataset linking claims to both procedures and diagnoses
- Most common diagnosis code (CodeValue) associated with CPT code 99291 (Merge the 3 files)

**8: Charges by Payer**
- Merge HEADER and LINE files
- Total charges (sum of all line charges) per claim
- Group by PrimaryPayerName and calculate:
  - Total charges
  - Average charges per claim
  - Number of claims
- Total charges descending and display top 10 payers

</details>

<br />

<details>
<summary>Part 4: Claims & Charges </summary>

<br />

**9: Analysis**
- Total Charges vs Number of Service Lines per Claim by Provider
- Average Total Charges by Number of Service Lines per Claim and Billing Provider
- Top 10 Average Total Charges by Num Service Lines and Provider
- Top 10 providers with most service lines
- Top 20 service lines with most charges
- Top 10 Unique Service Lines with Most Charges
- Top 10 providers billing for the most complex cases (highest number of diagnosis codes)
- Place of service and average charges distribution

</details>

## Summary of key findings

- Max Claims

   - Billing Provider with max claims `SB INTERNISTS - 152`

   - Primarypayer with max claims `MEDICARE - 242`

   - ICD codes with max claims `J96.01 - Acute respiratory failure with hypoxia - 62`  

   - HCPCS codes with max claims  `99291 - CRITICAL CARE, INITIAL FIRST HOUR - 68`

   - Place of Service with max claims `11 - DOCTOR'S OFFICE - 132` 

   - Facility location with max claims `231 - INPATIENT - 59.5361`   *Shud have rounded to 59 in the code*

- Top provider charges `SB INTERNISTS - $87560`

- Avg charge by place of service  `22 - Outpatient Hospital - $678.60`  *Shud have rounded to 678 in the code*

## FYI

1. **Multiple rows per claim**: LINE and CODE files had multiple rows per ProspectiveClaimId
2. **Aggregation**: Code does not double-counting (I think!)
3. **Data types**: Converted date to datetime format (if needed)
