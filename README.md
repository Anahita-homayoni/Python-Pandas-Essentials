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

Rename a Series (or Column) in Pandas

This tutorial shows multiple ways to rename columns in a Pandas DataFrame using the Olympic dataset.

1️⃣ Load the dataset
import pandas as pd

filename = "olympics_1896_2004.csv"
oo = pd.read_csv(filename, skiprows=5)

oo.sample(3)


skiprows=5 skips metadata rows so Pandas reads the correct header.

2️⃣ Rename columns using a dictionary (recommended)
mapper = {
    "Athlete Name": "Athlete_Name",
    "Event Gender": "Event_Gender"
}

oo = oo.rename(columns=mapper)
oo.sample(3)


🔹 Uses a dictionary where:

keys = old column names

values = new column names

🔹 This method is clear and flexible.
3️⃣ Rename columns inline (one-liner)
oo = oo.rename(columns={
    "Athlete Name": "Athlete_Name",
    "Event Gender": "Event_Gender"
})


🔹 Same result, written more compactly.

4️⃣ Rename all columns at once
column_names = [
    'Year', 'City', 'Sport', 'Discipline', 'Athlete_Name',
    'NOC', 'Gender', 'Event', 'Event_Gender', 'Medal', 'Position'





    Difference Between DataFrame and Dictionary
Dictionary (Python)

A dictionary is a basic Python data structure that stores data as key–value pairs.

medals = {
    "Gold": 10,
    "Silver": 8,
    "Bronze": 6
}


🔹 Data is unstructured
🔹 Each key maps to one value
🔹 No rows or columns
🔹 Used for simple lookups and mappings

DataFrame (Pandas)

A DataFrame is a tabular data structure from the Pandas library, similar to an Excel table or SQL table.

import pandas as pd

df = pd.DataFrame({
    "Medal": ["Gold", "Silver", "Bronze"],
    "Count": [10, 8, 6]
})


🔹 Data is organized in rows and columns
🔹 Supports powerful operations (filtering, grouping, aggregation)
🔹 Designed for data analysis
🔹 Can be created from dictionaries, CSV, or Excel files

Key Differences
Feature	Dictionary	DataFrame
Type	Python built-in	Pandas object
Structure	Key–value pairs	Rows & columns
Data size	Small/simple	Large & complex
Data analysis	❌ Limited	✅ Powerful
Similar to	JSON	Excel / SQL table
Relationship Between Them

A DataFrame can be created from a dictionary, but a dictionary cannot behave like a DataFrame.

Summary

Use dictionaries for simple data storage and mapping

Use DataFrames for structured data analysis and manipulation





]

oo.columns = column_names
oo.sample(3)


🔹 Useful when you want full control over column names.
⚠️ The number of names must match the number of columns.

5️⃣ Set column names while reading the file
oo = pd.read_csv(filename, skiprows=5, names=column_names)
oo.head()


🔹 Assigns column names during import.
6️⃣ Use custom names but keep the header row
oo = pd.read_csv(
    filename,
    skiprows=5,
    names=column_names,
    header=0
)
oo.head()


🔹 header=0 tells Pandas to ignore the original header row.

Summary

rename(columns=...) → best for renaming specific columns

df.columns = [...] → rename all columns at once

names= in read_csv() → rename columns while loading data

Renaming columns improves readability and makes data analysis easier.



Remove a Series (Column) or Row in Pandas

This section explains how to remove columns and rows from a Pandas DataFrame using the Olympic dataset.

1️⃣ Load the dataset
import pandas as pd

filename = "olympics_1896_2004.csv"
oo = pd.read_csv(filename, skiprows=5)

oo.sample(3)


🔹 skiprows=5 skips metadata rows
🔹 sample(3) displays 3 random rows

2️⃣ Remove a column (Series) using drop()
oo.drop('Position', axis=1)


🔹 Removes the Position column
🔹 axis=1 → column
🔹 This does not modify the original DataFrame unless assigned

Save the change
oo = oo.drop('Position', axis=1)

3️⃣ Remove a column while reading the file
oo = pd.read_csv(filename, skiprows=5).drop('Position', axis=1)
oo.sample(3)


🔹 Combines reading + cleaning in one line
🔹 Useful for cleaner code

4️⃣ Method chaining (recommended style)
oo = (
    pd.read_csv(filename, skiprows=5)
      .drop('Position', axis=1)
)
oo.sample(3)


🔹 Improves readability
🔹 Common in professional Pandas code

5️⃣ Remove a column in place
oo = pd.read_csv(filename, skiprows=5)
oo.drop('Position', axis=1, inplace=True)


🔹 inplace=True modifies the DataFrame directly
⚠️ Cannot be chained with other methods

6️⃣ Remove rows by index
Remove a single row
oo = (
    pd.read_csv(filename, skiprows=5)
      .drop(2, axis=0)
)
oo.head(3)


🔹 Removes row with index 2
🔹 axis=0 → row

Remove multiple rows
oo = (
    pd.read_csv(filename, skiprows=5)
      .drop([0, 1, 3], axis=0)
)
oo.head(3)


🔹 Removes rows with indices 0, 1, and 3

7️⃣ Summary of drop()
Task	Code
Drop column	drop('col', axis=1)
Drop row	drop(index, axis=0)
Save change	assign back or use inplace=True
8️⃣ Useful Pandas functions (overview)
pd.merge?
pd.concat?
oo.groupby?


🔹 merge() → combine DataFrames using keys (SQL-style joins)
🔹 concat() → stack DataFrames vertically or horizontally
🔹 groupby() → group data for aggregation (sum, mean, count)

Key Takeaways

Use axis=1 for columns, axis=0 for rows

drop() does not modify data unless assigned or inplace=True

Method chaining makes code cleaner and more readable



🔷 Rename a Series (or Column) in Pandas

This section explains multiple ways to rename columns in a Pandas DataFrame using the Olympics 1896–2004 dataset.

📥 Load the Dataset
oo = pd.read_csv(filename, skiprows=5)
oo.sample(3)


read_csv() loads the CSV file into a DataFrame

skiprows=5 skips metadata rows at the top of the file

sample(3) displays 3 random rows to preview the data

🗺️ Create a Mapper Dictionary
mapper = {
    "Athlete Name": "Athlete_Name",
    "Event Gender": "Event_Gender"
}


A dictionary where:

keys = old column names

values = new column names

Used to rename specific columns cleanly

❓ Explore the rename() Method
oo.rename?


Shows documentation for the rename() function

Useful for learning parameters and usage

✏️ Rename Columns (Without Saving)
oo.rename(columns=mapper)


Renames columns temporarily

Original DataFrame remains unchanged

💾 Rename Columns and Save Changes
oo = oo.rename(columns=mapper)
oo.sample(3)


Assigning back to oo saves the changes

Column names are now permanently updated

⚡ Rename While Reading the File
oo = pd.read_csv(filename, skiprows=5).rename(columns=mapper)
oo.sample(3)


Combines reading + renaming in one line

Cleaner and more efficient code

🧩 Inline Dictionary Rename
oo = pd.read_csv(filename, skiprows=5)
oo = oo.rename(columns={
    "Athlete Name": "Athlete_Name",
    "Event Gender": "Event_Gender"
})
oo.sample(3)


Same result as using a mapper dictionary

Written directly inside rename()

🔍 View Column Names
oo = pd.read_csv(filename, skiprows=5)
oo.columns


Displays all column names

Useful before renaming or selecting columns

🏷️ Rename All Columns at Once
column_names = [
    'Year', 'City', 'Sport', 'Discipline', 'Athlete_Name',
    'NOC', 'Gender', 'Event', 'Event_Gender', 'Medal', 'Position'
]

oo.columns = column_names
oo.sample(3)


Replaces all column names

⚠️ Number of names must exactly match number of columns

🧾 Set Column Names While Loading Data
oo = pd.read_csv(filename, skiprows=5, names=column_names)
oo.head()


Assigns custom column names during import

Pandas treats all rows as data

🧠 Use Custom Names but Keep Header Row
oo = pd.read_csv(
    filename,
    skiprows=5,
    names=column_names,
    header=0
)
oo.head()


header=0 tells Pandas to ignore the original header

Useful when replacing existing column names

✅ Key Takeaways

rename(columns=...) → best for renaming specific columns

df.columns = [...] → rename all columns at once

names= in read_csv() → rename columns while loading

Renaming columns improves readability and consistency
