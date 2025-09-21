# Data Cleaning and Preprocessing Report

## Overview

This report outlines the data cleaning and preprocessing steps applied to the student dataset (`bi.csv`). The dataset contains student information, including demographic and academic performance metrics. The objective was to ensure data consistency, handle missing values, address outliers, and perform feature engineering to prepare the dataset for reliable analysis and machine learning tasks.

## Data Cleaning

### Dataset Structure

- **Shape**: The dataset originally had 77 rows and 11 columns.
- **Data Types**: Included objects (categorical) and numerical (int64, float64).

### Inconsistencies Found and Fixed

1. **Gender Column**

   - **Inconsistencies**: Values included 'Female', 'M', 'Male', 'F', 'female', 'male'.
   - **Fix**: Standardized to 'F' and 'M' using `.replace()`.

2. **Country Column**

   - **Inconsistencies**: Variations like 'norway', 'Norge', and 'Rsa' instead of 'Norway' and 'South Africa'.
   - **Fix**: Replaced 'norway' and 'Norge' with 'Norway', and 'Rsa' with 'South Africa'.

3. **Previous Education Column**

   - **Inconsistencies**: Varied spellings such as 'Diplomaaa', 'diploma', 'DIPLOMA', 'High School', and 'Barrrchelors'.
   - **Fix**: Standardized to 'Diploma', 'HighSchool', and 'Bachelors'.

4. **Residence Column**
   - **Inconsistencies**: Variations like 'BI-Residence', 'BI_Residence', 'BIResidence'.
   - **Fix**: Standardized to 'BI Residence' using `.replace()`.

### Duplicates

- **Check**: No duplicates were found (`df.duplicated().sum()` returned 0).

## Handling Missing Data

### Missing Values Identified

- **Python Column**: 2 missing values.
- **Other Columns**: No missing values.

### Imputation Strategy

- **Method**: Median imputation for the Python column.
- **Reason**: The median is robust to outliers and maintains the central tendency of the score distribution.

## Outlier Detection and Handling

### Outlier Detection

- **Method**: Used boxplots and summary statistics to identify outliers in `studyHOURS`.
- **Findings**: Unrealistically low values (e.g., 114, 120, 122) compared to the median (156).

### Outlier Handling

- **Method**: Applied the IQR method via `datasist.structdata.detect_outliers()`.
- **Action**: Replaced outliers in `studyHOURS` with the median (156).

## Feature Engineering

### New Features Created

1. **Programming_Avg**

   - **Calculation**: Average of `Python` and `DB` scores.
   - **Reason**: To consolidate programming proficiency into a single metric for predictive modeling.

2. **isAdult**

   - **Calculation**: Binary indicator (1 if `Age` >= 18, else 0).
   - **Reason**: To identify adult status for potential demographic analysis, though most students are adults.

3. **studyHOURS_cat**
   - **Calculation**: Binned `studyHOURS` into 'Low', 'Medium', and 'High' categories using quantiles or custom thresholds.
   - **Reason**: To transform continuous study hours into categorical groups for interpretive analysis or non-linear modeling.

### Encoding Categorical Variables

1. **Ordinal Encoding for prevEducation**

   - **Mapping**: 'HighSchool' → 1, 'Diploma' → 2, 'Bachelors' → 3, 'Masters' → 4, 'Doctorate' → 5.
   - **Reason**: To convert ordinal education levels into numerical values suitable for machine learning.

2. **One-Hot Encoding**
   - **Applied to**: `gender` (resulting in `gender_0`, `gender_1`), `residence` (`residence_0`, `residence_1`), `country` (`country_0`, `country_1`, `country_2`, `country_3`; likely after grouping countries into 4 categories for dimensionality reduction).
   - **Reason**: To transform nominal categorical variables into binary vectors, avoiding ordinal assumptions.
   - **Dropped Columns**: Original categorical columns (`gender`, `residence`, `country`) and non-relevant columns (`fNAME`, `lNAME`) were removed after encoding.

### Feature Scaling

- **Method**: Min-Max scaling applied to numerical features (`Age`, `entryEXAM`, `studyHOURS`, `Python`, `DB`, `Programming_Avg`) to normalize to a [0, 1] range.
- **Reason**: To ensure equal contribution of features in distance-based machine learning algorithms and improve model convergence.

## Deliverables

### Cleaned and Preprocessed Dataset

- **Intermediate Output**: Cleaned dataset (post-cleaning, imputation, and outlier handling) saved as `cleaned_data.csv`.
- **Final Output**: Preprocessed dataset with 77 rows and 17 columns, including engineered features, encoded categoricals, and scaled numerical values.
- **Characteristics**: Standardized categories, no missing values, mitigated outliers, and fully prepared for analysis.

### Key Changes Summary

1. **Standardized Categories**: Gender, country, residence, and education levels are now consistent.
2. **Missing Values**: Imputed Python scores using the median.
3. **Outliers**: Replaced extreme `studyHOURS` values with the median.
4. **Feature Engineering**: Added `Programming_Avg`, `isAdult`, `studyHOURS_cat`; encoded categorical variables; scaled numerical features.
5. **Dropped Columns**: Removed `fNAME`, `lNAME`, and original categorical columns after encoding.

The preprocessed dataset is ready for exploratory data analysis (EDA), visualization, and machine learning tasks such as classification or regression.

## Tools Used

- **Python Libraries**: pandas (data manipulation), seaborn (visualization), datasist (outlier detection), scikit-learn (encoding and scaling).
- **Visualization**: Boxplots for outlier detection.
- **Imputation**: Median for numerical missing values.
- **Encoding and Scaling**: Ordinal mapping, one-hot encoding, and Min-Max scaling for feature preparation.
