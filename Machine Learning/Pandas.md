Start by importing Pandas:  
```python  
import pandas as pd  
```  
  
---  
  
### **Step 3: Basic Data Structures**  
  
#### 1. **Series** (1D Data)  
A Series is like a column in a table.  
```python  
import pandas as pd  
  
# Creating a Series  
s = pd.Series([10, 20, 30, 40])  
print(s)  
```  
  
**Output:**  
```  
0    10  
1    20  
2    30  
3    40  
dtype: int64  
```  
  
#### 2. **DataFrame** (2D Data)  
A DataFrame is like a table or spreadsheet.  
```python  
# Creating a DataFrame  
data = {  
    'Name': ['Alice', 'Bob', 'Charlie'],  
    'Age': [25, 30, 35],  
    'Salary': [50000, 60000, 70000]  
}  
  
df = pd.DataFrame(data)  
print(df)  
```  
  
**Output:**  
```  
      Name  Age  Salary  
0    Alice   25   50000  
1      Bob   30   60000  
2  Charlie   35   70000  
```  
  
---  
  
### **Step 4: Reading and Writing Data**  
  
#### 1. Reading from CSV  
```python  
df = pd.read_csv('data.csv')  
print(df.head())  # Display the first 5 rows  
```  
  
#### 2. Writing to CSV  
```python  
df.to_csv('output.csv', index=False)  
```  
  
#### 3. Reading from Excel  
```python  
df = pd.read_excel('data.xlsx')  
```  
  
#### 4. Writing to Excel  
```python  
df.to_excel('output.xlsx', index=False)  
```  
  
---  
  
### **Step 5: Basic Operations on DataFrames**  
  
#### 1. Viewing Data  
```python  
df.head()     # First 5 rows  
df.tail()     # Last 5 rows  
df.shape      # Dimensions of the DataFrame  
df.info()     # Summary of the DataFrame  
df.describe() # Statistics for numeric columns  
```  
  
#### 2. Selecting Columns  
```python  
# Single column  
df['Name']  
  
# Multiple columns  
df[['Name', 'Salary']]  
```  
  
#### 3. Selecting Rows  
```python  
# Select rows by index  
df.iloc[0]      # First row  
df.iloc[0:2]    # First two rows  
  
# Select rows by condition  
df[df['Age'] > 30]  
```  
  
#### 4. Adding Columns  
```python  
df['Bonus'] = df['Salary'] * 0.1  
```  
  
#### 5. Dropping Columns  
```python  
df = df.drop('Bonus', axis=1)  # Drop the 'Bonus' column  
```  
  
---  
  
### **Step 6: Handling Missing Data**  
  
#### 1. Checking for Missing Values  
```python  
df.isnull().sum()  
```  
  
#### 2. Filling Missing Values  
```python  
df['Age'].fillna(df['Age'].mean(), inplace=True)  # Replace NaNs with the mean  
```  
  
#### 3. Dropping Missing Values  
```python  
df.dropna(inplace=True)  
```  
  
---  
  
### **Step 7: Data Transformation**  
  
#### 1. Renaming Columns  
```python  
df.rename(columns={'Name': 'Full Name'}, inplace=True)  
```  
  
#### 2. Sorting Data  
```python  
df.sort_values(by='Age', ascending=False, inplace=True)  
```  
  
#### 3. Applying Functions  
```python  
df['Salary'] = df['Salary'].apply(lambda x: x * 1.1)  # Increase salary by 10%  
```  
  
---  
  
### **Step 8: Grouping and Aggregation**  
  
#### Grouping  
```python  
grouped = df.groupby('Age')  
print(grouped['Salary'].mean())  
```  
  
#### Aggregation  
```python  
df.groupby('Age').agg({'Salary': ['mean', 'sum']})  
```  
  
---  
  
### **Step 9: Joining and Merging**  
  
#### Concatenation  
```python  
df1 = pd.DataFrame({'ID': [1, 2], 'Name': ['Alice', 'Bob']})  
df2 = pd.DataFrame({'ID': [3, 4], 'Name': ['Charlie', 'David']})  
pd.concat([df1, df2])  
```  
  
#### Merging  
```python  
df1 = pd.DataFrame({'ID': [1, 2], 'Name': ['Alice', 'Bob']})  
df2 = pd.DataFrame({'ID': [1, 2], 'Salary': [50000, 60000]})  
pd.merge(df1, df2, on='ID')  
```  
  
---  
  
### **Step 10: Visualization with Pandas**  
```python  
import matplotlib.pyplot as plt  
  
df['Age'].plot(kind='hist')  
plt.show()  
```  
  
---  
  
