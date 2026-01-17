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
