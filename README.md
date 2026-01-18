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


.

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

If you want, I can:

make this shorter for a minimal README

add diagrams

or rewrite it for absolute beginners

PLEASE EXPLAIN THESE CODES FOR MY GITHUB AND PROVIDE HEADER BOLD AND BIGGER

Below is a GitHub-ready explanation with a BIG, BOLD HEADER and clear step-by-step explanations for all the code blocks you shared.
You can copy-paste this directly into your README.

🔷 Rename a Series (or Column) in Pandas

This tutorial demonstrates different ways to rename columns in a Pandas DataFrame using the Olympics 1896–2004 dataset. Renaming columns improves readability, consistency, and ease of analysis.

📦 Install and Import Required Libraries
!pip install --quiet pandas==2.0.2


Installs a specific version of Pandas to ensure consistent results.

import pandas as pd
from pathlib import Path


pandas → used for data manipulation

Path → checks if a file exists

📥 Download the Dataset (If Not Already Present)
if not Path("olympics_1896_2004.csv").exists():
    !wget https://github.com/jonfernandes/pandas_essential/raw/main/olympics_1896_2004.csv


Downloads the dataset only if it does not already exist locally.

📄 Load the CSV File
filename = "olympics_1896_2004.csv"
print("Installed all of the necessary files for this section ...")


Defines the filename and confirms setup completion.

oo = pd.read_csv(filename, skiprows=5)
oo.sample(3)


skiprows=5 skips metadata rows

sample(3) displays three random rows

🗂️ Create a Column Rename Mapping
mapper = {
    "Athlete Name": "Athlete_Name",
    "Event Gender": "Event_Gender"
}


A dictionary that maps old column names to new ones.

📖 Explore the Rename Method
oo.rename?


Displays documentation for the rename() method (useful for learning).

✏️ Rename Columns (Without Saving Changes)
oo.rename(columns=mapper)


Returns a renamed DataFrame

Does not modify the original DataFrame

💾 Rename Columns and Save the Changes
oo = oo.rename(columns=mapper)
oo.sample(3)


Assigning the result back to oo makes the change permanent.

⚡ Rename Columns While Reading the File
oo = pd.read_csv(filename, skiprows=5).rename(columns=mapper)
oo.sample(3)


Combines file loading and column renaming in one step.

🧩 Inline Rename Without a Mapper Variable
oo = pd.read_csv(filename, skiprows=5)
oo = oo.rename(columns={
    "Athlete Name": "Athlete_Name",
    "Event Gender": "Event_Gender"
})
oo.sample(3)


Renames columns directly inside the rename() function.

🔍 View Current Column Names
oo = pd.read_csv(filename, skiprows=5)
oo.columns


Displays all column names in the DataFrame.

🏷️ Rename All Columns at Once
column_names = [
    'Year', 'City', 'Sport', 'Discipline', 'Athlete_Name',
    'NOC', 'Gender', 'Event', 'Event_Gender', 'Medal', 'Position'
]

oo.columns = column_names
oo.sample(3)


Replaces all column names

⚠️ The number of new names must match the number of columns

🧾 Set Column Names During File Import
oo = pd.read_csv(filename, skiprows=5, names=column_names)
oo.head()


Assigns custom column names while reading the CSV file.

🧠 Use Custom Names and Ignore Original Header
oo = pd.read_csv(
    filename,
    skiprows=5,
    names=column_names,
    header=0
)
oo.head()


header=0 ignores the original header row

Useful when replacing existing column names completely

✅ Key Takeaways

rename(columns=...) → rename selected columns

df.columns = [...] → rename all columns

names= in read_csv() → rename during import

Renaming columns improves clarity and usability

<img width="997" height="532" alt="image" src="https://github.com/user-attachments/assets/a8a99080-4bc1-4952-8619-cc0694c45f30" />

Filtering, String Methods, and Sorting

This tutorial shows how to work with Pandas to filter rows based on conditions, manipulate strings, and sort data in a DataFrame. We use the dataset olympics_1896_2004.csv.

1. Setup and Loading Data
!pip install --quiet pandas==2.0.2

import pandas as pd
from pathlib import Path

# Download the dataset if not already present
if not Path("olympics_1896_2004.csv").exists():
    !wget https://github.com/jonfernandes/pandas_essential/raw/main/olympics_1896_2004.csv

filename = "olympics_1896_2004.csv"

# Load the CSV, skip first 5 rows, and remove 'Position' column
oo = pd.read_csv(filename, skiprows=5).drop('Position', axis=1)


Explanation:

pd.read_csv(filename, skiprows=5): Reads the CSV but skips first 5 rows (often headers or notes).

.drop('Position', axis=1): Removes the column named "Position".

2. Filtering Rows for a Single Condition
# Filter all rows where Year is 1896
oo[oo.Year == 1896]


Explanation:

oo.Year == 1896 creates a Boolean Series (True/False).

oo[...] selects only rows where the condition is True.

Other examples:

oo[oo.Year < 1896]    # Rows before 1896
oo[oo.Year <= 1896]   # Rows in or before 1896

3. Filtering Rows for Multiple Conditions
# Rows where Year is 1896 OR 2004
oo[(oo.Year == 1896) | (oo.Year == 2004)]

# Rows where City is Athens AND Year is 2004
oo[(oo.City == 'Athens') & (oo.Year == 2004)]

# Rows where City is Athens AND Year is NOT 1896
oo[(oo.City == 'Athens') & ~(oo.Year == 1896)]


Explanation:

| = logical OR

& = logical AND

~ = logical NOT

Example with more columns:

first_men_100m = oo[(oo.Year == 1896) & (oo.Gender == 'Men') & (oo.Event == '100m')]
first_men_100m[["Year", "Athlete Name", "NOC", "Event", "Medal"]]

4. Using String Methods
# Convert all athlete names to lowercase
oo["Athlete Name"].str.lower()

# Capitalize event names
oo.Event = oo.Event.str.capitalize()
oo.Event.unique()

# Filter athletes containing "Latynina"
oo[oo["Athlete Name"].str.contains("Latynina")]

# Convert city names to uppercase
oo.City = oo.City.str.upper()
oo.City.unique()


Explanation:

.str.lower(), .str.upper(), .str.capitalize(): Change string case.

.str.contains("text"): Filter rows containing the text.

5. Sorting a DataFrame or Series
# Sort a column alphabetically
oo["Athlete Name"].sort_values()

# Sort the entire DataFrame by a column
oo.sort_values("Athlete Name")

# Sort descending
oo.sort_values("Athlete Name", ascending=False)

# Sort by multiple columns
oo.sort_values(by=['Year','Athlete Name'])

# Sort by multiple columns with mixed order
oo.sort_values(by=["Year", "Event", "Medal"], ascending=[True, True, False])


Explanation:

sort_values("column"): Sort by one column.

ascending=False: Sort in descending order.

by=[col1, col2]: Sort by multiple columns.

ascending=[True, False]: Specify sort order for each column individually.

✅ Tip: Always check the first few rows with oo.head() or oo.tail() to confirm your operations worked correctly.






Data Types, Memory, Functions, and Indexing

This tutorial covers how to work efficiently with data types, categorical data, memory optimization, Python functions, indexes, and best practices in pandas.

1. Working with Data Types (dtype)
import pandas as pd
from pathlib import Path

filename = "olympics_1896_2004.csv"

oo = pd.read_csv(filename, skiprows=5).drop('Position', axis=1)
oo.sample(3)


Check current dtypes of each column:

oo.dtypes


Convert columns to category to save memory and improve performance:

oo.Medal = oo.Medal.astype("category")
oo.Gender = oo.Gender.astype("category")
oo['Event Gender'] = oo['Event Gender'].astype("category")
oo.dtypes


Ordered categorical data for sorting purposes:

medal_order = ["Bronze", "Silver", "Gold"]
oo.Medal = pd.Categorical(oo.Medal, categories=medal_order, ordered=True)


Sorting by categorical column:

oo.sort_values(by=["Year", "Event", "Medal"], ascending=[True, True, False]).head(7)

2. Memory Usage of Different dtypes

Compare memory usage between object and category:

df = pd.read_csv(filename, skiprows=5)  # Original object dtype

print("Medal memory with category:", oo.Medal.memory_usage(deep=True))
print("Medal memory with object:", df.Medal.memory_usage(deep=True))
print("Memory reduction:", oo.Medal.memory_usage(deep=True) / df.Medal.memory_usage(deep=True))


Convert string-like columns to string dtype for consistency:

oo.City = oo.City.astype("string")
oo.Sport = oo.Sport.astype("string")
oo.Discipline = oo.Discipline.astype("string")
oo["Athlete Name"] = oo["Athlete Name"].astype("string")
oo.NOC = oo.NOC.astype("string")
oo.Event = oo.Event.astype("string")
oo.dtypes

3. Defining Dtypes When Reading a File
ordered_medals = pd.api.types.CategoricalDtype(categories=["Bronze", "Silver", "Gold"], ordered=True)

dtype_mapper = {
    "Year": "int64",
    "City": "string",
    "Sport": "string",
    "Discipline": "string",
    "Athlete Name": "string",
    "NOC": "string",
    "Gender": "category",
    "Event": "string",
    "Event_gender": "category",
    "Medal": ordered_medals
}

oo = pd.read_csv(filename, skiprows=5, dtype=dtype_mapper).drop('Position', axis=1)
oo["Event Gender"] = oo["Event Gender"].astype("category")
oo.dtypes


Explanation:

Pre-defining dtypes ensures consistent memory usage and avoids conversions later.

CategoricalDtype allows specifying order for sorting.

4. Using Python Functions for Data Preprocessing

Simple function to load data:

def show_df(filename="olympics_1896_2004.csv"):
    """Read CSV and return dataframe"""
    return pd.read_csv(filename, skiprows=5)

df = show_df()


Advanced preprocessing function with dtypes:

def preprocess(filename="olympics_1896_2004.csv"):
    """Load and transform dataframe with correct dtypes"""
    ordered_medals = pd.api.types.CategoricalDtype(categories=["Bronze", "Silver", "Gold"], ordered=True)
    dtype_mapper = {
        "Year": "int64",
        "City": "string",
        "Sport": "string",
        "Discipline": "string",
        "Athlete Name": "string",
        "NOC": "string",
        "Gender": "category",
        "Event": "string",
        "Event_gender": "category",
        "Medal": ordered_medals
    }
    df = pd.read_csv(filename, skiprows=5, dtype=dtype_mapper).drop('Position', axis=1)
    df["Event Gender"] = df["Event Gender"].astype("category")
    return df

oo = preprocess()
oo.sample(3)


Explanation:

Wrapping preprocessing in a function allows reusable, clean code.

Always transform the DataFrame without modifying the original CSV.

5. Working with Indexes

View default index and columns:

oo.index
oo.columns
oo.shape


Set a column as index:

oo = oo.set_index("Athlete Name")
oo.loc["LEWIS, Carl", ["Year", "Event", "Medal"]]


Reset index back to default:

oo = oo.reset_index()
oo.head(3)


Access rows by position using .iloc:

oo.iloc[0, 0]   # First row, first column
oo.iloc[0, :]   # First row, all columns


Sorting index:

oo = oo.sort_index()
oo.head(3)

6. Best Practices in Pandas

Get immediate feedback:

oo.sample(3)


Search for methods:

import re
search_string = "excel"
[func for func in dir(pd) if re.search(rf"{search_string}", func, re.IGNORECASE)]


Check documentation in IDE:

pd.read_excel?


Use assert to validate data:

assert(oo[(oo.Year < 1896) & (oo.Year > 2004)].shape[0] == 0)
print("All tests passed")


Chain operations instead of using inplace=True:

oo = (pd.read_csv(filename, skiprows=5)
      .drop('Position', axis=1)
      .sort_values(['Year', 'Athlete Name'])
      .tail(3))


Filter using isin:

years_of_interest = [1972, 1980, 1984, 1992, 2000, 2004]
oo[oo.Year.isin(years_of_interest)]
oo[~oo.Year.isin(years_of_interest)]


This covers data types, memory optimization, functions, indexing, and best practices in pandas.

