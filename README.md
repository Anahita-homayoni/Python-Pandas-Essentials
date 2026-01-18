# Python-Pandas-Essentials

Pandas is a Python library used for data analysis and data manipulation. It provides easy-to-use data structures like DataFrames that allow working with tables, CSV files, and datasets efficiently. Pandas is commonly used to clean, explore, and analyze data.

import pandas as pd from google.colab import files files.upload()

In Google Colab, you can import an Excel (.xlsx) file in a few easy ways. Here are the most common and beginner-friendly methods.

✅ Method 1: Upload from your computer (easiest)

Open your Google Colab notebook

Run this code:

from google.colab import files uploaded = files.upload()

Click Choose Files and select your .xlsx file

Load it with pandas:

import pandas as pd

df = pd.read_excel('your_file_name.xlsx') df.head()

✅ Method 2: Upload to Google Drive (recommended for big files) Step 1: Mount Google Drive from google.colab import drive drive.mount('/content/drive')

Authorize access.

Step 2: Read the Excel file import pandas as pd

df = pd.read_excel('/content/drive/MyDrive/your_file_name.xlsx') df.head()

📁 Make sure the file is inside MyDrive.

✅ Method 3: If the Excel has multiple sheets df = pd.read_excel('your_file_name.xlsx', sheet_name='Sheet1')

or

df = pd.read_excel('your_file_name.xlsx', sheet_name=0) # first sheet

🔍 Check sheet names pd.ExcelFile('your_file_name.xlsx').sheet_names


Selecting Columns in Pandas
import pandas as pd

oo = pd.read_csv(filename, skiprows=5)

# View random rows
oo.sample(3)

# Select a single column
oo["Discipline"]


🔹 Column names in Pandas are case-sensitive.
This means:

oo["Discipline"]   # ✅ works
oo["discipline"]   # ❌ KeyError
oo["DISCIPLINE"]   # ❌ KeyError


🔹 The column name must match exactly as it appears in the DataFrame.

To check column names:

oo.columns

Exploring Columns in Pandas
oo.Year.unique()


🔹 Returns all unique values in the Year column.
Useful to see which years are present in the dataset.

oo.Year.value_counts()


🔹 Counts how many times each year appears.
Useful for understanding how much data exists per year.

oo.Year.value_counts(normalize=True)


🔹 Shows the proportion (percentage) of each year instead of raw counts.
Helpful for comparing years relative to the total dataset.

oo.NOC.unique()


🔹 Returns all unique National Olympic Committee (NOC) codes in the dataset.
Useful to see how many countries participated.

Example Use Case

unique() → What values exist?

value_counts() → How many times does each value occur?

normalize=True → What percentage does each value represent?

Summary:

These methods help explore categorical data by identifying unique values, frequencies, and proportions.


Summary:

Pandas column selection is case-sensitive, so column names must be typed exactly as shown in the dataset.


Lists and Dictionaries in Python (Used with Pandas)
List Example
medals = ["Gold", "Silver", "Bronze"]
medals


🔹 A list stores multiple values in a single variable.
🔹 Lists are ordered and indexed (starting from 0).
🔹 Commonly used in Pandas to represent columns, labels, or categories.

Dictionary Example
position = {
    "First": "Gold",
    "Second": "Silver",
    "Third": "Bronze"
}

position


🔹 A dictionary stores data as key–value pairs.
🔹 Keys ("First", "Second", "Third") map to values ("Gold", "Silver", "Bronze").
🔹 Dictionaries are often used in Pandas for mapping and replacing values.

Dictionary Type
type(position)


🔹 Returns the data type of position → dict.

Accessing a Dictionary Value
position["First"]


🔹 Retrieves the value associated with the key "First".
🔹 Output: "Gold"

Summary

Lists store ordered collections of values.

Dictionaries store labeled (key–value) data.

Both are commonly used with Pandas for data transformation and mapping.
