# data_analytics_intern-veda-technology-
# Data Cleaning and Preprocessing – Titanic Dataset

## Project Overview

This project was completed as part of the Veda Technology Data Analytics Track – Task 1.

The objective of this task is to clean and preprocess a raw dataset by identifying and handling common data quality issues such as missing values, duplicate records, inconsistent text formatting, incorrect data types, invalid numerical values, and potential outliers.

The Titanic dataset was selected for this task because it contains missing values and provides a suitable dataset for practicing data cleaning and preprocessing techniques.

---

## Objective

The main objectives of this project are:

* Identify data quality issues in the raw Titanic dataset.
* Analyze missing values and duplicate records.
* Standardize text-based columns.
* Handle missing values appropriately.
* Validate numerical data types and values.
* Identify potential outliers.
* Perform final data quality checks.
* Export the cleaned dataset for further analysis.

---

## Dataset

**Dataset:** Titanic Dataset

The dataset contains **891 rows and 12 columns**.

### Columns

* PassengerId
* Survived
* Pclass
* Name
* Sex
* Age
* SibSp
* Parch
* Ticket
* Fare
* Cabin
* Embarked

---

## Tools and Technologies

* Python
* Pandas
* NumPy
* Google Colab
* CSV

---

## Data Inspection

The dataset was loaded using Pandas and initially inspected using:

```python
df.head()
df.tail()
df.shape
df.columns
df.info()
```

The dataset contains 891 records and 12 columns.

The data types were also examined to identify numerical and categorical columns.

---

## Data Quality Issues Identified

The initial inspection identified missing values in the following columns:

| Column        | Missing Values |
| ------------- | -------------: |
| Age           |            177 |
| Cabin         |            687 |
| Embarked      |              2 |
| Other columns |              0 |

The total number of missing values was **866**.

Duplicate records were also checked using:

```python
df.duplicated().sum()
```

The dataset contained **0 duplicate rows**.

---

## Data Cleaning Process

### 1. Text Cleaning

Whitespace was removed from text-based columns to maintain consistent formatting.

```python
text_columns = df.select_dtypes(include="object").columns

for col in text_columns:
    df[col] = df[col].apply(
        lambda x: x.strip() if isinstance(x, str) else x
    )
```

### 2. Standardizing Sex

The `Sex` column was converted to lowercase.

```python
df["Sex"] = df["Sex"].str.lower()
```

The resulting categories were:

* male
* female

### 3. Standardizing Embarked

The `Embarked` column was converted to uppercase.

```python
df["Embarked"] = df["Embarked"].str.upper()
```

The available categories were:

* S
* C
* Q

### 4. Handling Missing Age Values

There were 177 missing values in the `Age` column.

The median age was calculated as **28.0** and used to fill the missing values.

```python
age_median = df["Age"].median()
df["Age"] = df["Age"].fillna(age_median)
```

This preserved the existing records instead of removing rows containing missing age values.

### 5. Handling Missing Embarked Values

There were 2 missing values in the `Embarked` column.

The mode was calculated as **S** and used to fill the missing values.

```python
embarked_mode = df["Embarked"].mode()[0]
df["Embarked"] = df["Embarked"].fillna(embarked_mode)
```

### 6. Handling Missing Cabin Values

There were 687 missing values in the `Cabin` column.

Instead of deleting a large number of records, missing cabin values were replaced with:

```text
Unknown
```

using:

```python
df["Cabin"] = df["Cabin"].fillna("Unknown")
```

This preserves the records while clearly indicating that cabin information was unavailable.

### 7. Duplicate Records

Duplicate rows were checked and removed if present.

```python
df = df.drop_duplicates()
```

The dataset contained **0 duplicate rows**, so no records were removed.

### 8. Data Type Validation

The data types were inspected using:

```python
df.dtypes
```

The `Age` and `Fare` columns were explicitly validated as numeric:

```python
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")
df["Fare"] = pd.to_numeric(df["Fare"], errors="coerce")
```

### 9. Invalid Numerical Values

Negative values were checked for `Age` and `Fare`.

```python
print("Negative Age values:", (df["Age"] < 0).sum())
print("Negative Fare values:", (df["Fare"] < 0).sum())
```

The result was:

```text
Negative Age values: 0
Negative Fare values: 0
```

Therefore, no negative age or fare values were found.

---

## Outlier Detection

Potential outliers in the `Fare` column were identified using the Interquartile Range (IQR) method.

```python
Q1 = df["Fare"].quantile(0.25)
Q3 = df["Fare"].quantile(0.75)

IQR = Q3 - Q1

lower_limit = Q1 - 1.5 * IQR
upper_limit = Q3 + 1.5 * IQR

outliers = df[
    (df["Fare"] < lower_limit) |
    (df["Fare"] > upper_limit)
]
```

The analysis identified **116 potential Fare outliers**.

These values were identified and reviewed rather than automatically removed, because a high fare value can represent a genuine observation.

---

## Final Data Quality Check

After preprocessing, the following checks were performed:

* Dataset shape
* Missing values
* Duplicate rows
* Data types
* Sample records

Final dataset shape:

```text
(891, 12)
```

Final missing-value check:

```text
All columns: 0 missing values
```

Final duplicate check:

```text
0 duplicate rows
```

The cleaned dataset therefore contains **891 rows and 12 columns**, with no remaining missing values or duplicate rows.

---

## Change Log

| Issue             | Column       | Action Taken                   | Reason                                                                   |
| ----------------- | ------------ | ------------------------------ | ------------------------------------------------------------------------ |
| Missing values    | Age          | Median imputation              | Preserves records while using the median value                           |
| Missing values    | Embarked     | Mode imputation                | Suitable for a small number of missing categorical values                |
| Missing values    | Cabin        | Replaced with `Unknown`        | Preserves records and represents unavailable information                 |
| Duplicate records | All columns  | Checked and removed if present | Prevents repeated records                                                |
| Text formatting   | Text columns | Removed unnecessary whitespace | Improves consistency                                                     |
| Text formatting   | Sex          | Converted to lowercase         | Standardizes categories                                                  |
| Text formatting   | Embarked     | Converted to uppercase         | Standardizes categories                                                  |
| Data types        | Age, Fare    | Validated as numeric           | Supports reliable numerical analysis                                     |
| Invalid values    | Age, Fare    | Checked for negative values    | Ensures valid numerical values                                           |
| Outliers          | Fare         | Identified using IQR           | Detects unusual values without automatically deleting valid observations |

---

## Deliverables

The project contains:

* `TASK1.ipynb` – Google Colab notebook containing the complete data cleaning process.
* `titanic_cleaned.csv` – Cleaned Titanic dataset.
* `README.md` – Project description, methodology, cleaning decisions, and results.

---

## Conclusion

The Titanic dataset was successfully inspected and preprocessed using Python and Pandas in Google Colab.

The cleaning process addressed missing values, duplicate records, text formatting, data type validation, invalid numerical values, and potential outliers. Missing values in `Age`, `Cabin`, and `Embarked` were handled using appropriate methods while preserving the dataset records.

After preprocessing, final quality checks confirmed that the cleaned dataset contains **891 rows and 12 columns with no remaining missing values or duplicate records**.

The resulting cleaned dataset is ready for further data analysis.
