Data Cleaning Project: Data Scientist Job Postings     
Objective: The primary goal of this project was to take a raw, messy dataset of data science job postings and transform it into a clean, structured format ready for analysis. This project demonstrates a typical data wrangling and preparation workflow.
Dataset:
The original dataset used is Uncleaned_DS_jobs.csv, which contains various details about job listings, including salary estimates, company information, and job descriptions.

**Tools and Libraries:**
Language: Python
Libraries: Pandas, NumPy
Environment: Jupyter Notebook

**Key Cleaning Steps Performed:**
Parsed and converted salary estimates from text ranges to numerical columns (min, max, and average).
Cleaned company names by removing embedded ratings.
Extracted the job's state from the location column.
Handled and standardized missing or invalid values in columns like 'Founded', 'Size', and 'Revenue'.
Created a final, structured dataset with selected columns for clarity.

**Outcome:**
The output of this project is a clean and ready-to-use file, Cleaned_DS_jobs.csv, which can now be used for exploratory data analysis, visualization, or machine learning tasks. The entire cleaning process is documented in the ds_jobs.ipynb notebook.
