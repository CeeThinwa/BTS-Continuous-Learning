(pandas-python)=
# Functional Pandas in Python workflow 🐼🐍

When beginning Data Science, it is common to directly apply Pandas functions on individual dataframes. But what if you are creating code for a Python dataflow? The approach has to
completely change.

## Sample functions

The packages being discussed in this part are loaded into Python as follows:
```
import pandas as pd
import numpy as np
```
:::{admonition} Data representation in Python:
:class: note

A good idea for transformation tracking is to represent a collection of Pandas dataframes using a dictionary because it helps you easily locate a particular dictionary to perform operations on. It can be represented like so:
```
dict = {
    'df1': df1,
    'df2': df2,
    'df3': df3,
}
```
<br>
:::

### Visualizing your dataset

🔎 The `dataframe_describer` is really useful in viewing the shape and column names of each
dataframe contained in a dictionary within a particular app.

```
def dataframe_describer(df,var_name=''):
    print(f'{var_name} has {df.shape[0]} rows and {df.shape[1]} columns namely:')
    for item in df.columns:
        print(f'{list(df.columns).index(item)+1}.', item)
```

🔎 The `dataframes_displayed` function is really useful in getting summaries of the various columns in each dataframe describing their

* measures of central tendency and data distribution if the datatype is numeric (shown in the code
below as `np.number`)
* the count, the number of unique values, the mode and its frequency if the datatype is an object (a combination of strings shown in the code below as `'O'`) or a categorical variable (shown in the
code below as `'category'`)

and checking for any missing headers or unwanted data in the last values of the dataset.

```
def dataframes_displayed(df_dict={}, input_string='', boolean=bool()):

    dataframe_describer(df=df_dict[input_string], var_name=input_string)

    for column in df_dict[input_string].columns:
        display(df_dict[input_string][column].describe(
            include=['O','category', np.number]))

    for column in df_dict[input_string].columns:
        if column.find('Unnamed:') != -1:
            boolean = True

    if boolean == True:
        display(df_dict[input_string].head(5))
        display(df_dict[input_string].tail(5))
    else:
        display(df_dict[input_string].tail(5))
```

:::{admonition} What does `column.find('Unnamed:') != -1` mean?
:class: note

According to [this source](https://tech-related.com/p/6q3ePQloAv):

*The `str.find` method is to find whether there is a character or substring to be found in*<br>
*a given string or a substring within the range of the start and end index. If found, return*<br>
*the index of the appearance position, if not found, return -1.*

So `column.find('Unnamed:') != -1` means that the `'Unnamed'` substring **is not** missing in the
column name.
<br>
:::

### Modifying your dataframes

Sometimes you may wish to do a standard operation on a number of dataframes, so it is helpful to
have functions that can do the transformations in one batch, instead of repeating code.

🔎 The `column_remover` is really useful in deleting columns in each dataframe that share the same
column name(s)

```
def column_remover(removed_columns=[],affected_dfs={}):
    modified_dfs = {}
    vals = list(affected_dfs.keys())
    for val in vals:
        modified_df = affected_dfs[val].copy()
        modified_dfs[val] = modified_df.drop(removed_columns,axis=1)
    return modified_dfs
```
:::{admonition} Changing the actual variable
:class: note
To reflect a transformation, make a copy of the dataframe, then modify the dataframe. Store it in a new dictionary and return the transformed dictionary.

It would be best practice to then save the transformation like so:
transformed_dataframes = column_remover(
    removed_columns=['a','b'],
    affected_dfs=original_dataframes)
:::

🔎 If we wish to merge changes made to dataframes, ensure the modified dataframes and original dataframes share the same name, then we can run the following code:

```
original_dataframes =  {'df1':df1,
                        'df2':df2,
                        'df3':df3}

transformed_dataframes =   {'df1':df1,
                            'df3':df3}

current_dataframes = {**original_dataframes, **transformed_dataframes}
```
:::{admonition}
:class: warning
Be VERY CAREFUL with the order; the transformed dictionary is put second, so that the update is reflected; if this is not done, the changes will be overwritten.
:::

🔎 The `blank_row_remover` is really useful in deleting blank rows in each dataframe; `thresh=3` means that blank rows and rows that have 2 filled values or less will be deleted. 

```
def blank_row_remover(affected_dfs={}):
    modified_dfs = {}
    vals = list(affected_dfs.keys())
    for val in vals:
        modified_df = affected_dfs[val].copy()
        modified_dfs[val] = modified_df.dropna(axis=0,how='all',thresh=3)
    return modified_dfs
```

🔎 The `header_promoter` is really useful in identifying dataframes within a dictionary that are
missing headers, and then promoting the header in that case.

```
def header_promoter(affected_dfs={}):
    
    boolean=bool()
    modified_dfs = {}
    vals = list(affected_dfs.keys())
    
    for val in vals:
        for column in affected_dfs[val].columns:
            if column.find('Unnamed:') != -1:
                boolean = True
                
        if boolean == True:
            modified_df = affected_dfs[val].copy()
            modified_df.columns = modified_df.iloc[0,:]
            # drop the column name source
            modified_df = modified_df.drop(index=modified_df.index[0], axis=0)
            modified_dfs[val] = modified_df
    
    return modified_dfs
```
