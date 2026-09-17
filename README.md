# ECE 2112 PROGRAMMING ASSIGNMENT 4 - DATA WRANGLING AND DATA VISUALIZATION
## Programmed By: Eddrid Gabriell Viloria, 2ECE-C
This is the repository for the FOURTH programming assignment for ECE 2112, Advanced Computer Programming and Algorithms. What you will see here is the .ipynb file of the assignment itself, alongside the xlsx file and this README file.  

**Objective/s:** The students/programmers are expected to filter tabular data using several categorical and numerical conditions. They are also expected to be able to construct focused DataFrames via selecting relevant features, as well as being able to summarize the relationship between categorical features and a numerical variable, and communicate a data comparison using clear and correctly labeled plots.   


Before anything else, it is important to declare these statements first before doing the given tasks, as everything will NOT work without it:
```python
import pandas as pd
```
```python
import matplotlib.pyplot as plt
```
* We were also instructed to display the DataFrame given to us, being `board2.xlsx`, to which we are required to display `Name`, `Gender`, `Track`, `Hometown`, `Math`, `Electronics`, and `Average`.
* Every key category is already listed in the `.xlsx` file itself, except `Average`. In order to get the average, we must use `.mean()` on the given tracks in order to do so. We can do this by typing it on this order:
```python
#First is for the original DataFrame
df = pd.read_excel('board2.xlsx')
df #This will display the DataFrame itself

#However, we are instructed to not edit the original DataFrame. With this, we can do a work around, being to copy the dataframe itself.

df_updated = df.copy()

#After this, we can use this new DataFrame to create our "Average" column.

df_updated['Average'] = df_updated[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis = 1)
df_updated #This will display our new data frame.
```

## Programming Problem A - Visayas Communication DataFrame
* Students were tasked to create a DataFrame "VisComm", which would contain students whose hometown is "Visayas", and whose track is "Communication". We were required to retain only these columns in this stated order: `Name`, `Gender`, `Math`, `Electronics`, `Average`.
* In order to do this, we simply use `.loc`, along with logic operators to include the truth values for the hometown and the track, then followed by the columns to be included in the `VisComm` DataFrame. Putting this together, we get this code:
```python
VisComm = df_updated.loc[(df_updated['Hometown'] == 'Visayas')&(df_updated['Track'] == 'Communication'), ['Name', 'Gender', 'Math', 'Electronics', 'Average']]
VisComm #This will display the DataFrame
```
* As an additional task, we were also asked to display the number of rows in this new DataFrame, in order to do this, we could simply use `.len()`, which counts the number of rows in a given DataFrame. This can be done with this code shown below:
```python
print('Number of Rows: ')
len(VisComm)
```

## Programming Problem B - Visayas Female DataFrame
* Just like in Programming Problem A, students were tasked to create a DataFrame, this time under the label "VisFemale", which would contain students whose hometown is also "Visayas", however, with the other category whose gender is "Female". Similarly, we are tasked to retain these columns in this order, with minor differences: `Name`, `Track`, `GEAS`, `Electronics`, `Average`.
* The code is almost similar, as we will still be using `.loc`, alongside logic operators, but instead of track, we will be looking at the gender, alongside a few changes in the columns to be included. With this, we end up with this code:
```python
VisFemale = df_updated.loc[(df_updated['Gender'] == 'Female')&(df_updated['Hometown'] == 'Visayas'), ['Name', 'Track', 'GEAS', 'Electronics', 'Average']]
VisFemale
```
* Additionally, we were also tasked to display the rows whose average is at least 60. We will use `.loc` for this case, alongside stating within the bracket that the average must be greater than or equal to 60. This can be done by typing in this code:
```python
VisFemale.loc[(VisFemale['Average'] >= 60)]
```

## Programming Problem C - Category-Average Visualization
* Examine how the recorded Average differs across the three categorical features Track, Gender, and
Hometown.
* This task has 4 parts:
  - For each feature, compute the mean of Average for every category using Pandas.
  - Display the three summary tables.
  - Create one figure containing three bar charts: mean Average by Track, by Gender, and by
Hometown.
  - Below the figure, write three concise statements identifying the category with the highest sample
mean for each feature.
* In order to compute for the mean of the average of each category, we can use `.groupby()` in order to gather the rows that possess the desired category, which would then be followed by `['Average']`, referring to the "Average" column, then followed by `.mean()` for the mean of all values within this column, then ended with `.reset_index()` in order to reset their standard numerical index and most importantly turn them back from your typical text list to a DataFrame. Putting this all together, we end up with this code:
```python
Track_Average = df_updated.groupby('Track')['Average'].mean().reset_index()
Gender_Average = df_updated.groupby('Gender')['Average'].mean().reset_index()
Hometown_Average = df_updated.groupby('Hometown')['Average'].mean().reset_index()
```
* In order to display the summary tables, we simply call for them in their own cells by their variable name:
```python
print('Mean of the average in "Track"')
Track_Average
```
```python
print('Mean of the average in "Gender"')
Gender_Average
```
```python
print('Mean of the average in "Hometown"')
Hometown_Average
```
* In order to create the three bar charts, along with the concise statements regarding the category with the highest sample mean for each feature, we can first use `plt.subplots(nrow, ncol, figsize = (width, length)` to create our three graphs. It's important to note that we must equate this to `fig, axes` or anything with this format: `The_Entire_Canvas, The_Graphs_Itself`. Afterwards, we use `.bar(x-values, y-values, color = 'Insert_Color_Here')`, followed by `.set_title`, `.set_xlabel()`, and `.set_ylabel()`. However, y label can be declared with just the first graph to avoid a repeated label throughout the entire subplot. All of this while keeping in mind the index of the graphs itself beforehand. Doing so will give us this large piece of code:
```python
axes[0].bar(Track_Average['Track'], Track_Average['Average'], color ='yellow')
axes[0].set_title('Mean/Average by Track')
axes[0].set_xlabel('Track')
axes[0].set_ylabel('Mean of the Average Scores')

axes[1].bar(Gender_Average['Gender'], Gender_Average['Average'], color ='blue')
axes[1].set_title('Mean/Average by Gender')
axes[1].set_xlabel('Gender')

axes[2].bar(Hometown_Average['Hometown'], Hometown_Average['Average'], color ='red')
axes[2].set_title('Mean/Average by Hometown')
axes[2].set_xlabel('Hometown')
```
* Afterwards, we can print the entire subplot itself by using the code `plt.show()`, then the interpretation. This is done via the text of the sentences itself, alongside the usage of `.groupby()`, followed by `.mean()` and `.idxmax()` in order to display the category with the highest mean sample size. This is then followed by another `.groupby()`, then `.mean()`, but instead of `.idxmax()`, we use `.max()` in order to get the actual value of the highest sample size. Doing this on all three categories will give us this code:
```python
plt.show()
print('The track with the highest average on the list is', Track_Average.groupby('Track')['Average'].mean().idxmax(),'attaining an average of', Track_Average.groupby('Track')['Average'].mean().max())
print('The gender with the highest average on the list is', Gender_Average.groupby('Gender')['Average'].mean().idxmax(),'attaining an average of', Gender_Average.groupby('Gender')['Average'].mean().max())
print('The hometown with the highest average on the list is', Hometown_Average.groupby('Hometown')['Average'].mean().idxmax(),'attaining an average of', Hometown_Average.groupby('Hometown')['Average'].mean().max())
```

## Versions
**Sept. 17, 2026**  
* Version 0.1
    - Development of PA4 initiated.
    - Solutions for Programming Problems A and B are fully functional.
    - Solutions and Tables for Programming Problem C fully functional.
 * Version 1.0
    - Interpretation of the three tables fully written.
    - Full Release
