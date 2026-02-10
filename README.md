# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
import pandas as pd
import numpy as np
from scipy import stats
import seaborn as sns
import matplotlib.pyplot as plt

df1 = pd.read_csv('Loan_data.csv')
df1.head()
<img width="830" height="245" alt="Screenshot 2026-02-10 101748" src="https://github.com/user-attachments/assets/1c44e74f-8ad2-463c-8a3e-a8d943662d1a" />


df1.info()
df1.describe()

df1.isnull()
df1.isnull().sum()
<img width="313" height="316" alt="Screenshot 2026-02-10 101731" src="https://github.com/user-attachments/assets/a5e48405-6d5d-475e-bf4c-bfedea6617b1" />


df1_fill_0 = df1.fillna(0)
df1_fill_0

df1_ffill = df1.ffill()
df1_ffill

df1_bfill = df1.bfill()
df1_bfill

df1['CoapplicandtIncome'] = df1['CoapplicantIncome'].fillna(df1['CoapplicantIncome'].mean())
df1

df1_dropna = df1.dropna()
df1_dropna

df1_dropna.to_csv('clean_data.csv', index=False)

df2 = pd.read_csv('clean_data.csv')
df2.head()
df2.info()
df2.describe() 

sns.boxplot(x=df2['ApplicantIncome'])
plt.show()
<img width="649" height="536" alt="Screenshot 2026-02-10 101701" src="https://github.com/user-attachments/assets/608b9956-9e48-4dc8-bdc6-b597480624ba" />


Q1 = df2['ApplicantIncome'].quantile(0.25)
Q3 = df2['ApplicantIncome'].quantile(0.75)
IQR = Q3 - Q1
print("IQR:", IQR)
<img width="145" height="51" alt="Screenshot 2026-02-10 101643" src="https://github.com/user-attachments/assets/209a6224-ef18-47e6-aaf6-2aeddc4cbd66" />


outliers_iqr = df2[
    (df2['ApplicantIncome'] < (Q1 - 1.5 * IQR)) |
    (df2['ApplicantIncome'] > (Q3 + 1.5 * IQR))
]
outliers_iqr

df2_cleaned = df2[
    ~((df2['ApplicantIncome'] < (Q1 - 1.5 * IQR)) |
      (df2['ApplicantIncome'] > (Q3 + 1.5 * IQR)))
]
df2_cleaned

data = [1,12,15,18,21,24,27,30,33,36,39,42,45,48,51,
        54,57,60,63,66,69,72,75,78,81,84,87,90,93]

df_z = pd.DataFrame(data, columns=['values'])
df_z

z_scores = np.abs(stats.zscore(df_z))
z_scores

threshold = 3
outliers_z = df_z[z_scores > threshold]
print("Outliers:")
outliers_z
<img width="197" height="111" alt="Screenshot 2026-02-10 101611" src="https://github.com/user-attachments/assets/9f85f4ac-818e-40ab-97b7-b3485851a207" />



df_z_cleaned = df_z[z_scores <= threshold]
df_z_cleaned


# Result
         Thus we have cleaned data and removed the outliers by detection using IQR and Z-score method
