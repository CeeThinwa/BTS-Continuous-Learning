(pandas-python)=
# Functional Pandas in Python workflow 🐼🐍

When beginning Data Science, it is common to directly apply Pandas functions on individual dataframes. But what if you are creating code for a Python dataflow? The approach has to
completely change.

## Sample functions

```
import pandas as pd
import numpy as np
```

### Visualizing your dataset

🔎 The `dataframe_describer` is really useful in viewing characteristics of dataframes contained
in a dictionary within a particular app.

```
def dataframe_describer(df,var_name=''):
    print(f'{var_name} has {df.shape[0]} rows and {df.shape[1]} columns namely:')
    for item in df.columns:
        print(f'{list(df.columns).index(item)+1}.', item)
```

:::{admonition} Lesson 1:
:class: note
A good idea is to represent a collection of Pandas dataframes using a dictionary because it
helps you easily locate a particular dictionary to perform operations on. It can be represented like so:
```
dict = {
    'df1': df1,
    'df2': df2,
    'df3': df3,
}
```
<br>
:::

🔎 The `dataframes_displayed` function is really useful in checking for any missing headers, and getting summaries of the various columns in each dataframe describing their

* measures of central tendency and data distribution if the datatype is numeric (shown in the code
below as `np.number`)
* the count, the number of unique values, the mode and its frequency if the datatype is an object (a combination of strings shown in the code below as `'O'`) or a categorical variable (shown in the
code below as `'category'`)

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

