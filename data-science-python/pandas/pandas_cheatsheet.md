# Pandas Arguments and Useful Functions

Pandas official documentation: https://pandas.pydata.org/docs/

## Commonly Used Arguments in Pandas

### General Arguments
- **`axis`**: 
  - Determines the axis for the operation.
  - Options: `0` (rows) or `1` (columns).
- **`inplace`**:
  - Whether to modify the original object.
  - Options: `True` or `False` (default).
- **`ascending`**:
  - Sort order.
  - Options: `True` (ascending) or `False` (descending).
- **`drop`**:
  - Whether to drop the column/row from the result.
  - Options: `True` or `False` (default).
- **`how`**:
  - For filtering/join operations.
  - Options: `‘any’` (default) or `‘all’`.
- **`na_position`**:
  - Position of `NaN` during sorting.
  - Options: `‘first’` or `‘last’` (default).

---

## `read_csv()` Arguments
- **`pd.read_csv("filename.csv") | df.to_csv('filename.csv')`**: To read a csv file and to convert a dataframe to a csv file.
- **`filepath_or_buffer`**: Path to the CSV file or a file-like object.
- **`sep`**: Delimiter used to separate values (default: `','`).
- **`header`**: Row number to use as the column names (default: `0`).
- **`names`**: List of column names to use.
- **`index_col`**: Column to use as the index (can be a list for a MultiIndex).
- **`usecols`**: List of column names to read.
- **`nrows`**: Number of rows to read from the beginning of the file.
- **`skiprows`**: Number of rows to skip from the beginning of the file.
- **`parse_dates`**: Column(s) to parse as dates.

---

## Pandas .merge(), .concat() and .query() for joining, concatenating and selecting data from Tables

### df.merge()
- **`df1.merge(df2, on, how)`**: Merges DataFrames based on a common column.
#### Arguments
```python
df1.merge(
    df2                 # Other DataFram
    how='inner',         # Type of merge: 'inner', 'outer', 'left', 'right'
    on=None,            # Column(s) to merge on if same name in both DFs
    left_on=None,       # Column(s) from left DF to merge on
    right_on=None,      # Column(s) from right DF to merge on
    left_index=False,   # Use index from left DF as merge key
    right_index=False,  # Use index from right DF as merge key
    sort=False,         # Sort merged DataFrame by join keys
    suffixes=('_x', '_y'), # Suffixes for overlapping column names
    indicator=False,    # Add '_merge' column showing merge source
    validate=None      # Check merge integrity: '1:1', '1:m', 'm:1', 'm:m'
)
```
#### For ordered or time-series data
- **`pd.merge_ordered(df1, df2, on, how, fill_method='ffill')`**: Used to merge time-series dataframes. (default is outer join)
- **`pd.merge_asof(df1, df2, on, suffixes, direction='backward/forward')`**: Unlike an ordered left join, merge_asof() will match on the nearest value columns rather than equal values. This brings up an important point - whatever columns we merge on must be sorted.

### .concat()
- **`pd.concat([df1, df2], axis)`**: Concatenates DataFrames along rows or columns.
  
To concatenate DataFrames together **vertically** (axis=0)

- **`pd.concat([df1,df2,df3])`**: join 3 tables vertically
- **`pd.concat([df1,df2,df3], ignore_index=True)`**: If the index contains no valuable information, we can ignore it in the concat method by setting `ignore_index` to `True`. The result is that the index will go from 0 to n-1.
- **`pd.concat([df1,df2,df3], ignore_index=False, keys=['jan','feb','mar])`**: If we wanted to associate specific keys with each of the pieces of the 3 original tables. We can provide a list of labels to the keys argument. Make sure that ignore_index argument is False, since we can't add a key and ignore the index at the same time. This results in a table with a multi-index, with the label on the first level.
- **`pd.concat([df1,df2,df3], sort=True)`**: Concatenate tables with different column names. In the results the column which is not there in other tables is NaN.
- **`pd.concat([df1,df2,df3], join='inner')`**: If we only want the matching columns between tables, we can set the join argument to "inner". Its default value is equal to "outer", which is why concat by default will include all of the columns. Additionally, the sort argument has no effect when join equals "inner". The order of the columns will be the same as the input tables. 

```python
pd.concat(
    objs,               # List/dict of pandas objects to concatenate
    axis=0,            # 0/'index' for row-wise, 1/'columns' for column-wise
    join='outer',      # How to handle indexes: 'inner' or 'outer'
    ignore_index=False, # If True, don't use index values from objects
    keys=None,         # Create hierarchical index with these labels
    levels=None,       # Specific levels to use for hierarchical index
    names=None,        # Names for levels in hierarchical index
    verify_integrity=False, # Check new axis for duplicates
    sort=False,        # Sort non-concatenation axis if it is not aligned
    copy=True          # Copy data from objects to new DataFrame
)
```

### .query()
- **`df.query()`**: The query() method accepts an input string that it will use to select rows to return from the table. Similar to SQL, the string provided to the query function is similar to the portion after the WHERE clause of a SQL statement. 
- **`gdp_pivot.query('date >= "1991-01-01"')`**

## Essential DataFrame Functions

### Inspection
- **`df.head(n)`**: Returns the first `n` rows (default: 5).
- **`df.tail(n)`**: Returns the last `n` rows (default: 5).
- **`df.shape`**: Returns the number of rows and columns as a tuple.
- **`df.info()`**: Displays summary information about the DataFrame (data types, missing values, etc.).
- **`df.describe()`**: Generates descriptive statistics (count, mean, std, min, max, etc.).
- **`df.dtypes`**: Returns the data types of each column.

### Missing Data
- **`df.isnull()`**: Returns a boolean DataFrame indicating missing values.
- **`df.isna() | df.isna().any() | .isna().sum()`**: Returns True/False for NaNs. `any()` is applied along each column (by default) to check if there’s at least one NaN.
- **`df.fillna(value)`**: Fills missing values with a specified value or strategy.
- **`df.dropna(axis, how)`**: Drops rows or columns with missing values.
- **`df.drop_duplicates(subset=(['column1','column2']))`**: Removes duplicate rows from a DataFrame or duplicate values from a Series.

---

## Data Selection and Manipulation

### Selection
- **`df.loc[row, col]`**: Selects rows and columns by label.
- **`df.iloc[row, col]`**: Selects rows and columns by integer position.
- **`df['column']` or `df.column`**: Selects columns by name.
- **`df.filter()`**: Selects columns by name or regular expression.
- **`df['salary'].idxmax()`**: .idxmax() finds the **index of the row** where **salary** has the **maximum** value.

### Grouping
- **`df.groupby(by)`**: Groups data by one or more columns.
- **`df.agg(func)`**: Aggregates data within groups (e.g., mean, sum, count).
- **`df.transform(func)`**: Applies a function to each group and returns a DataFrame with the same shape.
- **`df.apply(func, axis)`**: Applies a function to each row or column.


### Pivot Table
- **`df.pivot_table(values, index, columns, margins=True/False, aggfunc)`**: In pandas, pivot tables are another way of performing grouped calculations. `fill_value` replaces missing values with a real value (known as imputation). `margins` is a shortcut for when pivoted by two variables, but also wanted to pivot by each of those variables separately: it gives the row and column totals of the pivot table contents.
- **`pivotdf.mean(axis="index/columns")`**: For most DataFrames, setting the axis argument doesn't make any sense, since we'll have different data types in each column. Pivot tables are a special case since every column contains the same data type.
- **`.melt()`**: The `.melt()` method in pandas is used to transform a DataFrame from a wide format to a long format.
#### **Syntax**
```python
DataFrame.melt(id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None, ignore_index=True)

**Parameters**
- id_vars (list, optional): Column(s) to keep as identifier variables (not melted).
- value_vars (list, optional): Column(s) to melt; defaults to all columns except id_vars.
- var_name (str, optional): Name for the melted variable column; defaults to "variable".
- value_name (str, optional, default="value"): Name for the melted values column.
- col_level (int or str, optional): If columns are multi-indexed, specifies which level to melt.
- ignore_index (bool, default=True): If False, retains the index.
```



---

## Data Cleaning and Transformation

- **`df.astype(dtype)`**: Converts columns to a specified data type.
- **`df.str.replace(old, new)`**: Replaces substrings within a string column.
- **`df.str.split(delimiter)`**: Splits a string column into a list of substrings.
- **`df.str.join(delimiter)`**: Joins elements of a list into a single string.
- **`pd.to_datetime(arg)`**: Converts strings to datetime objects.
- **`pd.to_numeric(arg)`**: Converts strings to numeric values.
- **`df["column"].dt.month | df["column"].dt.year`**: To extract month and year column from the date column in the dataframe

---

## Data Visualization

- **`df.plot(kind, ...)`**: Creates various types of plots (line, bar, scatter, etc.).
- **`df.hist()`**: Creates histograms.
- **`df.boxplot()`**: Creates box plots.

---

### Sorting and Counting
- **`df.sort_values(by, axis, ascending)`**: Sorts a DataFrame by one or more columns.
- **`df.value_counts()`**: Counts the occurrences of unique values in a Series.
- **`df.sort_index()`**: The .sort_index() method is used to sort a pandas DataFrame or Series by its index instead of column values.
- **`df.set_index()`**: It converts one or more columns into the index of the DataFrame.
- **`df.reset_index()`**: This restores the previous index as a column and creates a default integer index. If you want to discard the old index, use `drop=True`


### Cumulative Statistics
- **`df['Salary'].cumsum()`**: Calculates the cumulative sum of values in a DataFrame or Series.
- `df['Salary'].cummin()`**
- `df['Salary'].cummax()`**

## Pandas String Matching

| Method | What it does | Example |
|:---|:---|:---|
| `.str.lower()` | Converts all text to lowercase | `"HELLO"` → `"hello"` |
| `.str.upper()` | Converts all text to uppercase | `"hello"` → `"HELLO"` |
| `.str.title()` | Capitalizes first letter of each word | `"hello world"` → `"Hello World"` |
| `.str.strip()` | Removes spaces from start and end | `"  hello "` → `"hello"` |
| `.str.lstrip()` | Removes spaces from start only | `"  hello "` → `"hello "` |
| `.str.rstrip()` | Removes spaces from end only | `"  hello "` → `"  hello"` |
| `.str.contains('pattern')` | Checks if string contains pattern (returns True/False) | `"hello world"` contains `"hello"` → `True` |
| `.str.replace('a', 'b')` | Replace text | `"cat"` → `"cbt"` |
| `.str.startswith('pattern')` | Check if string starts with pattern | `"hello"` starts with `"he"` → `True` |
| `.str.endswith('pattern')` | Check if string ends with pattern | `"hello"` ends with `"lo"` → `True` |
| `.str.len()` | Gives the length of each string | `"hello"` → `5` |
| `.str.split(' ')` | Splits strings into a list based on separator | `"a b c"` → `["a", "b", "c"]` |
| `.str.get(i)` | Get the ith element from a split string/list | After split: get first word, second word, etc. |
| `.str.extract('regex')` | Extract groups based on a regex pattern | Pull specific parts of the text |
| `.str.find('pattern')` | Finds position of pattern (-1 if not found) | `"hello".find("e")` → `1` |
| `.str.isnumeric()` | Checks if string is numeric | `"123"` → `True` |
| `.str.isalpha()` | Checks if string has only letters | `"abc"` → `True` |
| `.str.zfill(width)` | Pads string on the left with zeros | `"42".zfill(5)` → `"00042"` |


### 
This markdown file covers common arguments, functions, and scenarios for using Pandas effectively in Python.



