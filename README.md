
# 30 Days of Pandas – Solutions

This file lists all 30 problems from LeetCode's "30 Days of Pandas" study plan. For each day:

- **Question**: The problem statement.
- **Solution**: Clean pandas code.
- **Reasoning**: Textual explanation of each step.
- **Documentation**: Descriptions of the pandas (or numpy) functions used.

### Day 1: Second Highest Salary (LeetCode 176)
**Question:**
Given a DataFrame `employee` with columns `id` and `salary`, return the second highest salary or None if it doesn't exist.

**Solution:**
```python
import pandas as pd

def second_highest_salary(employee: pd.DataFrame) -> pd.DataFrame:
    salaries = employee['salary'].drop_duplicates().sort_values(ascending=False)
    second = salaries.iloc[1] if len(salaries) >= 2 else None
    return pd.DataFrame({'SecondHighestSalary': [second]})

```

**Reasoning:**

1.  Remove duplicate salary values to consider unique salaries only.
    
2.  Sort the salaries in descending order so the highest appear first.
    
3.  Select the second element if at least two exist; otherwise, return None.
    

**Documentation:**

-   `pandas.Series.drop_duplicates()`: Returns Series without duplicate values.
    
-   `pandas.Series.sort_values(ascending=False)`: Sorts Series values in descending order.
    
-   `Series.iloc`: Integer-location based indexing to select by position.
    

----------

### Day 2: Nth Highest Salary (LeetCode 177)

**Question:**  
Given `employee` DataFrame and an integer `n`, return the nth highest salary or None if it doesn't exist.

**Solution:**

```python
import pandas as pd

def nth_highest_salary(employee: pd.DataFrame, n: int) -> pd.DataFrame:
    salaries = employee['salary'].drop_duplicates().sort_values(ascending=False)
    nth = salaries.iloc[n-1] if len(salaries) >= n else None
    return pd.DataFrame({'NthHighestSalary': [nth]})

```

**Reasoning:**

1.  Remove duplicate salaries to get distinct values.
    
2.  Sort descending to list highest salaries first.
    
3.  Use zero-based indexing to pick the (n-1)th element if available; else return None.
    

**Documentation:**

-   `pandas.Series.drop_duplicates()`: Drops duplicate entries in the Series.
    
-   `pandas.Series.sort_values()`: Orders values; descending when `ascending=False`.
    
-   `Series.iloc`: Access element by integer position.
    

----------

### Day 3: Rank Scores (LeetCode 178)

**Question:**  
Given a `scores` DataFrame with columns `student` and `score`, assign a dense rank based on score descending.

**Solution:**

```python
import pandas as pd

def rank_scores(scores: pd.DataFrame) -> pd.DataFrame:
    df = scores.copy()
    df['rank'] = df['score'].rank(method='dense', ascending=False).astype(int)
    return df[['student', 'score', 'rank']]

```

**Reasoning:**

1.  Copy the DataFrame to avoid mutating the original.
    
2.  Apply a dense ranking on the `score` column: highest score gets rank 1, ties share rank, and next distinct score increments by 1.
    
3.  Convert the resulting float ranks to integers.
    

**Documentation:**

-   `DataFrame.copy()`: Creates a deep copy of the DataFrame.
    
-   `pandas.Series.rank(method='dense', ascending=False)`: Assigns ranks with no gaps in ranking sequence.
    
-   `Series.astype(int)`: Casts Series data to integer type.
    

----------

### Day 4: Customers Who Never Order (LeetCode 183)

**Question:**  
Given DataFrames `customers` (with `id`) and `orders` (with `customer_id`), find customers who have no orders.

**Solution:**

```python
import pandas as pd

def customers_without_orders(customers: pd.DataFrame, orders: pd.DataFrame) -> pd.DataFrame:
    ordered_ids = orders['customer_id'].unique()
    return customers[~customers['id'].isin(ordered_ids)]

```

**Reasoning:**

1.  Extract the array of unique `customer_id` values from the orders DataFrame.
    
2.  Filter the customers DataFrame to rows whose `id` is not in that array.
    

**Documentation:**

-   `Series.unique()`: Returns unique values of the Series.
    
-   `Series.isin(values)`: Boolean mask indicating membership of Series values in given list/array.
    
-   Boolean indexing: Filters DataFrame rows where mask is True.
    

----------

### Day 5: Department Highest Salary (LeetCode 184)

**Question:**  
Given an `employee` DataFrame with columns `department` and `salary`, find each department's maximum salary.

**Solution:**

```python
import pandas as pd

def department_highest_salary(employee: pd.DataFrame) -> pd.DataFrame:
    result = employee.groupby('department')['salary'].max().reset_index()
    result.rename(columns={'salary': 'HighestSalary'}, inplace=True)
    return result

```

**Reasoning:**

1.  Group the employee DataFrame by `department`.
    
2.  Compute the maximum `salary` within each group.
    
3.  Reset the index to turn the grouped keys back into columns and rename the salary column.
    

**Documentation:**

-   `DataFrame.groupby('col')`: Groups rows by unique values in `col`.
    
-   `Series.max()`: Returns the maximum value.
    
-   `DataFrame.reset_index()`: Converts index levels into columns.
    
-   `DataFrame.rename()`: Renames columns.
    

----------

### Day 6: Delete Duplicate Emails (LeetCode 196)

**Question:**  
Given a DataFrame `person` with columns `id` and `email`, delete duplicate emails so only the row with the smallest `id` remains.

**Solution:**

```python
import pandas as pd

def delete_duplicate_emails(person: pd.DataFrame) -> pd.DataFrame:
    df = person.sort_values(by='id')
    return df.drop_duplicates(subset=['email'], keep='first')

```

**Reasoning:**

1.  Sort rows by `id` ascending so the smallest id appears first for each email.
    
2.  Drop duplicate rows based on the `email` column, keeping the first occurrence.
    

**Documentation:**

-   `DataFrame.sort_values(by='col')`: Sorts rows by column values.
    
-   `DataFrame.drop_duplicates(subset=[...], keep='first')`: Removes duplicate rows based on specified columns, keeping the first.
    

----------

### Day 7: Game Play Analysis I (LeetCode 511)

**Question:**  
Given an `activity` DataFrame with columns `player_id` and `event_date`, find each player's first login date.

**Solution:**

```python
import pandas as pd

def game_play_analysis(activity: pd.DataFrame) -> pd.DataFrame:
    df = activity.groupby('player_id')['event_date'].min().reset_index()
    df.rename(columns={'event_date': 'first_login'}, inplace=True)
    return df

```

**Reasoning:**

1.  Group by `player_id`.
    
2.  Compute the minimum `event_date` per player, representing first login.
    
3.  Rename the resulting column for clarity.
    

**Documentation:**

-   `groupby('col')`: Groups rows by column.
    
-   `Series.min()`: Returns minimum value.
    
-   `reset_index()`: Converts group keys to columns.
    
-   `rename()`: Renames columns.
    

----------

### Day 8: Managers with at Least 5 Direct Reports (LeetCode 570)

**Question:**  
Given an `employee` DataFrame with columns `id`, `name`, and `managerId`, find the names of managers who have at least five direct reports.

**Solution:**

```python
import pandas as pd

def managers_with_direct_reports(employee: pd.DataFrame) -> pd.DataFrame:
    counts = employee.groupby('managerId')['id'].count().reset_index(name='report_count')
    managers = counts[counts['report_count'] >= 5]['managerId']
    df = employee[employee['id'].isin(managers)][['id', 'name']].drop_duplicates()
    return df[['name']]

```

**Reasoning:**

1.  Group by `managerId` and count their direct reports.
    
2.  Filter managers with at least five reports.
    
3.  Retrieve the unique names of those managers.
    

**Documentation:**

-   `groupby()`: Groups rows by specified columns.
    
-   `count()`: Counts non-null observations.
    
-   `reset_index(name=…)`: Converts group keys into columns with a named aggregation column.
    
-   `isin()`: Filters rows based on membership.
    
-   `drop_duplicates()`: Removes duplicate rows.
    

----------

### Day 9: Customer Placing the Largest Number of Orders (LeetCode 586)

**Question:**  
Given an `orders` DataFrame with columns `order_number` and `customer_number`, find the customer who placed the largest number of orders.

**Solution:**

```python
import pandas as pd

def customer_with_most_orders(orders: pd.DataFrame) -> pd.DataFrame:
    counts = orders.groupby('customer_number')['order_number'].count()
    top = counts.idxmax()
    return pd.DataFrame({'customer_number': [top]})

```

**Reasoning:**

1.  Count orders per customer by grouping and counting.
    
2.  Identify the customer_number with the highest count.
    

**Documentation:**

-   `groupby()`: Groups rows.
    
-   `count()`: Tallies non-null entries.
    
-   `idxmax()`: Returns the index of the first occurrence of the maximum value.
    

----------

### Day 10: Big Countries (LeetCode 595)

**Question:**  
Given a `world` DataFrame with columns `name`, `continent`, `area`, `population`, and `gdp`, find all countries with `area >= 3000000` or `population >= 25000000`.

**Solution:**

```python
import pandas as pd

def big_countries(world: pd.DataFrame) -> pd.DataFrame:
    df = world[(world['area'] >= 3000000) | (world['population'] >= 25000000)]
    return df[['name', 'continent', 'area', 'population']]

```

**Reasoning:**

1.  Apply boolean filtering for area or population thresholds.
    
2.  Select relevant columns for output.
    

**Documentation:**

-   Boolean indexing with conditions.
    
-   Column selection by list of column names.
    

----------

### Day 11: Classes More Than 5 Students (LeetCode 596)

**Question:**  
Given a `courses` DataFrame with columns `student` and `class`, list classes that have more than 5 distinct students.

**Solution:**

```python
import pandas as pd

def classes_more_than_five(courses: pd.DataFrame) -> pd.DataFrame:
    counts = courses.groupby('class')['student'].nunique().reset_index(name='student_count')
    df = counts[counts['student_count'] > 5]
    return df[['class']]

```

**Reasoning:**

1.  Group by class and count unique students.
    
2.  Filter classes where the count exceeds five.
    

**Documentation:**

-   `nunique()`: Counts distinct values.
    
-   `reset_index(name=…)`: Converts grouped indices to columns with named count.
    

----------

### Day 12: Sales Person (LeetCode 607)

**Question:**  
Given a `sales` DataFrame with columns `sales_person` and `amount`, find the salesperson with the highest total sales amount.

**Solution:**

```python
import pandas as pd

def top_salesperson(sales: pd.DataFrame) -> pd.DataFrame:
    totals = sales.groupby('sales_person')['amount'].sum()
    top = totals.idxmax()
    return pd.DataFrame({'sales_person': [top]})

```

**Reasoning:**

1.  Sum the amount per salesperson.
    
2.  Identify the salesperson with the maximum total.
    

**Documentation:**

-   `sum()`: Adds values in each group.
    
-   `idxmax()`: Finds index of maximum.
    

----------

### Day 13: Actors and Directors Who Cooperated At Least Three Times (LeetCode 1050)

**Question:**  
Given a `production` DataFrame with columns `actor_id` and `director_id`, find all pairs that have worked together three or more times.

**Solution:**

```python
import pandas as pd

def frequent_pairs(production: pd.DataFrame) -> pd.DataFrame:
    counts = production.groupby(['actor_id', 'director_id']).size().reset_index(name='times')
    df = counts[counts['times'] >= 3]
    return df[['actor_id', 'director_id']]

```

**Reasoning:**

1.  Group by both actor and director IDs and count occurrences.
    
2.  Filter pairs with counts of at least three.
    

**Documentation:**

-   `size()`: Returns group sizes.
    
-   Boolean filtering on the times column.
    

----------

### Day 14: Article Views I (LeetCode 1148)

**Question:**  
Given a `views` DataFrame with columns `article_id`, `author_id`, and `viewer_id`, identify authors who have viewed their own articles.

**Solution:**

```python
import pandas as pd

def self_viewing_authors(views: pd.DataFrame) -> pd.DataFrame:
    df = views[views['author_id'] == views['viewer_id']]
    df = df[['author_id']].drop_duplicates().rename(columns={'author_id': 'id'})
    return df.sort_values('id').reset_index(drop=True)

```

**Reasoning:**

1.  Filter rows where `author_id` equals `viewer_id`.
    
2.  Select and dedupe the `author_id` column, renaming it to `id`.
    
3.  Sort and reset the index for neatness.
    

**Documentation:**

-   Equality comparison for boolean indexing.
    
-   `drop_duplicates()`: Removes duplicate rows.
    
-   `rename()`: Changes column names.
    
-   `sort_values()`: Orders rows by column.
    

----------

### Day 15: Immediate Food Delivery I (LeetCode 1173)

**Question:**  
Given a `delivery` DataFrame with columns `order_date` and `customer_pref_delivery_date`, compute the percentage of orders delivered immediately (same day).

**Solution:**

```python
import pandas as pd

def immediate_delivery_percentage(delivery: pd.DataFrame) -> pd.DataFrame:
    total = len(delivery)
    immediate = (delivery['order_date'] == delivery['customer_pref_delivery_date']).sum()
    percent = round(immediate / total * 100, 2)
    return pd.DataFrame({'immediate_percentage': [percent]})

```

**Reasoning:**

1.  Count total orders.
    
2.  Count orders where the order date matches the preferred delivery date.
    
3.  Calculate the percentage and round to two decimals.
    

**Documentation:**

-   Comparison operator on Series returns boolean Series.
    
-   `sum()` on boolean Series counts True values.
    
-   `round()` for numeric rounding.
    

----------

### Day 16: Students and Examinations (LeetCode 1280)

**Question:**  
Given a `scores` DataFrame with columns `student` and `score`, return students who scored above the average.

**Solution:**

```python
import pandas as pd

def above_average_students(scores: pd.DataFrame) -> pd.DataFrame:
    avg = scores['score'].mean()
    return scores[scores['score'] > avg]

```

**Reasoning:**

1.  Compute the average score.
    
2.  Filter rows where the score exceeds that average.
    

**Documentation:**

-   `mean()`: Computes the average of Series.
    
-   Boolean indexing for filtering.
    

----------

### Day 17: Replace Employee ID With The Unique Identifier (LeetCode 1378)

**Question:**  
Given an `employee` DataFrame with columns `id` and `name`, create a new column `unique_id` by concatenating `name` and `id` with an underscore.

**Solution:**

```python
import pandas as pd

def replace_employee_id(employee: pd.DataFrame) -> pd.DataFrame:
    employee['unique_id'] = employee['name'] + '_' + employee['id'].astype(str)
    return employee[['unique_id']]

```

**Reasoning:**

1.  Convert `id` to string and concatenate with `name`.
    
2.  Return only the new column.
    

**Documentation:**

-   `astype(str)`: Casts values to string.
    
-   String concatenation on Series.
    

----------

### Day 18: Group Sold Products By The Date (LeetCode 1484)

**Question:**  
Given a `sales` DataFrame with columns `product`, `date`, and `amount`, compute total `amount` sold per product per date.

**Solution:**

```python
import pandas as pd

def group_sold_products(sales: pd.DataFrame) -> pd.DataFrame:
    df = sales.groupby(['date', 'product'])['amount'].sum().reset_index()
    return df

```

**Reasoning:**

1.  Group by both `date` and `product`.
    
2.  Sum the `amount` in each group.
    
3.  Reset index to flatten the grouped DataFrame.
    

**Documentation:**

-   `groupby()` with multiple keys.
    
-   Aggregation with `sum()`.
    
-   `reset_index()`.
    

----------

### Day 19: Find Users With Valid E-Mails (LeetCode 1517)

**Question:**  
Given a `users` DataFrame with column `email`, return rows where `email` matches a simple regex pattern.

**Solution:**

```python
import pandas as pd
import re

def valid_emails(users: pd.DataFrame) -> pd.DataFrame:
    pattern = r'^[A-Za-z\d]+@[A-Za-z\d]+\.[A-Za-z]{2,}$'
    return users[users['email'].str.contains(pattern, regex=True)]

```

**Reasoning:**

1.  Define a regex for basic email validation.
    
2.  Use string contains to filter rows matching the pattern.
    

**Documentation:**

-   `str.contains()`: Tests each string for pattern match.
    
-   Regular expression basics.
    

----------

### Day 20: Patients With a Condition (LeetCode 1527)

**Question:**  
Given a `patients` DataFrame with columns `patient_id` and `condition`, return rows where `condition` equals a given string.

**Solution:**

```python
import pandas as pd

def patients_with_condition(patients: pd.DataFrame, cond: str) -> pd.DataFrame:
    return patients[patients['condition'] == cond]

```

**Reasoning:**

1.  Compare the `condition` column to the target string.
    
2.  Filter and return matching rows.
    

**Documentation:**

-   Boolean indexing for equality comparison.
    

----------

### Day 21: Fix Names in a Table (LeetCode 1667)

**Question:**  
Given a `users` DataFrame with column `name`, capitalize the first letter and lowercase the rest.

**Solution:**

```python
import pandas as pd

def fix_names(users: pd.DataFrame) -> pd.DataFrame:
    users['name'] = users['name'].str.capitalize()
    return users

```

**Reasoning:**

1.  Use the string capitalize method to adjust casing.
    
2.  Return the modified DataFrame.
    

**Documentation:**

-   `str.capitalize()`: Capitalizes first character and lowers the rest.
    

----------

### Day 22: Invalid Tweets (LeetCode 1683)

**Question:**  
Given a `tweets` DataFrame with column `tweet` and a list of banned words, return tweets containing any banned word.

**Solution:**

```python
import pandas as pd
import re

def invalid_tweets(tweets: pd.DataFrame, banned: list) -> pd.DataFrame:
    pattern = '|'.join(map(re.escape, banned))
    return tweets[tweets['tweet'].str.contains(pattern, regex=True)]

```

**Reasoning:**

1.  Escape banned words and join them into a regex OR pattern.
    
2.  Filter tweets containing any of those words.
    

**Documentation:**

-   `re.escape()`: Escapes special regex characters.
    
-   `str.contains()`: Filters strings by regex match.
    

----------

### Day 23: Daily Leads and Partners (LeetCode 1693)

**Question:**  
Given a `leads` DataFrame with columns `partner` and `date`, count leads per partner per day.

**Solution:**

```python
import pandas as pd

def daily_leads(leads: pd.DataFrame) -> pd.DataFrame:
    df = leads.groupby(['partner', 'date'])['lead_id'].count().reset_index(name='lead_count')
    return df

```

**Reasoning:**

1.  Group by both `partner` and `date`.
    
2.  Count the `lead_id` entries in each group.
    
3.  Reset index and name the count column.
    

**Documentation:**

-   `count()` on grouped Series.
    
-   `reset_index(name=…)`.
    

----------

### Day 24: Find Total Time Spent by Each Employee (LeetCode 1741)

**Question:**  
Given an `employees` DataFrame with columns `emp_id`, `in_time`, and `out_time`, compute total time spent per employee.

**Solution:**

```python
import pandas as pd

def total_time_spent(employees: pd.DataFrame) -> pd.DataFrame:
    employees['duration'] = employees['out_time'] - employees['in_time']
    df = employees.groupby('emp_id')['duration'].sum().reset_index(name='total_time')
    return df

```

**Reasoning:**

1.  Compute duration for each row by subtracting datetime columns.
    
2.  Group by `emp_id` and sum durations.
    
3.  Reset index and name the aggregated column.
    

**Documentation:**

-   Datetime subtraction yields timedelta.
    
-   `sum()` on timedelta Series.
    
-   `reset_index(name=…)`.
    

----------

### Day 25: Recyclable and Low Fat Products (LeetCode 1757)

**Question:**  
Given a `products` DataFrame with columns `product_id`, `low_fats`, and `recyclable`, find products where both flags are 'Y'.

**Solution:**

```python
import pandas as pd

def find_products(products: pd.DataFrame) -> pd.DataFrame:
    return products[(products['low_fats'] == 'Y') & (products['recyclable'] == 'Y')][['product_id']]

```

**Reasoning:**

1.  Apply boolean mask for both conditions.
    
2.  Select the `product_id` column.
    

**Documentation:**

-   Boolean operators on Series.
    
-   Column selection.
    

----------

### Day 26: Rearrange Products Table (LeetCode 1795)

**Question:**  
Given a wide-format `products` DataFrame with columns `product_id`, `store1_price`, `store2_price`, etc., reshape into long format.

**Solution:**

```python
import pandas as pd

def rearrange_products(products: pd.DataFrame) -> pd.DataFrame:
    return pd.melt(products, id_vars=['product_id'], value_vars=[col for col in products.columns if col.endswith('_price')], var_name='store', value_name='price')

```

**Reasoning:**

1.  Identify price columns by naming convention.
    
2.  Use melt to reshape from wide to long.
    

**Documentation:**

-   `pandas.melt()`: Converts wide tables to long format.
    

----------

### Day 27: Calculate Special Bonus (LeetCode 1873)

**Question:**  
Given an `employee` DataFrame with columns `salary` and `bonus`, add `special_bonus` = 1.0 if `bonus > salary * 0.03`, else 0.0.

**Solution:**

```python
import pandas as pd
import numpy as np

def calculate_special_bonus(employee: pd.DataFrame) -> pd.DataFrame:
    employee['special_bonus'] = np.where(employee['bonus'] > employee['salary'] * 0.03, 1.0, 0.0)
    return employee

```

**Reasoning:**

1.  Compute the condition on salary and bonus.
    
2.  Use numpy.where for vectorized assignment.
    

**Documentation:**

-   `numpy.where()`: Vectorized conditional selection.
    

----------

### Day 28: Count Salary Categories (LeetCode 1907)

**Question:**  
Given an `employee` DataFrame with column `salary`, categorize into bins and count each category.

**Solution:**

```python
import pandas as pd
import numpy as np

def count_salary_categories(employee: pd.DataFrame) -> pd.DataFrame:
    bins = [0, 10000, 20000, 30000, np.inf]
    labels = ['Low', 'Medium', 'High', 'Very High']
    employee['category'] = pd.cut(employee['salary'], bins=bins, labels=labels)
    return employee['category'].value_counts().reset_index(name='count').rename(columns={'index': 'category'})

```

**Reasoning:**

1.  Define numerical bins and labels.
    
2.  Use pd.cut to assign each salary to a category.
    
3.  Count occurrences of each category.
    

**Documentation:**

-   `pd.cut()`: Bins continuous data into discrete intervals.
    
-   `value_counts()`: Counts unique values.
    
-   `reset_index(name=…)`: Formats counts into DataFrame.
    

----------

### Day 29: The Number of Rich Customers (LeetCode 2082)

**Question:**  
Given a `customer` DataFrame with columns `id`, `income`, and `threshold`, return the number of customers with income greater than threshold.

**Solution:**

```python
import pandas as pd

def rich_customers(customer: pd.DataFrame) -> pd.DataFrame:
    count = (customer['income'] > customer['threshold']).sum()
    return pd.DataFrame({'rich_customers': [count]})

```

**Reasoning:**

1.  Compare `income` and `threshold` to create a boolean Series.
    
2.  Sum True values to count.
    

**Documentation:**

-   Boolean comparison on Series.
    
-   `sum()` on boolean Seriescounts True values.
    

----------

### Day 30: Number of Unique Subjects Taught by Each Teacher (LeetCode 2356)

**Question:**  
Given a `teacher` DataFrame with columns `teacher_id` and `subject_id`, count unique subjects per teacher.

**Solution:**

```python
import pandas as pd

def count_unique_subjects(teacher: pd.DataFrame) -> pd.DataFrame:
    return teacher.groupby('teacher_id')['subject_id'].nunique().reset_index(name='cnt')

```

**Reasoning:**

1.  Group by `teacher_id`.
    
2.  Count distinct `subject_id` per group.
    
3.  Reset index with appropriate column name.
    

**Documentation:**

-   `groupby()`: Groups rows.
    
-   `nunique()`: Counts unique values.
    
-   `reset_index(name=…)`.
