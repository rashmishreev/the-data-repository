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

### Grouping
- **`df.groupby(by)`**: Groups data by one or more columns.
- **`df.agg(func)`**: Aggregates data within groups (e.g., mean, sum, count).
- **`df.transform(func)`**: Applies a function to each group and returns a DataFrame with the same shape.
- **`df.apply(func, axis)`**: Applies a function to each row or column.
- **`df.pivot_table(values, index, columns, margins=True/False, aggfunc)`**: In pandas, pivot tables are another way of performing grouped calculations. `fill_value` replaces missing values with a real value (known as imputation). `margins` is a shortcut for when pivoted by two variables, but also wanted to pivot by each of those variables separately: it gives the row and column totals of the pivot table contents.

---

## Data Cleaning and Transformation

- **`df.astype(dtype)`**: Converts columns to a specified data type.
- **`df.str.replace(old, new)`**: Replaces substrings within a string column.
- **`df.str.split(delimiter)`**: Splits a string column into a list of substrings.
- **`df.str.join(delimiter)`**: Joins elements of a list into a single string.
- **`pd.to_datetime(arg)`**: Converts strings to datetime objects.
- **`pd.to_numeric(arg)`**: Converts strings to numeric values.

---

## Data Visualization

- **`df.plot(kind, ...)`**: Creates various types of plots (line, bar, scatter, etc.).
- **`df.hist()`**: Creates histograms.
- **`df.boxplot()`**: Creates box plots.

---

### Merging and Concatenation
- **`pd.merge(left, right, on, how)`**: Merges DataFrames based on a common column.
- **`pd.concat([df1, df2], axis)`**: Concatenates DataFrames along rows or columns.

### Sorting and Counting
- **`df.sort_values(by, axis, ascending)`**: Sorts a DataFrame by one or more columns.
- **`df.value_counts()`**: Counts the occurrences of unique values in a Series.

### Cumulative Statistics
- **`df['Salary'].cumsum()`**: Calculates the cumulative sum of values in a DataFrame or Series.
- `df['Salary'].cummin()`**
- `df['Salary'].cummax()`**

This markdown file covers common arguments, functions, and scenarios for using Pandas effectively in Python.

