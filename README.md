**Data Cleaning Project:** Data Scientist Job Postings     
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

**Step 1: Import Libraries and Load the Data**
This code imports the pandas library and loads your CSV file into a DataFrame called df.
import pandas as pd
import numpy as np

# Load the dataset from your local machine
df = pd.read_csv('Uncleaned_DS_jobs.csv')

# Display the first 5 rows to confirm it's loaded correctly
print("Original Data Head:")
print(df.head())

# Get an initial overview of the data
print("\nOriginal Data Info:")
df.info()

**Step 2: Cleaning the 'Salary Estimate' Column**
This is often the most complex part. We will parse the text to create numerical salary columns.

# Remove rows where salary is '-1' (or not specified)
df = df[df['Salary Estimate'] != '-1']

# Clean the 'Salary Estimate' column text
# Remove '(Glassdoor est.)' and 'K' and '$' symbols
salary = df['Salary Estimate'].str.split('(').str[0]
salary = salary.str.replace('K', '').str.replace('$', '')

# Split the range into 'min_salary' and 'max_salary'
df['min_salary'] = salary.apply(lambda x: int(x.split('-')[0]))
df['max_salary'] = salary.apply(lambda x: int(x.split('-')[1]))

# Create an 'avg_salary' column (in thousands)
df['avg_salary'] = (df['min_salary'] + df['max_salary']) / 2

print("\nSalary columns cleaned and created.")

**Step 3: Cleaning the 'Company Name' Column**
The company name sometimes includes the rating. We'll remove it to get a clean name.

# Remove the rating and newline characters from the Company Name
df['company_cleaned'] = df['Company Name'].apply(lambda x: x.split('\n')[0])

print("Company Name cleaned.")

**Step 4: Parsing State from the 'Location' Column**
Extracting the state can be very useful for location-based analysis.

# Create a 'job_state' column by splitting the location string
df['job_state'] = df['Location'].apply(lambda x: x.split(',')[1].strip() if ',' in x else x.strip())

# Check the count of jobs per state
print("\nJobs per state:")
print(df.job_state.value_counts().head(10))

**Step 5: Cleaning 'Founded' and 'Size' Columns**
We'll fix invalid values in the 'Founded' column and simplify the 'Size' column.

# Replace invalid 'Founded' year (-1) with NaN (Not a Number)
df['Founded'] = df['Founded'].replace(-1, np.nan)

# Simplify the 'Size' column by removing " employees"
df['Size'] = df['Size'].str.replace(' employees', '')

print("\nFounded and Size columns cleaned.")

**Step 6: Cleaning the 'Revenue' Column**
This column has text like 'Unknown / Non-Applicable' which we will clean up.

# Simplify the 'Revenue' column
df['Revenue'] = df['Revenue'].str.replace(' (USD)', '', regex=False)
df['Revenue'] = df['Revenue'].replace('Unknown / Non-Applicable', np.nan)

print("Revenue column cleaned.")

**Step 7: Final Review and Saving the Cleaned Data**
Let's look at our new, cleaned DataFrame and save it to a new file.

# Select and reorder the most important columns for a cleaner view
df_cleaned = df[['Job Title', 'company_cleaned', 'Location', 'job_state', 
                 'min_salary', 'max_salary', 'avg_salary', 
                 'Size', 'Founded', 'Type of ownership', 'Industry', 
                 'Sector', 'Revenue', 'Job Description']]

print("\nHead of the Final Cleaned DataFrame:")
print(df_cleaned.head())

print("\nInfo of the Final Cleaned DataFrame:")
df_cleaned.info()

# Save the cleaned dataframe to a new CSV file
df_cleaned.to_csv('Cleaned_DS_jobs.csv', index=False)

print("\nSuccessfully cleaned the data and saved it to 'Cleaned_DS_jobs.csv'")
