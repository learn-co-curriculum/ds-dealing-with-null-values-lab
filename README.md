
# Dealing with Null Values

### Problem Statement

In this lab, we'll work through strategies for data cleaning and dealing with null values (NaNs).

### Objectives
* Detect null values in our dataset
* Deal with null values by dropping rows
* Deal with null values by using mean/median values as a placeholder
* Examine strategies for detecting null values encoded with a placeholder

### Dataset

In this lab, we'll continue working with the _Titanic Survivors_ dataset, which can be found in `titanic.csv`.

Before we can get going, we'll need to import the usual libraries.  In the cell below, import:
* `pandas` as `pd`
* `numpy` as `np`
* `matplotlib.pylot` as `plt`
* set `%matplotlib inline`


```python
# Import necessary libraries below
```

Now, let's get started by reading in the data from the .csv file and storing it in a DataFrame in the `df` variable below.  


```python
df = None
```

### Finding Null Values in a DataFrame

Before we can deal with null values, we first need to find them. There are several easy ways to detect them.  We will start by answering very general questions, such as "does this DataFrame contain any null values?", and then narrowing our focus each time the answer to a question is "yes"

We'll start by checking to see if the DataFrame contains **any** null values (NaNs) at all. 

**_Hint_**: If you do this correctly, it will require method chaining, and will return a boolean value for each column.  

Now we know which columns contain null values, but not how many. 

In the cell below, check chain a different method with `isna()` to check how many total null values are in each column.  

Expected Output:

```
PassengerId      0
Survived         0
Pclass           0
Name             0
Sex              0
Age            177
SibSp            0
Parch            0
Ticket           0
Fare             0
Cabin          687
Embarked         2
dtype: int64```

Now that we know how many null values exist in each column, we can make some decisions about how to deal with them.  

We'll deal with each column individually, and employ a different strategy for each.  


### Strategy 1: Dropping the Column

The first column we'll deal with is the `Cabin` column.  This column contains 687 null values, in a dataset of 891 total observations.  In other words, the strong majority of observations in this column are null values.  This is a great indicator to tell us that this column is probably a lost cause. 

Additionally, this is a column of non-numeric data--this means that we can't employ strategies for replacing null values with placeholder data such as the column mean, since we can't compute the mean of string data.  

In the cell below, drop the `Cabin` column in place from the `df` DataFrame. Then, check the remaining number of null values in the data set by using the code you wrote in the cell above.  

### Computing Placeholder Values

A common strategy for dealing with null values is to replace them with the mean or median for that column.  This strategy is often preferable because it allows us to save data instead of dropping it.  However, we risk artificially modifying the distribution of the dataset by injecting a bunch of noise into it.  For this reason, we choose the mean or median to replace the missing values, since they preserve the summary statistics of the distribution for the given column. 

Be very thoughtful about when to use this method, as often times it can introduce more problems than it solves, and possibly have a significant negative impact on your data during the modeling step.  

Before we decide to use the median or mean, we should first examine the distribution of the data.  If the distribution is skewed, the mean might not actually be as representative of the underlying data as it seems, because an outlier could be skewing it in a certain direction.  If the data is normally distributed and centered, then the mean is a good choice--otherwise, we should consider using the median. 

In the cell below, plot a histogram of values in the `'Age'` column with 80 bins (1 for each year).   Also print out the mean and median for the column.  


```python
age_mean = None
age_median = None
df['Age'].plot(kind='hist', bins=80)

print("Mean Value for Age column: {}".format(age_mean))
print("Median Value for Age column: {}".format(age_median))
```

From the visualization above, we can see the data has a slightly positive skew.  However, we can also see from the summary statistics of the column that there is not a significant difference in value between the mean and median.  In this case, it is probably safe to use the median, since this is a round number (however, we could also just round the mean to a whole number).  

In the cell below, replace all null values in the `'Age'` column with the median of the column.  **Do not hard code this value--use the methods from pandas or numpy to make this easier!**  Do this replacement in place on the DataFrame. 

In the cell below, check how many null values remain in the dataset.  

Great! Now we only need to deal with the two pesky null values in the `'Embarked'` column.  

### Dropping Rows That Contain Null Values

Perhaps the most common solution to dealing with null values is to simply drop any rows that contain them.  Of course, this is only a good idea if the number dropped does not constitute a significant portion of our dataset.  Often, you'll need to make the overall determination to see if dropping the values is an acceptable loss, or if it is a better idea to just drop an offending column (e.g. the `'Cabin'` column) or to impute placeholder values instead.

In the cell below, use the appropriate built-in DataFrame method to drop the rows containing null values. Do this in place on the DataFrame.  

Now, let's do a final check to ensure that there are no more null values remaining in this dataset.  

Great! We've dealt with all the **_obvious_** null values, but we should also take some time to make sure that there aren't symbols or numbers included that are meant to denote a missing value. 

### Missing Values with Placeholders

A common thing to see when working with datasets is missing values denoted with a preassigned code or symbol, such as `'?'` or `0`.  This is especially common in the real world for industries such as health care. Often times, databases require all data entry to have some sort of value, so the data entry team just fills in the missing values with a pre-agreed upon term or symbol that denotes that the value is missing.  Pandas will not pick these up! They see it as just another entry.  

For this reason, it is important to familiarize yourself with the dataset's **_Data Dictionary_**.  Any special cases such as this should be denoted there. However, in datasets where we do not have a Data Dictionary, it is a good practice to check to see what unique values can be found in the dataset.

**_NOTE_**: This is most common (and easiest to miss) in non-numeric data, because the offending null values will just look like another label.  However, when in numeric data, you will typically see null values encoded as 0, or possibly as an extremely large number that is clearly out of place in the distribution (e.g. `Age` = `9999`).

In the cell below, return the unique values in the `'Embarked'`, `'Sex'`, `'Pclass'`, and `'Survived'` columns to ensure that there are no values in there that we don't understand or can't account for.  

Great! Those all seem in line with our expectations.  We can confidently say that this dataset contains no pesky null values that will mess up our analysis later on!

## Conclusion

In this lab, we learned:
* How to detect null values in our dataset
* How to deal with null values by dropping rows
* How to deal with null values by imputing mean/median values 
* Strategies for detecting null values encoded with a placeholder
