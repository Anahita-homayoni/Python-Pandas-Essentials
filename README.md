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

Series, DataFrames, Dates, and Combining Datasets

This tutorial covers creating pandas Series and DataFrames, working with dates, and combining datasets using concat and merge.

1. Creating Series and DataFrames
import pandas as pd

city = ["London", "Rio", "Tokyo"]
start_date = ["27th Jul, 2012", "5th Aug, 2016", "23rd July, 2021"]


Create a Series:

pd.Series(city)


Create a DataFrame using Series or lists:

pd.DataFrame({"City": pd.Series(city),
              "Start Date": pd.Series(start_date)})

pd.DataFrame({"City": city,
              "Start Date": start_date})


Create a DataFrame using zip:

pd.DataFrame(zip(city, start_date), columns=["City", "Start Date"])


Explanation:

Series is a 1D labeled array, like a single column.

DataFrame is a 2D labeled data structure, like a table.

zip() allows combining multiple lists column-wise.

2. Working with Dates
end_date = ["12th Aug, 2012", "21-08-2016", "8th Aug, 2021"]

games = pd.DataFrame(zip(city, start_date, end_date), columns=["City", "Start Date", "End Date"])
games.dtypes


Convert string columns to datetime:

games["Start Date"] = pd.to_datetime(games["Start Date"], format='mixed')
games["End Date"] = pd.to_datetime(games["End Date"], format='mixed')
games["City"] = games.City.astype("string")
games.dtypes


Calculate duration of each event:

games = games.assign(duration=games["End Date"] - games["Start Date"])
games


Explanation:

pd.to_datetime() converts strings to datetime objects.

Subtracting two datetime columns gives a timedelta object (duration).

.assign() creates a new column without modifying existing ones directly.

3. Combining DataFrames
3.1 Using concat
start = pd.DataFrame({"city": ["London", "Rio", "Tokyo"],
                      "start_date": ["27th Jul, 2012", "5th Aug, 2016", "23rd July, 2021"]})

end = pd.DataFrame({"city": ["London", "Tokyo", "Paris"],
                    "end_date": ["12th Aug, 2012", "8th Aug, 2021", "11th Aug, 2024"]})


Concatenate vertically (axis=0):

pd.concat([start, end], axis=0)


Concatenate horizontally (axis=1):

pd.concat([start, end], axis=1)

3.2 Using merge (SQL-style joins)

Inner join: Only rows with matching city values:

pd.merge(left=start, right=end, on="city", how="inner")


Outer join: All rows from both DataFrames:

pd.merge(left=start, right=end, on="city", how="outer")


Left join: All rows from left, matching rows from right:

pd.merge(left=start, right=end, on="city", how="left")


Right join: All rows from right, matching rows from left:

pd.merge(left=start, right=end, on="city", how="right")


Explanation:

on="city" specifies the key for joining.

how= defines type of join: inner, outer, left, right.

4. Combining Datasets (Olympics Example)

Load 1896–2004 dataset:

def preprocess(filename="olympics_1896_2004.csv"):
    """Load and transform the Olympics dataset"""
    ordered_medals = pd.api.types.CategoricalDtype(categories=["Bronze", "Silver", "Gold"], ordered=True)
    dtype_mapper = {
        "Year": "int64", "City": "string", "Sport": "string", "Discipline": "string",
        "Athlete Name": "string", "NOC": "string", "Gender": "category",
        "Event": "string", "Event_gender": "category", "Medal": ordered_medals
    }
    df = pd.read_csv(filename, skiprows=5, dtype=dtype_mapper).drop('Position', axis=1)
    df["Event Gender"] = df["Event Gender"].astype("category")
    return df

oo = preprocess()
oo.sample(3)


Load 2008 dataset and align columns:

new_filename = "olympics_2008.csv"
nw = pd.read_csv(new_filename)
nw.columns = ['City', 'Year', 'Sport', 'Discipline', 'Athlete Name', 'NOC',
              'Gender', 'Event', 'Event Gender', 'Medal', 'Result']
nw = nw.drop("Result", axis=1)
nw.sample(3)


Combine 1896–2004 with 2008:

combined = pd.concat([oo, nw], axis=0)
combined.sample(3)
combined.dtypes


Explanation:

Always ensure columns match before concatenating.

Use concat for stacking datasets vertically.

Use merge if you want to join datasets based on a key column.

This tutorial demonstrates how to:

Create Series and DataFrames

Work with dates and calculate durations

Combine datasets with concat and merge

Prepare real-world datasets for analysis



Handling Missing Data and Duplicates

This tutorial covers techniques to detect, fill, remove missing data and handle duplicate rows in pandas DataFrames.

1. Loading the Dataset
import pandas as pd
from pathlib import Path

# Ensure data files are available
if not Path("olympics_1896_2004.csv").exists():
    !wget https://github.com/jonfernandes/pandas_essential/raw/main/olympics_1896_2004.csv
if not Path("olympics_2008.csv").exists():
    !wget https://github.com/jonfernandes/pandas_essential/raw/main/olympics_2008.csv

filename = "olympics_1896_2004.csv"


Preprocessing function for 2008 Olympics:

def preprocess_2008(filename="olympics_2008.csv"):
    """Load 2008 Olympics data, fix missing values, and drop unnecessary columns"""
    df = pd.read_csv(filename)
    df.columns = ['City', 'Year', 'Sport', 'Discipline', 'Athlete Name', 'NOC',
                  'Gender', 'Event', 'Event Gender', 'Medal', 'Result']
    df = df.drop("Result", axis=1)
    df.City = df.City.fillna(value="Beijing")  # Fill missing city names
    df.Year = df.Year.fillna(value=2008)       # Fill missing years
    return df

nw = preprocess_2008()
nw.sample(3)

2. Detecting Missing Data

Check for missing values in a column:

nw.City.isna()
nw.City.isna().sum()   # Total missing values


View rows with missing data:

nw[nw.City.isna()]


Explanation:

.isna() returns a Boolean Series (True if value is missing).

.sum() counts how many missing values exist.

3. Filling Missing Data

Fill missing values using .fillna():

nw.City = nw.City.fillna(value="Beijing")
nw.Year = nw.Year.fillna(value=2008)
nw.info()


Explanation:

.fillna(value=...) replaces all NaN with a specified value.

Useful for categorical or numeric columns where a default makes sense.

4. Removing Missing Data

Drop rows where all values are missing:

nw.dropna(how="all", axis=0)


Drop rows where any value is missing:

nw.dropna(how="any", axis=0)


Drop rows missing in specific columns:

nw = nw.dropna(subset=['Sport', 'Discipline', 'Athlete Name', 'NOC', 
                        'Gender', 'Event', 'Event Gender', 'Medal'], how="all")
nw.info()


Explanation:

how="all" drops rows if all specified columns are missing.

how="any" drops rows if any specified column is missing.

subset allows targeting specific columns.

5. Detecting Duplicates

Check for duplicates:

nw.duplicated()
nw.duplicated().sum()  # Total duplicated rows
nw.loc[nw.duplicated(), :]


Explanation:

.duplicated() returns a Boolean Series marking duplicate rows.

.sum() counts total duplicates.

6. Removing Duplicates

Remove duplicate rows:

nw = nw.drop_duplicates()
nw.shape


Detect duplicates based on specific columns:

athlete_multiple_events = nw.duplicated(subset=['Athlete Name', 'NOC', 'Gender'])
nw.loc[athlete_multiple_events, :].sort_values("Athlete Name")


Explanation:

subset specifies which columns define duplicates.

Sorting helps review duplicates for specific athletes/events.

Example: Check a specific athlete:

nw.loc[nw["Athlete Name"] == "ZOU, Kai"]

7. Full Preprocessing Function with Missing Data and Duplicates Handling
def preprocess_2008(filename="olympics_2008.csv"):
    """Load 2008 Olympics dataset, fill missing values, drop empty rows and duplicates"""
    df = pd.read_csv(filename)
    df.columns = ['City', 'Year', 'Sport', 'Discipline', 'Athlete Name', 'NOC',
                  'Gender', 'Event', 'Event Gender', 'Medal', 'Result']
    df = df.drop("Result", axis=1)
    df.City = df.City.fillna(value="Beijing")
    df.Year = df.Year.fillna(value=2008)
    df = df.dropna(subset=['Sport', 'Discipline', 'Athlete Name', 'NOC', 
                            'Gender', 'Event', 'Event Gender', 'Medal'], how="all")
    df = df.drop_duplicates()
    return df

nw = preprocess_2008()
nw.sample(3)


Explanation:

Step 1: Fill missing values for essential columns.

Step 2: Drop rows where all key columns are missing.

Step 3: Remove duplicates for clean analysis.

✅ Summary of Best Practices:

Always inspect missing values with .isna() and .info().

Fill or drop missing values depending on context.

Use .duplicated() to identify duplicates before removing.

Chain operations in a preprocessing function for reproducibility.



🚀 VALIDATING AND PREPARING OLYMPICS DATA USING PANDAS

This section demonstrates how to clean, validate, standardize, and combine datasets using pandas. The goal is to ensure that historical Olympics data (1896–2004) and modern data (2008) follow the same structure, rules, and data types before merging them.

📦 INSTALLING AND IMPORTING DEPENDENCIES
!pip install --quiet pandas==2.0.2


Installs a specific pandas version to ensure consistent behavior.

import pandas as pd
from pathlib import Path


pandas is used for data manipulation.

Path helps check if files already exist before downloading.

📥 DOWNLOADING THE DATASETS
if not Path("olympics_1896_2004.csv").exists():
  !wget https://github.com/jonfernandes/pandas_essential/raw/main/olympics_1896_2004.csv

if not Path("olympics_2008.csv").exists():
  !wget https://github.com/jonfernandes/pandas_essential/raw/main/olympics_2008.csv


Downloads datasets only if they are missing.

Prevents redundant downloads.

🧹 PREPROCESSING HISTORICAL OLYMPICS DATA (1896–2004)
✅ Why Preprocessing Is Needed

Fix incorrect values

Enforce consistent data types

Standardize text formatting

Prepare data for validation and merging

🔧 Preprocessing Function for 1896–2004
def preprocess(filename="olympics_1896_2004.csv"):

🔹 Step 1: Define Ordered Medal Categories
ordered_medals = pd.api.types.CategoricalDtype(
    categories=["Bronze", "Silver", "Gold"], ordered=True
)


Ensures medals have a logical ranking order.

🔹 Step 2: Define Column Data Types
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


Prevents mixed or incorrect data types.

Improves performance and consistency.

🔹 Step 3: Read and Clean the Dataset
df = (pd.read_csv(filename, skiprows=5, dtype=dtype_mapper)
      .drop('Position', axis=1))


skiprows=5 removes metadata rows.

Drops unnecessary columns.

🔹 Step 4: Fix Data Errors
df.loc[24676, "Gender"] = "Women"


Corrects a known incorrect value.

🔹 Step 5: Standardize Text Formatting
df.Sport = df.Sport.str.lower()
df.Discipline = df.Discipline.str.lower()
df.Event = df.Event.str.lower()
df.NOC = df.NOC.str.upper()


Ensures consistent text comparisons across datasets.

📊 Result
oo = preprocess()
oo.dtypes


Data is clean, validated, and standardized.

🧹 PREPROCESSING OLYMPICS 2008 DATA

The 2008 dataset has a different structure, so it needs extra cleaning.

🔧 Preprocessing Function for 2008 Data
🔹 Rename Columns
df.columns = [
 'City','Year','Sport','Discipline','Athlete Name','NOC',
 'Gender','Event','Event Gender','Medal','Result'
]


Matches column names with the historical dataset.

🔹 Remove Unnecessary Columns
df = df.drop("Result", axis=1)

🔹 Fill Missing Values
df.City = df.City.fillna("Beijing")
df.Year = df.Year.fillna(2008)


Ensures no missing key values.

🔹 Remove Empty Rows and Duplicates
df = df.dropna(how="all")
df = df.drop_duplicates()

🔹 Standardize Text Formatting
df.Sport = df.Sport.str.lower()
df.Discipline = df.Discipline.str.lower()
df.Event = df.Event.str.lower()
df.NOC = df.NOC.str.upper()
df.Medal = df.Medal.str.capitalize()

🔹 Convert Data Types
df.Gender = df.Gender.astype("category")
df['Event Gender'] = df['Event Gender'].astype("category")
df.Medal = pd.Categorical(
    df.Medal,
    categories=["Bronze", "Silver", "Gold"],
    ordered=True
)


Aligns data types with the historical dataset.

📊 Result
nw = preprocess_2008()
nw.dtypes


2008 data is now fully compatible.

🔍 DATA VALIDATION USING ASSERTIONS
✅ Why Validate?

Ensures both datasets share the same rules

Prevents silent data corruption

✔️ Validate Categories
assert sorted(nw["Event Gender"].unique()) == sorted(oo["Event Gender"].unique())
assert sorted(nw.Gender.unique()) == sorted(oo.Gender.unique())
assert sorted(nw.Medal.unique()) == sorted(oo.Medal.unique())


Confirms both datasets use the same categories.

🎉 Validation Passed
print("Passes all tests ...")

🔗 COMBINING BOTH DATASETS

Once validated, the datasets can be safely merged.

up = pd.concat([oo, nw])


Stacks rows vertically.

Preserves data types and categories.

📊 Final Dataset
up.sample(3)
up.dtypes


✔ Unified
✔ Clean
✔ Validated
✔ Analysis-ready

✅ FINAL OUTCOME

You now have:

Clean historical data (1896–2004)

Clean modern data (2008)

Validated schemas

A single combined Olympics dataset ready for analysis 🏅




🟢🟢🟢 PLOTTING DATA 🟢🟢🟢

Below is the same notebook structure you provided, but with:
✅ Bigger & bold title
✅ Much more explanation
✅ Clear comments so you understand what each step is doing and why
✅ Learning-focused descriptions for plotting, seaborn, groupby, and reshaping

📦 Installing Libraries & Loading Data
!pip install --quiet pandas==2.0.2

import pandas as pd
from pathlib import Path
import matplotlib.pyplot as plt

📌 Purpose:

pandas → data handling

pathlib → file checking

matplotlib.pyplot → plotting graphs

📥 Downloading the Datasets
if not Path("olympics_1896_2004.csv").exists():
  !wget https://github.com/jonfernandes/pandas_essential/raw/main/olympics_1896_2004.csv
if not Path("olympics_2008.csv").exists():
  !wget https://github.com/jonfernandes/pandas_essential/raw/main/olympics_2008.csv

filename = "olympics_1896_2004.csv"
print("Installed all of the necessary files for this section ...")

📌 Purpose:

Checks if the CSV files already exist

Downloads them only if missing

Prevents re-downloading every time the notebook runs

🧹 Preprocessing Functions
🔹 Old Olympics Data (1896–2004)
def preprocess(filename = "olympics_1896_2004.csv"):
  """Preparing and transforming dataframe"""
  print(f"Preprocessing {filename} ...\n")

  ordered_medals = pd.api.types.CategoricalDtype(
      categories=["Bronze", "Silver", "Gold"], ordered=True)

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

  df = (pd.read_csv(filename, skiprows=5, dtype=dtype_mapper)
        .drop('Position', axis=1)
  )

  # Fix data types and errors
  df["Event Gender"] = df["Event Gender"].astype("category")
  df.loc[24676, "Gender"] = "Women"

  # Standardise text format
  df.Sport = df.Sport.str.lower()
  df.Discipline = df.Discipline.str.lower()
  df.Event = df.Event.str.lower()
  df.NOC = df.NOC.str.upper()

  return df

📌 Why this matters:

Makes medal values ordered → Bronze < Silver < Gold

Fixes one wrong gender record

Makes text consistent:

sports/events → lowercase

country codes → uppercase

Drops unnecessary column (Position)

🔹 2008 Olympics Data
def preprocess_2008(filename="olympics_2008.csv"):
  print(f"Preprocessing {filename} ...\n")

  df = pd.read_csv(filename)
  df.columns = ['City', 'Year', 'Sport', 'Discipline', 'Athlete Name', 'NOC',
       'Gender', 'Event', 'Event Gender', 'Medal', 'Result']

  df = df.drop("Result", axis=1)

  # Fill missing values
  df.City = df.City.fillna(value="Beijing")
  df.Year = df.Year.fillna(value=2008)

  # Remove empty rows and duplicates
  df = df.dropna(subset=['Sport', 'Discipline', 'Athlete Name', 'NOC', 'Gender',
       'Event', 'Event Gender', 'Medal'], how="all")
  df = df.drop_duplicates()

  # Standardise text
  df.Sport = df.Sport.str.lower()
  df.Discipline = df.Discipline.str.lower()
  df.Event = df.Event.str.lower()
  df.NOC = df.NOC.str.upper()
  df.Medal = df.Medal.str.capitalize()

  # Correct data types
  df.City = df.City.astype("string")
  df.Year = df.Year.astype(int)
  df.Sport = df.Sport.astype("string")
  df.Discipline = df.Discipline.astype("string")
  df["Athlete Name"] = df["Athlete Name"].astype("string")
  df.NOC = df.NOC.astype("string")
  df.Gender = df.Gender.astype("category")
  df.Event = df.Event.astype("string")
  df['Event Gender'] = df['Event Gender'].astype("category")

  medal_order = ["Bronze", "Silver", "Gold"]
  df.Medal = pd.Categorical(df.Medal, categories=medal_order, ordered=True)

  return df

🔗 Combine Both Datasets
oo = preprocess()
nw = preprocess_2008()
up = pd.concat([oo, nw])
up.sample(3)

📌 Purpose:

Merges 1896–2004 + 2008 data

Creates one full Olympics dataset: up

📊 Plotting Data
❓ Question:

For the first Olympics (1896), how many events were there for each sport?

🔹 Filter first Olympics
first_games = up[up.Year == 1896]
first_games

🔹 Count events per sport
first_games.Sport.value_counts()


📌 This counts how many rows (events) exist for each sport.

📈 Line Plot
first_games.Sport.value_counts().plot(kind='line')

📌 Meaning:

Shows trend-style view of events per sport

Not ideal for categories, but useful to demonstrate line plotting

📊 Bar Plot
(first_games
 .Sport
 .value_counts()
 .plot(kind='bar')
)

📌 Meaning:

Best graph for categorical data

Shows number of events per sport clearly

📊 Horizontal Bar Plot
(first_games
 .Sport
 .value_counts()
 .plot(kind='barh')
)

📌 Meaning:

Same data as bar plot

Easier to read long sport names

🎨 Custom Colours
(first_games
 .Sport
 .value_counts()
 .plot(kind='barh', color=['blue', 'red'])
)

📌 Meaning:

Custom colours improve readability

Can use color lists or colormaps

🧠 Why Plotting Matters

Plotting helps:
✅ identify trends
✅ compare categories
✅ detect outliers
✅ communicate results visually
✅ support data-driven decisions

🎨 Working with Seaborn & Colormaps
🔹 Filter 2008 Games
games_2008 = up[up.Year == 2008]

📊 Countplot (Medals)
import seaborn as sns

plt.figure(figsize=(5,3))
plt.title("Medals from the 2008 games")
sns.countplot(data=games_2008, x='Medal')


📌 Shows number of medals by type.

📊 Ordered Medals
sns.countplot(
    data=games_2008,
    x='Medal',
    order=["Gold", "Silver", "Bronze"]
)


📌 Ensures correct medal ranking order

👫 Gender Comparison
sns.countplot(
    data=games_2008,
    x='Medal',
    order=["Gold", "Silver", "Bronze"],
    hue='Gender'
)


📌 Compares male vs female medal counts

🎨 Colour Palettes
sns.countplot(
    data=games_2008,
    x='Medal',
    order=["Gold", "Silver", "Bronze"],
    hue='Gender',
    palette='coolwarm'
)


📌 Uses a diverging colormap for contrast

🧮 Working with groupby
sprints = up[(up.Year == 2008) & ((up.Event == '100m') | (up.Event == '200m'))]


📌 Filters sprint events

sp = sprints.groupby(['NOC', 'Gender', 'Event'])


📌 Groups by:

Country

Gender

Event

up.groupby("Year").size()


📌 Counts records per year

up.groupby(['Year','NOC','Medal']).size()


📌 Counts medals by:

year

country

medal type

🔁 Reshaping Data
sp = sprints.groupby(['NOC','Gender','Event']).size()


📌 Creates MultiIndex Series

🔄 Unstacking
sp.unstack('Gender', fill_value=0)


📌 Converts:

index level → columns

fills missing values with 0

sp.unstack(['Gender', 'Event'], fill_value=0)


📌 Creates a table format (pivot-style)

🔄 Stacking Back
sprints_table = sp.unstack(level=1, fill_value=0).unstack(level=1, fill_value=0)
sprints_NOC = sprints_table.stack("Gender")


📌 Converts wide format → long format again

📍 Data Access Examples
sprints_NOC.loc[('JAM', 'Men'), :]


📌 Gets all sprint results for Jamaica men

sprints_NOC.loc[('JAM', 'Men'), '100m']


📌 Gets only 100m sprint medals for Jamaican men

🎯 Learning Outcomes

By this section, you now understand how to:

✅ plot with pandas
✅ plot with matplotlib
✅ plot with seaborn
✅ use bar, line, horizontal bar plots
✅ use colormaps
✅ group data
✅ reshape data
✅ stack / unstack
✅ use MultiIndex
✅ analyse categories
✅ compare groups
✅ visualise distributions
✅ build analytical tables


http://matplotlib.org/stable/users/explain/colors/colormaps.html

What iloc does

iloc lets you access data using row and column indices, starting from 0.

Syntax
df.iloc[row_index, column_index]

Common uses

1. Select a single row

df.iloc[0]      # first row


2. Select a single column

df.iloc[:, 1]   # second column


3. Select specific rows and columns

df.iloc[0:3, 1:4]   # rows 0–2, columns 1–3


4. Select multiple rows

df.iloc[[0, 2, 4]]

Key points

Uses integer positions, not labels

Indexing starts at 0

Similar to Python list slicing

End index is excluded (like 0:3 → 0,1,2)

iloc vs loc
Feature	iloc	loc
Indexing type	Integer position	Label-based
Includes end index	❌ No	✅ Yes


iris.to_csv() in Pandas
Purpose

The to_csv() function in pandas is used to export a DataFrame to a CSV (Comma-Separated Values) file.

Example Code
iris.to_csv("iris.csv")

Explanation

iris → a pandas DataFrame (for example, the Iris dataset)

to_csv() → a pandas method that writes the DataFrame to a CSV file

"iris.csv" → the name of the output file

This line saves the entire contents of the iris DataFrame into a file called iris.csv in the current working directory.

Common Parameters
iris.to_csv("iris.csv", index=False)

Parameter	Description
index=False	Prevents pandas from writing row indices to the CSV
sep=','	Specifies the delimiter (comma by default)
header=True	Writes column names (default behavior)
encoding='utf-8'	Sets file encoding
Example with Options
iris.to_csv("iris.csv", index=False, encoding="utf-8")


✔ Exports data
✔ Removes row index
✔ Uses UTF-8 encoding

Why Use to_csv()?

Share data easily




Pandas Display Option: pd.set_option('display.max_columns', 2)
Overview

The pandas function pd.set_option() is used to customize how DataFrames are displayed in the output.
The option display.max_columns controls the maximum number of columns shown when a DataFrame is printed.

Syntax
pd.set_option('display.max_columns', 2)

Explanation

pd.set_option() → Sets a pandas configuration option

'display.max_columns' → Specifies how many columns can be displayed

2 → Limits the display to 2 columns only

If the DataFrame contains more than 2 columns, pandas will hide the extra columns and replace them with ....

Example
import pandas as pd

pd.set_option('display.max_columns', 2)

df = pd.DataFrame({
    'Name': ['A', 'B'],
    'Age': [20, 21],
    'City': ['NY', 'LA']
})

print(df)

Output
  Name  ...  City
0    A  ...   NY
1    B  ...   LA

Why Use This Option?

Improves readability for wide DataFrames

Keeps notebook or console output clean

Useful for debugging and presentations

Reset to Default
pd.reset_option('display.max_columns')


Store processed datasets

Load data into Excel, databases, or other tools
