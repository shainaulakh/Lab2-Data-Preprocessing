# Lab 2 - Data Preprocessing

## Student Information

- **Name:** Sukhchain Singh
- **Student Number:** 9111541
- **Course:** PROG8245 - Machine Learning Programming

## Overview

This project was completed for Lab 2 - Tidying, Cleaning, Imputation, and Outlier Detection. The purpose of the lab is to practice preparing real-world datasets for data analysis and machine learning.

The notebook contains four sections using the PEW, Billboard, Auto MPG Cars, and Pima Indians Diabetes datasets. The lab covers data tidying, string cleaning, missing-value handling, imputation, duplicate detection, visualization, and outlier detection.

## Topics Covered

### 1. PEW Dataset - Data Tidying

The PEW dataset was originally stored in wide format, with income categories represented as separate columns.

The main tasks included:

- inspecting the dataset structure;
- identifying identifier and measured variables;
- using `melt()` to convert the dataset from wide to long format;
- grouping and analyzing income categories;
- creating a visualization from the tidy data.

### 2. Billboard Dataset - Tidying and String Cleaning

The Billboard dataset contained weekly rankings stored across many columns such as `1st week`, `2nd week`, and later weeks.

The main tasks included:

- reshaping weekly ranking columns using `melt()`;
- cleaning week labels using Pandas string operations;
- converting week numbers and rankings to numeric values;
- identifying and handling missing ranking values;
- calculating approximate chart dates;
- visualizing a song's Billboard ranking over time.

### 3. Cars Dataset - Data Cleaning and Imputation

The Auto MPG dataset was used to study missing-data handling and cleaning strategies.

The main tasks included:

- inspecting data types and missing values;
- checking for invalid and duplicate records;
- comparing row and column deletion using `dropna()`;
- demonstrating mean, median, and mode imputation;
- using visualization to support an imputation decision;
- applying Scikit-Learn's `SimpleImputer`;
- comparing the dataset before and after cleaning.

The `Horsepower` column contained missing values. Median imputation was used in the final cleaned copy because it is less sensitive to unusually high or low values than the mean.

## Section 4 - CDC Diabetes Health Indicators

In Section 4, I updated the diabetes analysis using the **CDC Diabetes Health Indicators** dataset from the UCI Machine Learning Repository.

Instead of loading only a local CSV file, I accessed the dataset programmatically using the `ucimlrepo` package and dataset ID `891`. This made the data source easier to reproduce and allowed the notebook to retrieve the dataset directly from UCI.

### Data Quality Checks

Before performing outlier analysis, I checked:

- Dataset shape and column names
- Data types
- Missing values
- Duplicate rows
- Descriptive statistics

These checks helped me understand the structure and quality of the dataset before making any changes.

### Exploratory Analysis

I used visualizations such as:

- Box plots
- Scatter plots
- Histograms

These visualizations helped identify unusual BMI values and understand the relationship between BMI, physical health, and diabetes status.

### Outlier Detection

I used two statistical methods to detect BMI outliers:

- **Z-score**
- **Interquartile Range (IQR)**

The Z-score method identified **2,963** extreme BMI observations.

The IQR method identified **9,847** possible outliers.

All Z-score outliers were also identified by the IQR method. Because the IQR method was more aggressive, I used the Z-score method for treatment so that fewer potentially valid observations were removed.

After treatment:

- Rows decreased from **253,680** to **250,717**
- Mean BMI changed from approximately **28.38** to **28.02**
- BMI standard deviation decreased from approximately **6.61** to **5.61**

This showed that the extreme values were reduced without greatly changing the overall center of the data.

### Feature Engineering

I created a new feature called:

`Health_Risk_Count`

This feature combines the following binary health indicators:

- High blood pressure
- High cholesterol
- Smoking
- Stroke
- Heart disease or heart attack

The feature represents how many of these selected risk factors are present for each person.

### Health Risk and Diabetes

I compared `Health_Risk_Count` with diabetes status.

The analysis showed that the percentage of people with diabetes increased as the number of selected health risk factors increased.

People with no selected risk factors had the lowest diabetes percentage, while people with more selected risk factors had a much higher diabetes percentage.

This analysis shows an association between the selected health risk factors and diabetes, but it does not prove that the risk factors directly cause diabetes.

### What I Learned

This section helped me understand that outlier detection is not only about removing unusual values.

Different methods can identify different numbers of outliers, so it is important to compare the results and explain why a treatment method was selected.

I also learned how feature engineering can transform multiple existing variables into a new feature that makes patterns in the data easier to understand.

## Project Structure

```text
Lab2 - Data PreProcessing/
│
├── week5Lab.ipynb
├── README.md
├── requirements.txt
├── .gitignore
│
└── data/
    ├── pew-raw.csv
    ├── billboard.csv
    ├── auto-mpg.data
    └── diabetes.csv
```

## Datasets

The following datasets are used in this project:

- **PEW Dataset** - used for wide-to-long data tidying.
- **Billboard Dataset** - used for reshaping, string manipulation, missing-value handling, and trend analysis.
- **Auto MPG Dataset** - used for missing-data analysis, imputation, and data cleaning.
- **Pima Indians Diabetes Dataset** - used for visual and statistical outlier detection.

All datasets are stored inside the `data/` folder and are loaded using relative paths.

## Python Libraries Used

The project uses the following main Python libraries:

- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-Learn
- Jupyter
- `ucimlrepo` - used to retrieve the CDC Diabetes Health Indicators dataset directly from UCI

The project was completed using Python 3.11.

Exact package versions are listed in `requirements.txt`.

## How to Run the Project

1. Download or clone the project repository.

2. Open a terminal in the project folder.

3. Create or activate a Python environment.

4. Install the required packages using:

    ```bash
    python -m pip install -r requirements.txt
    ```

5. Open:

    ```text
    week5Lab.ipynb
    ```

6. Select the appropriate Python kernel.

7. Restart the kernel.

8. Run all notebook cells from top to bottom.

All datasets are loaded using relative paths such as:

```python
pd.read_csv("./data/diabetes.csv")
```

This allows the notebook to run on another computer without requiring paths specific to the original machine.

## Replicability Check

The notebook was tested by restarting the kernel and running all cells from top to bottom using **Run All**.

The notebook completed successfully with the required datasets located in the `data/` folder.

During environment setup, a standard Python virtual environment encountered a Windows Application Control issue while importing Pandas. To avoid changing system security settings, a Conda environment using Python 3.11 was created instead. The required libraries worked successfully in the Conda environment.
I tested the project in a separate clean Conda environment using Python 3.11.
During the first test, the original `requirements.txt` created with `pip freeze` contained local Conda build paths such as `file:///C:/...`. These paths were not portable and caused package installation to fail in the new environment.
I fixed this by regenerating `requirements.txt` using:

`python -m pip list --format=freeze > requirements.txt`

This produced portable `package==version` entries.

I then created a new `lab2_test` environment, installed the dependencies from `requirements.txt`, registered the environment as a Jupyter kernel, and executed the complete notebook from top to bottom using `nbconvert`.

The final replicability test completed successfully. This confirmed that the notebook can run in a clean environment using the submitted dependencies and relative dataset paths.

The project uses:

- relative dataset paths;
- a `requirements.txt` file for dependencies;
- a `.gitignore` file for unnecessary local files;
- no unnecessary `!pip install` commands inside the notebook.

## Data Cleaning Decisions

Important cleaning decisions were documented instead of automatically deleting or replacing data.

Examples include:

- missing Billboard rankings were removed instead of being artificially imputed;
- Cars `Horsepower` missing values were compared using several imputation approaches;
- `SimpleImputer` was used to demonstrate a reproducible machine-learning preprocessing approach;
- zero values in some Diabetes physiological measurements were identified as possible data-quality issues but were not automatically deleted;
- potential outliers were investigated using visual and statistical methods before treatment.

## Key Learning Outcomes

This lab demonstrated how preprocessing decisions can affect later analysis and machine-learning results.

The main skills practiced were:

- reshaping wide data into tidy long format;
- cleaning strings and converting data types;
- detecting missing and invalid values;
- comparing missing-value treatment strategies;
- using mean, median, and mode imputation;
- using Scikit-Learn preprocessing tools;
- identifying duplicate records;
- visualizing distributions and relationships;
- detecting potential outliers using box plots, Z-Scores, and IQR;
- comparing data before and after cleaning;
- maintaining reproducible project structure and dependencies.

## Validation

Before final submission, the notebook was:

- saved;
- restarted;
- executed using **Run All**;
- checked for errors;
- checked for relative file paths;
- checked for required visualizations and Markdown explanations.

The final notebook contains all four required sections and their corresponding analysis and reflections.
