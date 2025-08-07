# 30 Days of Pandas – Study Guide

This comprehensive study guide covers the "30 Days of Pandas" challenge, providing structured solutions and explanations for data manipulation problems using pandas. Each problem includes complete analysis, solution code, and detailed reasoning.

## 📚 Study Plan Overview

The 30 Days of Pandas challenge covers essential data manipulation concepts:
- Data Filtering & Selection
- Data Transformation
- Aggregation & Grouping
- Joining & Merging
- String Operations
- Date/Time Manipulation
- Advanced Data Analysis

## 📋 Template Structure

Each problem follows this format:
- **Problem Description**: Clear explanation of the task
- **Table Schema**: Input data structure in markdown tables
- **Solution**: Complete pandas implementation
- **Reasoning**: Step-by-step explanation
- **Key Concepts**: Functions and methods used
- **Documentation**: Links to relevant pandas documentation

---

## Day 1: Data Filtering

### Problem: Big Countries
**Difficulty**: Easy

**Description**: 
Find countries that are considered "big" based on area or population criteria.

**Table Schema**:
```sql
Table: World
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |
+-------------+---------+
```

**Sample Data**:
| name        | continent | area    | population | gdp          |
|-------------|-----------|---------|------------|--------------|
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |

**Solution**:
```python
import pandas as pd

def big_countries(world: pd.DataFrame) -> pd.DataFrame:
    # Filter countries that are big by area (>= 3000000) OR population (>= 25000000)
    big_countries_df = world[
        (world['area'] >= 3000000) | (world['population'] >= 25000000)
    ]
    
    # Select only required columns
    result = big_countries_df[['name', 'population', 'area']]
    
    return result
```

**Reasoning**:
1. **Filtering Logic**: Use boolean indexing with OR condition `|` to find countries meeting either criteria
2. **Column Selection**: Extract only the required columns using bracket notation
3. **Conditional Operators**: `>=` for numerical comparisons, `|` for logical OR

**Key Concepts**:
- `pd.DataFrame[]` - Boolean indexing for filtering
- `|` - Logical OR operator for multiple conditions  
- Column selection with list of column names

**Documentation**:
- [Boolean Indexing](https://pandas.pydata.org/docs/user_guide/indexing.html#boolean-indexing)
- [Comparison Operators](https://pandas.pydata.org/docs/reference/ops.html)

---

## Day 2: Data Selection

### Problem: Recyclable and Low Fat Products
**Difficulty**: Easy

**Description**: 
Find products that are both low fat and recyclable.

**Table Schema**:
```sql
Table: Products
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |
+-------------+---------+
```

**Solution**:
```python
import pandas as pd

def find_products(products: pd.DataFrame) -> pd.DataFrame:
    # Filter products that are both low fat AND recyclable
    filtered_products = products[
        (products['low_fats'] == 'Y') & (products['recyclable'] == 'Y')
    ]
    
    # Return only product_id column
    result = filtered_products[['product_id']]
    
    return result
```

**Reasoning**:
1. **Multiple Conditions**: Use `&` (AND) operator to combine two conditions
2. **String Comparison**: Direct equality comparison with string values
3. **Single Column Selection**: Return DataFrame with one column using list notation

**Key Concepts**:
- `&` - Logical AND operator
- String equality comparison in pandas
- Single column DataFrame selection

**Documentation**:
- [Boolean Indexing](https://pandas.pydata.org/docs/user_guide/indexing.html#boolean-indexing)
- [Logical Operators](https://pandas.pydata.org/docs/user_guide/boolean.html)
- [DataFrame Selection](https://pandas.pydata.org/docs/user_guide/indexing.html#selection-by-label)

---

## Day 3: String Methods

### Problem: Find Customer Referee
**Difficulty**: Easy

**Description**: 
Find customers who were not referred by customer with id = 2.

**Solution**:
```python
import pandas as pd

def find_customer_referee(customer: pd.DataFrame) -> pd.DataFrame:
    # Find customers NOT referred by customer id 2
    # Handle both null values and id != 2
    result = customer[
        (customer['referee_id'] != 2) | (customer['referee_id'].isna())
    ]
    
    return result[['name']]
```

**Reasoning**:
1. **Null Handling**: Use `isna()` to check for null/NaN values
2. **Negation Logic**: Use `!=` for "not equal" comparison
3. **OR Logic**: Include both null values and non-matching IDs

**Key Concepts**:
- `isna()` - Check for missing values
- `!=` - Not equal comparison
- Handling null values in filtering

**Documentation**:
- [Working with missing data](https://pandas.pydata.org/docs/user_guide/missing_data.html)
- [pandas.isna()](https://pandas.pydata.org/docs/reference/api/pandas.isna.html)
- [Comparison Operations](https://pandas.pydata.org/docs/reference/ops.html)

---

## Day 4: Data Transformation

### Problem: Article Views I
**Difficulty**: Easy

**Description**: 
Find all the authors who viewed at least one of their own articles. Return the result table sorted by id in ascending order.

**Table Schema**:
```sql
Table: Views
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+
```

**Sample Data**:
| article_id | author_id | viewer_id | view_date  |
|------------|-----------|-----------|------------|
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |

**Expected Output**:
| id |
|----|
| 4  |
| 7  |

**Solution**:
```python
import pandas as pd

def article_views(views: pd.DataFrame) -> pd.DataFrame:
    # Find rows where author_id equals viewer_id (authors viewing their own articles)
    self_views = views[views['author_id'] == views['viewer_id']]
    
    # Get unique author_ids and sort them
    unique_authors = self_views['author_id'].drop_duplicates().sort_values()
    
    # Create result DataFrame with renamed column
    result = pd.DataFrame({'id': unique_authors})
    
    return result
```

**Alternative Solution using unique():**
```python
import pandas as pd

def article_views(views: pd.DataFrame) -> pd.DataFrame:
    # Filter for self-views and get unique sorted author IDs
    self_viewing_authors = views[views['author_id'] == views['viewer_id']]['author_id'].unique()
    
    # Sort and create result DataFrame
    result = pd.DataFrame({'id': sorted(self_viewing_authors)})
    
    return result
```

**Reasoning**:
1. **Self-Reference Detection**: Compare `author_id` and `viewer_id` to find authors viewing their own content
2. **Deduplication**: Use `drop_duplicates()` or `unique()` to get distinct authors
3. **Sorting**: Use `sort_values()` or `sorted()` for ascending order
4. **DataFrame Creation**: Build new DataFrame with properly named column
5. **Multiple Approaches**: Show different ways to achieve the same result

**Key Concepts**:
- Column-to-column comparison within same DataFrame
- `drop_duplicates()` vs `unique()` for deduplication
- `sort_values()` vs `sorted()` for ordering
- DataFrame creation and column naming
- Self-referential data analysis

**Performance Notes**:
- `unique()` + `sorted()` can be more memory efficient for large datasets
- `drop_duplicates()` + `sort_values()` preserves pandas Series structure
- Both approaches are valid and perform similarly for most use cases

**Documentation**:
- [pandas.DataFrame.drop_duplicates](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html)
- [pandas.unique](https://pandas.pydata.org/docs/reference/api/pandas.unique.html)
- [pandas.DataFrame.sort_values](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_values.html)
- [DataFrame Creation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)

---

## Day 5: Data Aggregation

### Problem: Invalid Tweets
**Difficulty**: Easy

**Description**: 
Find tweet IDs where the content is longer than 15 characters.

**Table Schema**:
```sql
Table: Tweets
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| tweet_id       | int     |
| content        | varchar |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def invalid_tweets(tweets: pd.DataFrame) -> pd.DataFrame:
    # Filter tweets with content longer than 15 characters
    invalid = tweets[tweets['content'].str.len() > 15]
    
    # Return only tweet_id column
    return invalid[['tweet_id']]
```

**Reasoning**:
1. **String Length**: Use `.str.len()` to get character count of string column
2. **Numerical Comparison**: Apply `>` operator to length values
3. **Column Selection**: Extract only the required tweet_id column

**Key Concepts**:
- `.str.len()` - Get string length in pandas
- String accessor methods with `.str`
- Length-based filtering

**Documentation**:
- [String Methods](https://pandas.pydata.org/docs/user_guide/text.html)
- [pandas.Series.str.len](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.len.html)
- [String Accessor](https://pandas.pydata.org/docs/reference/series.html#string-handling)

---

## Day 6: String Operations

### Problem: Calculate Special Bonus
**Difficulty**: Easy

**Description**: 
Calculate bonus for employees based on conditions: bonus = salary if employee_id is odd and name doesn't start with 'M', otherwise bonus = 0.

**Table Schema**:
```sql
Table: Employees
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| name        | varchar |
| salary      | int     |
+-------------+---------+
```

**Solution**:
```python
import pandas as pd

def calculate_special_bonus(employees: pd.DataFrame) -> pd.DataFrame:
    # Create bonus column based on conditions
    employees['bonus'] = employees.apply(
        lambda row: row['salary'] 
        if (row['employee_id'] % 2 == 1) and (not row['name'].startswith('M'))
        else 0, 
        axis=1
    )
    
    # Return employee_id and bonus, sorted by employee_id
    result = employees[['employee_id', 'bonus']].sort_values('employee_id')
    
    return result
```

**Reasoning**:
1. **Conditional Logic**: Use `apply()` with lambda for complex conditional assignment
2. **Modulo Operation**: `% 2 == 1` checks for odd numbers
3. **String Methods**: `startswith()` for string pattern matching
4. **Sorting**: `sort_values()` to order results

**Key Concepts**:
- `apply()` with lambda functions
- `startswith()` string method
- Modulo operator `%` for odd/even checking
- Complex conditional logic in pandas

**Documentation**:
- [pandas.DataFrame.apply](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html)
- [pandas.Series.str.startswith](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.startswith.html)
- [Lambda Functions in Pandas](https://pandas.pydata.org/docs/user_guide/basics.html#function-application)

---

## Day 7: Data Selection & Sorting

### Problem: Fix Names in a Table
**Difficulty**: Easy

**Description**: 
Fix names so that only the first character is uppercase and the rest are lowercase, then sort by user_id.

**Table Schema**:
```sql
Table: Users
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| name           | varchar |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def fix_names(users: pd.DataFrame) -> pd.DataFrame:
    # Fix name capitalization
    users['name'] = users['name'].str.capitalize()
    
    # Sort by user_id
    result = users.sort_values('user_id')
    
    return result
```

**Reasoning**:
1. **String Transformation**: Use `.str.capitalize()` to fix capitalization
2. **In-place Modification**: Assign back to the same column
3. **Sorting**: Order results by user_id

**Key Concepts**:
- `.str.capitalize()` - Proper case conversion
- Column assignment and modification
- Basic sorting operations

**Documentation**:
- [pandas.Series.str.capitalize](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.capitalize.html)
- [pandas.DataFrame.sort_values](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_values.html)
- [String Methods](https://pandas.pydata.org/docs/user_guide/text.html)

---

## Day 8: Date Operations

### Problem: Patients With a Condition
**Difficulty**: Easy

**Description**: 
Find patients who have Type I Diabetes (conditions containing 'DIAB1' prefix).

**Table Schema**:
```sql
Table: Patients
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| patient_id     | int     |
| patient_name   | varchar |
| conditions     | varchar |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def find_patients(patients: pd.DataFrame) -> pd.DataFrame:
    # Find patients with DIAB1 condition (word boundary)
    diab1_patients = patients[
        patients['conditions'].str.contains(r'\bDIAB1', na=False, regex=True)
    ]
    
    return diab1_patients
```

**Reasoning**:
1. **Pattern Matching**: Use `.str.contains()` with regex for flexible matching
2. **Word Boundaries**: `\b` ensures DIAB1 is a complete word/code
3. **Null Handling**: `na=False` treats NaN as False
4. **Regex Flag**: `regex=True` enables regular expression patterns

**Key Concepts**:
- `.str.contains()` with regex patterns
- Word boundary regex `\b`
- Handling null values in string operations
- Regular expressions in pandas

**Documentation**:
- [pandas.Series.str.contains](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.contains.html)
- [Regular Expressions in Python](https://docs.python.org/3/library/re.html)
- [String Methods with regex](https://pandas.pydata.org/docs/user_guide/text.html#method-summary)

---

## Day 9: Data Aggregation - GroupBy

### Problem: Group Sold Products By The Date
**Difficulty**: Easy

**Description**: 
Group products sold by date and show count and comma-separated product names.

**Table Schema**:
```sql
Table: Activities
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| sell_date      | date    |
| product        | varchar |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def categorize_products(activities: pd.DataFrame) -> pd.DataFrame:
    # Group by sell_date and aggregate
    result = activities.groupby('sell_date').agg(
        num_sold=('product', 'nunique'),
        products=('product', lambda x: ','.join(sorted(x.unique())))
    ).reset_index()
    
    return result
```

**Reasoning**:
1. **GroupBy Operations**: Group by date to aggregate products per day
2. **Multiple Aggregations**: Use `agg()` with dictionary for different functions
3. **Unique Count**: `nunique()` counts distinct products
4. **String Aggregation**: Lambda function to join unique sorted products
5. **Reset Index**: Convert grouped result back to DataFrame

**Key Concepts**:
- `groupby()` operations
- `agg()` with multiple functions
- `nunique()` for unique counts
- Lambda functions in aggregation
- `reset_index()` for DataFrame conversion

**Documentation**:
- [GroupBy Operations](https://pandas.pydata.org/docs/user_guide/groupby.html)
- [pandas.DataFrame.agg](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.agg.html)
- [pandas.Series.nunique](https://pandas.pydata.org/docs/reference/api/pandas.Series.nunique.html)
- [pandas.DataFrame.reset_index](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.reset_index.html)

---

## Day 10: Advanced GroupBy

### Problem: Daily Leads and Partners
**Difficulty**: Easy

**Description**: 
Find unique lead_id and partner_id counts for each date_id and make_name combination.

**Table Schema**:
```sql
Table: DailySales
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| date_id        | date    |
| make_name      | varchar |
| lead_id        | int     |
| partner_id     | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def daily_leads_and_partners(daily_sales: pd.DataFrame) -> pd.DataFrame:
    # Group by date_id and make_name, count unique leads and partners
    result = daily_sales.groupby(['date_id', 'make_name']).agg(
        unique_leads=('lead_id', 'nunique'),
        unique_partners=('partner_id', 'nunique')
    ).reset_index()
    
    return result
```

**Reasoning**:
1. **Multi-level GroupBy**: Group by multiple columns
2. **Unique Counting**: Count distinct values for each group
3. **Column Renaming**: Provide meaningful names in aggregation

**Key Concepts**:
- Multi-column groupby
- `nunique()` aggregation
- Meaningful column naming in aggregation

**Documentation**:
- [GroupBy with Multiple Columns](https://pandas.pydata.org/docs/user_guide/groupby.html#grouping-with-a-grouper-specification)
- [pandas.Series.nunique](https://pandas.pydata.org/docs/reference/api/pandas.Series.nunique.html)
- [Aggregation Operations](https://pandas.pydata.org/docs/user_guide/groupby.html#aggregation)

---

## Day 11: Data Merging

### Problem: Employee Bonus
**Difficulty**: Easy

**Description**: 
Report name and bonus of employees with bonus less than 1000.

**Table Schema**:
```sql
Table: Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+

Table: Bonus
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| bonus       | int     |
+-------------+---------+
```

**Solution**:
```python
import pandas as pd

def employee_bonus(employee: pd.DataFrame, bonus: pd.DataFrame) -> pd.DataFrame:
    # Left join to include all employees
    merged = employee.merge(bonus, on='empId', how='left')
    
    # Filter employees with bonus < 1000 or no bonus (null)
    result = merged[
        (merged['bonus'] < 1000) | (merged['bonus'].isna())
    ][['name', 'bonus']]
    
    return result
```

**Reasoning**:
1. **Left Join**: Preserve all employees, even those without bonuses
2. **Null Handling**: Include employees with no bonus records
3. **Filtering**: Apply bonus condition after merge
4. **Column Selection**: Return only required columns

**Key Concepts**:
- `merge()` operations and join types
- Left joins with `how='left'`
- Post-merge filtering
- Handling null values from joins

**Documentation**:
- [Merge and Join Operations](https://pandas.pydata.org/docs/user_guide/merging.html)
- [pandas.DataFrame.merge](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html)
- [Join Types](https://pandas.pydata.org/docs/user_guide/merging.html#database-style-dataframe-or-named-series-joining-merging)

---

## Day 12: String Processing

### Problem: Students and Examinations
**Difficulty**: Easy

**Description**: 
Show how many times each student attended each exam.

**Table Schema**:
```sql
Table: Students
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| student_id     | int     |
| student_name   | varchar |
+----------------+---------+

Table: Subjects
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| subject_name   | varchar |
+----------------+---------+

Table: Examinations
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| student_id     | int     |
| subject_name   | varchar |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def students_and_examinations(students: pd.DataFrame, subjects: pd.DataFrame, 
                            examinations: pd.DataFrame) -> pd.DataFrame:
    # Create cartesian product of students and subjects
    student_subject = students.merge(subjects, how='cross')
    
    # Count examinations per student-subject combination
    exam_counts = examinations.groupby(['student_id', 'subject_name']).size().reset_index(name='attended_exams')
    
    # Left join to include all student-subject combinations
    result = student_subject.merge(
        exam_counts, 
        on=['student_id', 'subject_name'], 
        how='left'
    )
    
    # Fill missing counts with 0
    result['attended_exams'] = result['attended_exams'].fillna(0).astype(int)
    
    # Sort results
    result = result.sort_values(['student_id', 'subject_name'])
    
    return result[['student_id', 'student_name', 'subject_name', 'attended_exams']]
```

**Reasoning**:
1. **Cross Join**: Create all possible student-subject combinations
2. **Counting**: Use `groupby().size()` to count occurrences
3. **Left Join**: Preserve all combinations, even with zero exams
4. **Fill Missing**: Replace NaN with 0 for missing exam counts
5. **Type Conversion**: Convert to integer for clean display

**Key Concepts**:
- Cross joins with `how='cross'`
- `groupby().size()` for counting
- `fillna()` for missing value handling
- `astype()` for type conversion

**Documentation**:
- [Cross Join Operations](https://pandas.pydata.org/docs/user_guide/merging.html#cross-merge)
- [pandas.DataFrame.fillna](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.fillna.html)
- [pandas.DataFrame.astype](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.astype.html)
- [GroupBy size](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.size.html)

---

## Day 13: Advanced Filtering

### Problem: Managers with at Least 5 Direct Reports
**Difficulty**: Medium

**Description**: 
Find managers who have at least 5 direct reports.

**Table Schema**:
```sql
Table: Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
```

**Solution**:
```python
import pandas as pd

def find_managers(employee: pd.DataFrame) -> pd.DataFrame:
    # Count direct reports per manager
    manager_counts = employee.groupby('managerId').size().reset_index(name='direct_reports')
    
    # Filter managers with at least 5 direct reports
    qualified_managers = manager_counts[manager_counts['direct_reports'] >= 5]
    
    # Join back to get manager names
    result = employee.merge(
        qualified_managers,
        left_on='id',
        right_on='managerId'
    )[['name']]
    
    return result
```

**Reasoning**:
1. **Counting Groups**: Count employees per manager
2. **Threshold Filtering**: Find managers meeting minimum requirement
3. **Self Join**: Join employee table with itself to get manager details
4. **Column Mapping**: Use different column names in join

**Key Concepts**:
- Self-referencing joins
- Groupby counting with threshold filtering
- Multi-step data transformation

**Documentation**:
- [Self-Joins](https://pandas.pydata.org/docs/user_guide/merging.html#joining-on-index)
- [GroupBy size operations](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.size.html)
- [Multi-step transformations](https://pandas.pydata.org/docs/user_guide/basics.html#pipe)

---

## Day 14: Data Transformation

### Problem: Sales Person
**Difficulty**: Easy

**Description**: 
Find salespeople who did not have any orders related to company 'RED'.

**Table Schema**:
```sql
Table: SalesPerson
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| sales_id       | int     |
| name           | varchar |
| salary         | int     |
| commission_rate| int     |
| hire_date      | date    |
+----------------+---------+

Table: Company
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| com_id         | int     |
| name           | varchar |
| city           | varchar |
+----------------+---------+

Table: Orders
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| order_id       | int     |
| order_date     | date    |
| com_id         | int     |
| sales_id       | int     |
| amount         | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def sales_person(sales_person: pd.DataFrame, company: pd.DataFrame, 
                orders: pd.DataFrame) -> pd.DataFrame:
    # Find RED company ID
    red_company = company[company['name'] == 'RED']
    
    if red_company.empty:
        return sales_person[['name']]
    
    red_com_id = red_company['com_id'].iloc[0]
    
    # Find sales people who sold to RED
    red_sales = orders[orders['com_id'] == red_com_id]['sales_id'].unique()
    
    # Filter out salespeople who sold to RED
    result = sales_person[~sales_person['sales_id'].isin(red_sales)][['name']]
    
    return result
```

**Reasoning**:
1. **Company Lookup**: Find the specific company by name
2. **Sales Identification**: Get salespeople who dealt with target company
3. **Negation Filter**: Use `~` and `isin()` to exclude unwanted records
4. **Edge Case Handling**: Handle case where company doesn't exist

**Key Concepts**:
- `isin()` for membership testing
- `~` operator for negation
- Multi-table filtering logic
- Edge case handling

**Documentation**:
- [pandas.DataFrame.isin](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.isin.html)
- [Boolean Operations](https://pandas.pydata.org/docs/user_guide/boolean.html)
- [Set Operations](https://pandas.pydata.org/docs/user_guide/indexing.html#set-operations-on-the-other-axes)

---

## Day 15: Missing Relationships

### Problem: Customers Who Never Order
**Difficulty**: Easy

**Description**: 
Find all customers who never placed any order.

**Table Schema**:
```sql
Table: Customers
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+

Table: Orders
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| customerId  | int     |
+-------------+---------+
```

**Sample Data**:
| Customers |         |
|-----------|---------|
| id        | name    |
| 1         | Joe     |
| 2         | Henry   |
| 3         | Sam     |
| 4         | Max     |

| Orders |            |
|--------|------------|
| id     | customerId |
| 1      | 3          |
| 2      | 1          |

**Expected Output**:
| Customers |
|-----------|
| Henry     |
| Max       |

**Solution**:
```python
import pandas as pd

def find_customers(customers: pd.DataFrame, orders: pd.DataFrame) -> pd.DataFrame:
    # Left join customers with orders to identify customers without orders
    merged = customers.merge(orders, left_on='id', right_on='customerId', how='left')
    
    # Find customers with no orders (null customerId after left join)
    customers_no_orders = merged[merged['customerId'].isna()]
    
    # Return customer names
    result = customers_no_orders[['name']]
    result.columns = ['Customers']
    
    return result
```

**Alternative Solution using isin():**
```python
import pandas as pd

def find_customers(customers: pd.DataFrame, orders: pd.DataFrame) -> pd.DataFrame:
    # Get list of customer IDs who have placed orders
    customers_with_orders = orders['customerId'].unique()
    
    # Find customers NOT in the orders list
    customers_no_orders = customers[~customers['id'].isin(customers_with_orders)]
    
    # Return customer names
    result = customers_no_orders[['name']]
    result.columns = ['Customers']
    
    return result
```

**Alternative Solution using set operations:**
```python
import pandas as pd

def find_customers(customers: pd.DataFrame, orders: pd.DataFrame) -> pd.DataFrame:
    # Convert to sets for efficient set operations
    all_customers = set(customers['id'])
    customers_with_orders = set(orders['customerId'])
    
    # Find customers who never ordered using set difference
    customers_no_orders = all_customers - customers_with_orders
    
    # Filter original dataframe and return names
    result = customers[customers['id'].isin(customers_no_orders)][['name']]
    result.columns = ['Customers']
    
    return result
```

**Reasoning**:
1. **Missing Relationship Detection**: Identify customers who don't appear in orders table
2. **Left Join Approach**: Use left join to preserve all customers, then find nulls
3. **Negation with isin()**: Use `~` operator with `isin()` for exclusion logic  
4. **Set Operations**: Leverage mathematical set difference for clean logic
5. **Column Renaming**: Ensure output matches expected format

**Key Concepts**:
- Left joins to detect missing relationships
- `isin()` with negation operator `~`
- Set operations for membership testing
- Null value detection after joins
- Multiple solution approaches for the same problem

**Performance Considerations**:
- **Left Join**: Good for small to medium datasets, clear logic
- **isin() Method**: More efficient for larger datasets, direct filtering
- **Set Operations**: Most efficient for very large datasets, mathematical approach

**Documentation**:
- [Pandas Merge](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html)
- [isin() Method](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.isin.html)
- [Set Operations in Python](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)

---

## Day 16: Complex Joins

### Problem: Customer Who Visited but Did Not Make Any Transactions
**Difficulty**: Easy

**Description**: 
Find customers who visited but made no transactions.

**Table Schema**:
```sql
Table: Visits
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| visit_id       | int     |
| customer_id    | int     |
+----------------+---------+

Table: Transactions
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def find_customers(visits: pd.DataFrame, transactions: pd.DataFrame) -> pd.DataFrame:
    # Left join visits with transactions
    merged = visits.merge(transactions, on='visit_id', how='left')
    
    # Find visits with no transactions (null transaction_id)
    no_transactions = merged[merged['transaction_id'].isna()]
    
    # Count visits per customer
    result = no_transactions.groupby('customer_id').size().reset_index(name='count_no_trans')
    
    return result
```

**Reasoning**:
1. **Left Join**: Preserve all visits, mark unmatched as null
2. **Null Detection**: Find visits without corresponding transactions
3. **Aggregation**: Count no-transaction visits per customer

**Key Concepts**:
- Left joins to detect missing relationships
- Null value detection after joins
- Aggregation on filtered results

**Documentation**:
- [Left Join Operations](https://pandas.pydata.org/docs/user_guide/merging.html#database-style-dataframe-or-named-series-joining-merging)
- [Working with Missing Data](https://pandas.pydata.org/docs/user_guide/missing_data.html)
- [Aggregation after filtering](https://pandas.pydata.org/docs/user_guide/groupby.html#filtration)

---

## Day 16: Window Functions Alternative

### Problem: Rising Temperature
**Difficulty**: Easy

**Description**: 
Find days where temperature was higher than the previous day.

**Table Schema**:
```sql
Table: Weather
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| id             | int     |
| recordDate     | date    |
| temperature    | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def rising_temperature(weather: pd.DataFrame) -> pd.DataFrame:
    # Sort by date
    weather = weather.sort_values('recordDate')
    
    # Create previous day temperature using shift
    weather['prev_temp'] = weather['temperature'].shift(1)
    weather['prev_date'] = weather['recordDate'].shift(1)
    
    # Check if consecutive days and temperature rose
    weather['date_diff'] = (weather['recordDate'] - weather['prev_date']).dt.days
    
    rising_days = weather[
        (weather['date_diff'] == 1) & 
        (weather['temperature'] > weather['prev_temp'])
    ]
    
    return rising_days[['id']]
```

**Reasoning**:
1. **Data Sorting**: Ensure chronological order
2. **Lag Values**: Use `shift()` to get previous day data
3. **Date Arithmetic**: Calculate day differences
4. **Consecutive Filtering**: Ensure we're comparing adjacent days

**Key Concepts**:
- `shift()` for lag/lead operations
- Date arithmetic with `.dt.days`
- Window-like operations without window functions

**Documentation**:
- [pandas.DataFrame.shift](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.shift.html)
- [Date/Time Operations](https://pandas.pydata.org/docs/user_guide/timeseries.html)
- [Time Deltas](https://pandas.pydata.org/docs/user_guide/timedeltas.html)

---

## Day 17: Aggregation & Ranking

### Problem: Game Play Analysis I
**Difficulty**: Easy

**Description**: 
Find the first login date for each player.

**Table Schema**:
```sql
Table: Activity
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| player_id      | int     |
| device_id      | int     |
| event_date     | date    |
| games_played   | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def game_analysis(activity: pd.DataFrame) -> pd.DataFrame:
    # Group by player and find minimum date
    first_login = activity.groupby('player_id')['event_date'].min().reset_index()
    
    # Rename column to match expected output
    first_login.columns = ['player_id', 'first_login']
    
    return first_login
```

**Reasoning**:
1. **GroupBy Aggregation**: Group players and find earliest date
2. **Min Function**: Get minimum date per group
3. **Column Renaming**: Provide meaningful column names

**Key Concepts**:
- `groupby()` with single column aggregation
- `min()` aggregation function
- Column renaming techniques

**Documentation**:
- [GroupBy min operations](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.min.html)
- [Column Operations](https://pandas.pydata.org/docs/user_guide/basics.html#column-selection-addition-deletion)
- [Aggregation Functions](https://pandas.pydata.org/docs/user_guide/groupby.html#aggregation)

---

## Day 18: Advanced String Operations

### Problem: Find Users With Valid E-Mails
**Difficulty**: Easy

**Description**: 
Find users who have valid emails. A valid e-mail has a prefix name and a domain where:
- The prefix name is a string that may contain letters (upper or lower case), digits, underscore '_', period '.', and/or dash '-'. The prefix name must start with a letter.
- The domain is '@leetcode.com'.

**Table Schema**:
```sql
Table: Users
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
| mail          | varchar |
+---------------+---------+
```

**Sample Data**:
| user_id | name      | mail                    |
|---------|-----------|-------------------------|
| 1       | Winston   | winston@leetcode.com    |
| 2       | Jonathan  | jonathanisgreat         |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |
| 5       | Marwan    | quarz#2020@leetcode.com |
| 6       | David     | david69@gmail.com       |
| 7       | Shapiro   | .shapo@leetcode.com     |

**Expected Output**:
| user_id | name      | mail                    |
|---------|-----------|-------------------------|
| 1       | Winston   | winston@leetcode.com    |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |

**Solution**:
```python
import pandas as pd

def valid_emails(users: pd.DataFrame) -> pd.DataFrame:
    # Define regex pattern for valid email
    # ^[a-zA-Z] - must start with letter
    # [a-zA-Z0-9._-]* - followed by letters, digits, underscore, period, or dash
    # @leetcode\.com$ - must end with @leetcode.com
    pattern = r'^[a-zA-Z][a-zA-Z0-9._-]*@leetcode\.com$'
    
    # Filter users with valid emails
    valid_users = users[users['mail'].str.match(pattern, na=False)]
    
    return valid_users
```

**Alternative Solution using contains():**
```python
import pandas as pd

def valid_emails(users: pd.DataFrame) -> pd.DataFrame:
    # More explicit regex pattern with contains
    pattern = r'^[a-zA-Z][a-zA-Z0-9._-]*@leetcode\.com$'
    
    # Filter users with valid emails using contains with full match
    valid_users = users[users['mail'].str.contains(pattern, regex=True, na=False)]
    
    return valid_users
```

**Alternative Solution with step-by-step validation:**
```python
import pandas as pd

def valid_emails(users: pd.DataFrame) -> pd.DataFrame:
    # Create boolean conditions for each validation rule
    conditions = (
        # Must end with @leetcode.com
        users['mail'].str.endswith('@leetcode.com') &
        # Extract prefix (everything before @leetcode.com)
        users['mail'].str.replace('@leetcode.com', '').str.match(r'^[a-zA-Z][a-zA-Z0-9._-]*$', na=False)
    )
    
    return users[conditions]
```

**Reasoning**:
1. **Regex Pattern Construction**: Build pattern to match all email requirements
2. **Prefix Validation**: Ensure prefix starts with letter and contains only valid characters
3. **Domain Validation**: Verify exact domain match with escaped dot
4. **String Matching**: Use `str.match()` for full pattern matching from start
5. **Null Handling**: Use `na=False` to treat NaN values as non-matching

**Key Concepts**:
- Regular expressions for complex string validation
- `str.match()` vs `str.contains()` for pattern matching
- Character classes `[a-zA-Z0-9._-]` for allowed characters
- Anchors `^` and `$` for start/end matching
- Escaping special characters with backslash `\.`
- Multiple validation approaches for the same problem

**Regex Breakdown**:
- `^` - Start of string
- `[a-zA-Z]` - First character must be a letter
- `[a-zA-Z0-9._-]*` - Zero or more valid prefix characters
- `@leetcode\.com` - Literal domain (dot escaped)
- `$` - End of string

**Performance Notes**:
- `str.match()` is generally faster for full pattern matching
- `str.contains()` with anchors achieves same result but may be slower
- Step-by-step validation can be more readable but less efficient

**Documentation**:
- [Pandas String Methods](https://pandas.pydata.org/docs/user_guide/text.html)
- [Regular Expressions in Python](https://docs.python.org/3/library/re.html)
- [str.match() Documentation](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.match.html)

---

## Day 19: Advanced Grouping

### Problem: Biggest Single Number
**Difficulty**: Easy

**Description**: 
Find the largest number that appears only once.

**Table Schema**:
```sql
Table: MyNumbers
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| num            | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def biggest_single_number(my_numbers: pd.DataFrame) -> pd.DataFrame:
    # Count frequency of each number
    num_counts = my_numbers['num'].value_counts()
    
    # Find numbers that appear only once
    single_numbers = num_counts[num_counts == 1]
    
    # Get the largest single number
    if single_numbers.empty:
        result = pd.DataFrame({'num': [None]})
    else:
        largest_single = single_numbers.index.max()
        result = pd.DataFrame({'num': [largest_single]})
    
    return result
```

**Reasoning**:
1. **Frequency Counting**: Use `value_counts()` to count occurrences
2. **Filtering**: Find numbers with count = 1
3. **Max Finding**: Get largest from filtered results
4. **Edge Cases**: Handle case where no single numbers exist

**Key Concepts**:
- `value_counts()` for frequency analysis
- Index operations on Series
- Edge case handling with empty results

---

### Problem: Nth Highest Salary
**Difficulty**: Medium

**Description**: 
Write a solution to find the nth highest salary from the Employee table. If there is no nth highest salary, return null.

**Table Schema**:
```sql
Table: Employee
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
```

**Sample Data**:
| id | salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

**Expected Output for N=2**:
| getNthHighestSalary(2) |
|------------------------|
| 200                    |

**Expected Output for N=4**:
| getNthHighestSalary(4) |
|------------------------|
| null                   |

**Solution**:
```python
import pandas as pd

def nth_highest_salary(employee: pd.DataFrame, N: int) -> pd.DataFrame:
    # Remove duplicates and sort salaries in descending order
    unique_salaries = employee['salary'].drop_duplicates().sort_values(ascending=False)
    
    # Check if N is valid and within range
    if N <= 0 or N > len(unique_salaries):
        nth_salary = None
    else:
        # Get the Nth highest salary (N-1 index since 0-based)
        nth_salary = unique_salaries.iloc[N-1]
    
    # Return result in expected format
    result = pd.DataFrame({f'getNthHighestSalary({N})': [nth_salary]})
    
    return result
```

**Alternative Solution using nlargest():**
```python
import pandas as pd

def nth_highest_salary(employee: pd.DataFrame, N: int) -> pd.DataFrame:
    # Get N largest unique salaries
    unique_salaries = employee['salary'].drop_duplicates()
    
    if N <= 0 or N > len(unique_salaries):
        nth_salary = None
    else:
        # Use nlargest to get top N salaries, then take the last one
        top_n_salaries = unique_salaries.nlargest(N)
        nth_salary = top_n_salaries.iloc[-1] if len(top_n_salaries) == N else None
    
    result = pd.DataFrame({f'getNthHighestSalary({N})': [nth_salary]})
    
    return result
```

**Alternative Solution using rank():**
```python
import pandas as pd

def nth_highest_salary(employee: pd.DataFrame, N: int) -> pd.DataFrame:
    # Create salary ranks (dense ranking to handle duplicates)
    employee_copy = employee.copy()
    employee_copy['rank'] = employee_copy['salary'].rank(method='dense', ascending=False)
    
    # Find the Nth highest salary
    nth_salary_rows = employee_copy[employee_copy['rank'] == N]
    
    if nth_salary_rows.empty:
        nth_salary = None
    else:
        nth_salary = nth_salary_rows['salary'].iloc[0]
    
    result = pd.DataFrame({f'getNthHighestSalary({N})': [nth_salary]})
    
    return result
```

**Reasoning**:
1. **Duplicate Handling**: Use `drop_duplicates()` to ensure unique salary values
2. **Sorting**: Sort salaries in descending order to rank from highest to lowest
3. **Index Access**: Use `iloc[N-1]` for 0-based indexing to get Nth position
4. **Boundary Checking**: Validate N is within valid range (1 to number of unique salaries)
5. **Null Handling**: Return None when N is out of range or invalid
6. **Column Naming**: Create dynamic column name matching expected SQL function format

**Key Concepts**:
- `drop_duplicates()` for handling duplicate salary values
- `sort_values()` with `ascending=False` for descending order
- `iloc[]` for positional indexing
- `nlargest()` as alternative for top-N selection
- `rank()` method for creating salary rankings
- Boundary condition validation
- Dynamic DataFrame column naming

**Ranking Methods Comparison**:
- **Sort + Index**: Most straightforward, good performance
- **nlargest()**: Pandas optimized method, efficient for top-N queries
- **rank()**: Most flexible, handles ties explicitly with different methods

**Edge Cases Handled**:
- N ≤ 0: Invalid input, return null
- N > unique salary count: No Nth salary exists, return null
- Duplicate salaries: Only count unique values for ranking
- Empty table: Return null when no data exists

**Performance Notes**:
- `drop_duplicates()` + `sort_values()`: Good general performance
- `nlargest()`: Optimized for top-N operations, faster for small N
- `rank()`: More memory intensive but handles complex tie scenarios

**Documentation**:
- [Pandas drop_duplicates()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html)
- [Pandas nlargest()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.nlargest.html)
- [Pandas rank()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rank.html)

---

### Problem: Second Highest Salary
**Difficulty**: Medium

**Description**: 
Write a solution to find the second highest salary from the Employee table. If there is no second highest salary, return null.

**Table Schema**:
```sql
Table: Employee
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
```

**Sample Data**:
| id | salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

**Expected Output**:
| SecondHighestSalary |
|---------------------|
| 200                 |

**Solution**:
```python
import pandas as pd

def second_highest_salary(employee: pd.DataFrame) -> pd.DataFrame:
    # Remove duplicates and sort salaries in descending order
    unique_salaries = employee['salary'].drop_duplicates().sort_values(ascending=False)
    
    # Check if there are at least 2 unique salaries
    if len(unique_salaries) < 2:
        second_highest = None
    else:
        # Get the second highest salary
        second_highest = unique_salaries.iloc[1]
    
    # Return result in expected format
    result = pd.DataFrame({'SecondHighestSalary': [second_highest]})
    
    return result
```

**Alternative Solution using nlargest():**
```python
import pandas as pd

def second_highest_salary(employee: pd.DataFrame) -> pd.DataFrame:
    # Get unique salaries and find 2 largest
    unique_salaries = employee['salary'].drop_duplicates().nlargest(2)
    
    # Check if we have a second highest
    if len(unique_salaries) < 2:
        second_highest = None
    else:
        second_highest = unique_salaries.iloc[1]
    
    result = pd.DataFrame({'SecondHighestSalary': [second_highest]})
    
    return result
```

**Reasoning**:
1. **Duplicate Removal**: Ensure we only consider unique salary values
2. **Sorting**: Order salaries from highest to lowest
3. **Length Check**: Verify we have at least 2 unique salaries
4. **Index Access**: Get second position (index 1) for second highest
5. **Null Handling**: Return None when second highest doesn't exist

**Key Concepts**:
- `drop_duplicates()` for unique values
- `sort_values(ascending=False)` for descending order
- `nlargest()` for top-N selection
- `iloc[1]` for second position access
- Edge case handling for insufficient data

**Edge Cases**:
- Only one unique salary: Return null
- Empty table: Return null
- All salaries identical: Return null
- Multiple employees with same salary: Only count unique values

**Documentation**:
- [pandas.DataFrame.drop_duplicates](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html)
- [pandas.DataFrame.sort_values](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_values.html)
- [pandas.DataFrame.nlargest](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.nlargest.html)

---

### Problem: Rank Scores
**Difficulty**: Medium

**Description**: 
Write a solution to rank the scores. The ranking should be calculated according to the following rules:
- The scores should be ranked from the highest to the lowest.
- If there is a tie between two scores, both should have the same ranking.
- After a tie, the next ranking should be the next consecutive integer value.

**Table Schema**:
```sql
Table: Scores
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| score       | decimal |
+-------------+---------+
```

**Sample Data**:
| id | score |
|----|-------|
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |

**Expected Output**:
| score | rank |
|-------|------|
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |

**Solution**:
```python
import pandas as pd

def order_scores(scores: pd.DataFrame) -> pd.DataFrame:
    # Create dense ranking (consecutive integers even with ties)
    scores['rank'] = scores['score'].rank(method='dense', ascending=False)
    
    # Sort by score descending
    result = scores[['score', 'rank']].sort_values('score', ascending=False)
    
    # Convert rank to integer for clean display
    result['rank'] = result['rank'].astype(int)
    
    return result
```

**Alternative Solution with manual ranking:**
```python
import pandas as pd

def order_scores(scores: pd.DataFrame) -> pd.DataFrame:
    # Sort scores in descending order
    sorted_scores = scores.sort_values('score', ascending=False)
    
    # Get unique scores in descending order
    unique_scores = sorted_scores['score'].unique()
    
    # Create rank mapping
    rank_map = {score: rank for rank, score in enumerate(unique_scores, 1)}
    
    # Apply ranking
    sorted_scores['rank'] = sorted_scores['score'].map(rank_map)
    
    return sorted_scores[['score', 'rank']]
```

**Reasoning**:
1. **Dense Ranking**: Use `rank(method='dense')` for consecutive integer ranks
2. **Descending Order**: `ascending=False` to rank from highest to lowest
3. **Tie Handling**: Dense ranking assigns same rank to tied scores
4. **Consecutive Ranks**: Next rank after ties is the next consecutive integer
5. **Sorting**: Order final result by score descending

**Key Concepts**:
- `rank()` method with different ranking strategies
- `method='dense'` for consecutive integer ranking
- Handling tied values in ranking
- Score-based sorting and ranking
- Integer conversion for clean display

**Ranking Methods Comparison**:
- **dense**: 1, 1, 2, 3 (consecutive integers)
- **min**: 1, 1, 3, 4 (gaps after ties)
- **max**: 2, 2, 3, 4 (maximum position for ties)
- **average**: 1.5, 1.5, 3, 4 (average position for ties)

**Performance Notes**:
- `rank()` method is optimized for ranking operations
- Manual ranking with `map()` provides more control but is slower
- Dense ranking is most appropriate for leaderboard scenarios

**Documentation**:
- [pandas.DataFrame.rank](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rank.html)
- [Ranking Methods](https://pandas.pydata.org/docs/user_guide/basics.html#ranking)
- [pandas.DataFrame.sort_values](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_values.html)

---

### Problem: Department Highest Salary
**Difficulty**: Medium

**Description**: 
Find employees who have the highest salary in each department.

**Table Schema**:
```sql
Table: Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| salary      | int     |
| departmentId| int     |
+-------------+---------+

Table: Department
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
```

**Sample Data**:
| Employee |      |        |              |
|----------|------|--------|--------------|
| id       | name | salary | departmentId |
| 1        | Joe  | 70000  | 1            |
| 2        | Bob  | 80000  | 2            |
| 3        | Henry| 80000  | 2            |
| 4        | Sam  | 60000  | 2            |
| 5        | Max  | 90000  | 1            |

| Department |        |
|------------|--------|
| id         | name   |
| 1          | IT     |
| 2          | Sales  |

**Expected Output**:
| Department | Employee | Salary |
|------------|----------|--------|
| IT         | Max      | 90000  |
| Sales      | Bob      | 80000  |
| Sales      | Henry    | 80000  |

**Solution**:
```python
import pandas as pd

def department_highest_salary(employee: pd.DataFrame, department: pd.DataFrame) -> pd.DataFrame:
    # Join employee and department tables
    merged = employee.merge(department, left_on='departmentId', right_on='id', suffixes=('', '_dept'))
    
    # Find maximum salary per department
    max_salaries = merged.groupby('departmentId')['salary'].max().reset_index()
    max_salaries.columns = ['departmentId', 'max_salary']
    
    # Join back to get employees with maximum salary
    result = merged.merge(max_salaries, on='departmentId')
    result = result[result['salary'] == result['max_salary']]
    
    # Select and rename columns
    final_result = result[['name_dept', 'name', 'salary']].copy()
    final_result.columns = ['Department', 'Employee', 'Salary']
    
    return final_result
```

**Alternative Solution using window functions simulation:**
```python
import pandas as pd

def department_highest_salary(employee: pd.DataFrame, department: pd.DataFrame) -> pd.DataFrame:
    # Join tables
    merged = employee.merge(department, left_on='departmentId', right_on='id', suffixes=('', '_dept'))
    
    # Create rank within each department (simulate window function)
    merged['salary_rank'] = merged.groupby('departmentId')['salary'].rank(method='dense', ascending=False)
    
    # Filter employees with rank 1 (highest salary)
    highest_salary_employees = merged[merged['salary_rank'] == 1]
    
    # Format result
    result = highest_salary_employees[['name_dept', 'name', 'salary']].copy()
    result.columns = ['Department', 'Employee', 'Salary']
    
    return result
```

**Reasoning**:
1. **Table Joining**: Combine employee and department data for complete information
2. **Group Maximum**: Find highest salary within each department
3. **Filter Matching**: Include employees whose salary equals department maximum
4. **Handle Ties**: Multiple employees can have the same highest salary
5. **Result Formatting**: Return department name, employee name, and salary

**Key Concepts**:
- `merge()` for joining related tables
- `groupby().max()` for department-wise maximums
- Multiple employees can share highest salary (ties)
- Window function simulation with `groupby().rank()`
- Column aliasing with `suffixes` parameter

**Edge Cases Handled**:
- Multiple employees with same highest salary in a department
- Departments with single employee
- Different departments with different salary ranges

**Performance Notes**:
- Join operations should use indexed columns when possible
- Window function approach scales better for large datasets
- Consider department size distribution for optimization

**Documentation**:
- [pandas.DataFrame.merge](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html)
- [GroupBy max operations](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.max.html)
- [pandas.DataFrame.rank](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rank.html)
- [Merge suffixes](https://pandas.pydata.org/docs/user_guide/merging.html#overlapping-column-names)

---

## Day 20: Data Reshaping

### Problem: Not Boring Movies
**Difficulty**: Easy

**Description**: 
Find movies with odd ID and description not equal to 'boring', ordered by rating.

**Table Schema**:
```sql
Table: Cinema
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| id             | int     |
| movie          | varchar |
| description    | varchar |
| rating         | float   |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def not_boring_movies(cinema: pd.DataFrame) -> pd.DataFrame:
    # Filter movies with odd ID and not boring description
    filtered = cinema[
        (cinema['id'] % 2 == 1) & 
        (cinema['description'] != 'boring')
    ]
    
    # Sort by rating in descending order
    result = filtered.sort_values('rating', ascending=False)
    
    return result
```

**Reasoning**:
1. **Multiple Conditions**: Combine odd ID check and string comparison
2. **Modulo Operation**: Check for odd numbers
3. **String Inequality**: Filter out specific description
4. **Descending Sort**: Order by rating (highest first)

**Key Concepts**:
- Multiple boolean conditions
- Modulo arithmetic in filtering
- Descending sort with `ascending=False`

---

### Problem: Rearrange Products Table
**Difficulty**: Easy

**Description**: 
Rearrange the Products table so that each row has (product_id, store, price). If a product is not available in a store, do not include a row with that product_id and store combination in the result table.

**Table Schema**:
```sql
Table: Products
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| store1      | int     |
| store2      | int     |
| store3      | int     |
+-------------+---------+
```

**Sample Data**:
| product_id | store1 | store2 | store3 |
|------------|--------|--------|--------|
| 0          | 95     | 100    | 105    |
| 1          | 70     | null   | 80     |

**Expected Output**:
| product_id | store  | price |
|------------|--------|-------|
| 0          | store1 | 95    |
| 0          | store2 | 100   |
| 0          | store3 | 105   |
| 1          | store1 | 70    |
| 1          | store3 | 80    |

**Solution**:
```python
import pandas as pd

def rearrange_products_table(products: pd.DataFrame) -> pd.DataFrame:
    # Melt the DataFrame to transform columns to rows
    melted = products.melt(
        id_vars=['product_id'],
        value_vars=['store1', 'store2', 'store3'],
        var_name='store',
        value_name='price'
    )
    
    # Remove rows where price is null (product not available in store)
    result = melted.dropna(subset=['price'])
    
    # Convert price to integer (optional, for clean display)
    result['price'] = result['price'].astype(int)
    
    return result
```

**Alternative Solution using stack():**
```python
import pandas as pd

def rearrange_products_table(products: pd.DataFrame) -> pd.DataFrame:
    # Set product_id as index
    products_indexed = products.set_index('product_id')
    
    # Stack the store columns
    stacked = products_indexed.stack().reset_index()
    stacked.columns = ['product_id', 'store', 'price']
    
    # Remove null prices
    result = stacked.dropna(subset=['price'])
    result['price'] = result['price'].astype(int)
    
    return result
```

**Alternative Solution with manual unpivot:**
```python
import pandas as pd

def rearrange_products_table(products: pd.DataFrame) -> pd.DataFrame:
    result_list = []
    
    # Iterate through each product
    for _, row in products.iterrows():
        product_id = row['product_id']
        
        # Check each store
        for store in ['store1', 'store2', 'store3']:
            price = row[store]
            if pd.notna(price):  # Only include if price is not null
                result_list.append({
                    'product_id': product_id,
                    'store': store,
                    'price': int(price)
                })
    
    return pd.DataFrame(result_list)
```

**Reasoning**:
1. **Data Reshaping**: Transform wide format (columns) to long format (rows)
2. **Null Handling**: Remove entries where products aren't available in stores
3. **Column Transformation**: Convert store columns to store values
4. **Data Cleaning**: Remove missing values and ensure proper data types

**Key Concepts**:
- `melt()` for wide-to-long transformation
- `stack()` for column-to-row conversion
- `dropna()` for removing missing values
- Wide vs. long data format conversion
- Data normalization techniques

**Reshape Methods Comparison**:
- **melt()**: Most explicit and readable for this use case
- **stack()**: More concise but requires index manipulation
- **Manual iteration**: Most control but least efficient

**Use Cases**:
- Converting pivot tables back to normalized form
- Preparing data for visualization libraries
- Database normalization requirements
- Creating consistent data structure for analysis

**Performance Notes**:
- `melt()` is optimized for reshape operations
- `stack()` can be faster for simple cases
- Manual iteration should be avoided for large datasets

**Documentation**:
- [pandas.DataFrame.melt](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.melt.html)
- [pandas.DataFrame.stack](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.stack.html)
- [Reshaping Data](https://pandas.pydata.org/docs/user_guide/reshaping.html)
- [Wide to Long Format](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-by-melt)

---

## Day 21: Data Analysis

### Problem: Average Selling Price
**Difficulty**: Easy

**Description**: 
Calculate average selling price for each product.

**Table Schema**:
```sql
Table: Prices
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| product_id     | int     |
| start_date     | date    |
| end_date       | date    |
| price          | int     |
+----------------+---------+

Table: UnitsSold
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| product_id     | int     |
| purchase_date  | date    |
| units          | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def average_selling_price(prices: pd.DataFrame, units_sold: pd.DataFrame) -> pd.DataFrame:
    # Merge on product_id and date ranges
    merged = units_sold.merge(prices, on='product_id')
    
    # Filter for valid date ranges
    valid_sales = merged[
        (merged['purchase_date'] >= merged['start_date']) & 
        (merged['purchase_date'] <= merged['end_date'])
    ]
    
    # Calculate total revenue and units per product
    product_summary = valid_sales.groupby('product_id').agg(
        total_revenue=('price', lambda x: sum(x * valid_sales.loc[x.index, 'units'])),
        total_units=('units', 'sum')
    ).reset_index()
    
    # Calculate average price
    product_summary['average_price'] = (
        product_summary['total_revenue'] / product_summary['total_units']
    ).round(2)
    
    return product_summary[['product_id', 'average_price']]
```

**Reasoning**:
1. **Date Range Joins**: Merge and filter by date conditions
2. **Weighted Averages**: Calculate using total revenue / total units
3. **Complex Aggregation**: Use lambda functions for custom calculations
4. **Rounding**: Format financial results appropriately

**Key Concepts**:
- Date range filtering after joins
- Weighted average calculations
- Lambda functions in aggregation
- Financial data formatting

**Documentation**:
- [Date Filtering](https://pandas.pydata.org/docs/user_guide/timeseries.html#indexing)
- [Lambda in GroupBy](https://pandas.pydata.org/docs/user_guide/groupby.html#aggregation)
- [pandas.DataFrame.round](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.round.html)

---

## Day 22: Advanced Analysis

### Problem: Project Employees I
**Difficulty**: Easy

**Description**: 
Calculate average experience years for each project.

**Table Schema**:
```sql
Table: Project
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| project_id     | int     |
| employee_id    | int     |
+----------------+---------+

Table: Employee
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| employee_id    | int     |
| name           | varchar |
| experience_years| int    |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def project_employees_i(project: pd.DataFrame, employee: pd.DataFrame) -> pd.DataFrame:
    # Join project and employee data
    merged = project.merge(employee, on='employee_id')
    
    # Calculate average experience per project
    result = merged.groupby('project_id')['experience_years'].mean().reset_index()
    
    # Round to 2 decimal places
    result['average_years'] = result['experience_years'].round(2)
    
    return result[['project_id', 'average_years']]
```

**Reasoning**:
1. **Inner Join**: Combine project and employee information
2. **GroupBy Mean**: Calculate average experience per project
3. **Rounding**: Format decimal places for presentation

**Key Concepts**:
- Standard joins for data enrichment
- Mean aggregation
- Decimal formatting with `round()`

**Documentation**:
- [pandas.DataFrame.merge](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html)
- [GroupBy mean operations](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.mean.html)
- [pandas.DataFrame.round](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.round.html)

---

## Day 23: Multiple Aggregations

### Problem: Percentage of Users Attended a Contest
**Difficulty**: Easy

**Description**: 
Calculate participation percentage for each contest.

**Table Schema**:
```sql
Table: Users
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| user_name      | varchar |
+----------------+---------+

Table: Register
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| contest_id     | int     |
| user_id        | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def users_percentage(users: pd.DataFrame, register: pd.DataFrame) -> pd.DataFrame:
    # Get total number of users
    total_users = len(users)
    
    # Count registrations per contest
    contest_counts = register.groupby('contest_id').size().reset_index(name='registered_users')
    
    # Calculate percentage
    contest_counts['percentage'] = (
        (contest_counts['registered_users'] / total_users) * 100
    ).round(2)
    
    # Sort by percentage desc, then contest_id asc
    result = contest_counts.sort_values(
        ['percentage', 'contest_id'], 
        ascending=[False, True]
    )
    
    return result[['contest_id', 'percentage']]
```

**Reasoning**:
1. **Total Count**: Get baseline for percentage calculation
2. **Group Counting**: Count participants per contest
3. **Percentage Math**: Calculate and format percentages
4. **Multi-column Sort**: Sort by percentage (desc) then ID (asc)

**Key Concepts**:
- Percentage calculations in pandas
- Multi-criteria sorting
- Mathematical transformations

**Documentation**:
- [pandas.DataFrame.sort_values](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_values.html)
- [GroupBy size operations](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.size.html)
- [Mathematical Operations](https://pandas.pydata.org/docs/user_guide/basics.html#binary-operations)

---

## Day 24: Date Analysis

### Problem: Queries Quality and Percentage
**Difficulty**: Easy

**Description**: 
Calculate query quality and poor query percentage.

**Table Schema**:
```sql
Table: Queries
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| query_name     | varchar |
| result         | varchar |
| position       | int     |
| rating         | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def queries_stats(queries: pd.DataFrame) -> pd.DataFrame:
    # Calculate quality and poor query metrics
    queries['quality_score'] = queries['rating'] / queries['position']
    queries['poor_query'] = (queries['rating'] < 3).astype(int)
    
    # Group by query_name and calculate metrics
    result = queries.groupby('query_name').agg(
        quality=('quality_score', 'mean'),
        poor_query_percentage=('poor_query', lambda x: (x.sum() / len(x)) * 100)
    ).reset_index()
    
    # Round results
    result['quality'] = result['quality'].round(2)
    result['poor_query_percentage'] = result['poor_query_percentage'].round(2)
    
    return result
```

**Reasoning**:
1. **Calculated Columns**: Create intermediate calculations
2. **Boolean to Integer**: Convert boolean to numeric for calculations
3. **Custom Aggregation**: Use lambda for percentage calculation
4. **Multiple Metrics**: Calculate different statistics per group

**Key Concepts**:
- Calculated column creation
- Boolean to numeric conversion
- Custom aggregation functions
- Multiple metric calculations

---

### Problem: Count Salary Categories
**Difficulty**: Medium

**Description**: 
Calculate the number of bank accounts for each salary category. The salary categories are:
- "Low Salary": All the salaries strictly less than $20000.
- "Average Salary": All the salaries in the inclusive range [$20000, $50000].
- "High Salary": All the salaries strictly greater than $50000.

**Table Schema**:
```sql
Table: Accounts
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| income      | int  |
+-------------+------+
```

**Sample Data**:
| account_id | income |
|------------|--------|
| 3          | 108939 |
| 2          | 12747  |
| 8          | 87709  |
| 6          | 91796  |

**Expected Output**:
| category       | accounts_count |
|----------------|----------------|
| Low Salary     | 1              |
| Average Salary | 0              |
| High Salary    | 3              |

**Solution**:
```python
import pandas as pd

def count_salary_categories(accounts: pd.DataFrame) -> pd.DataFrame:
    # Define all categories to ensure they appear in result
    categories = ['Low Salary', 'Average Salary', 'High Salary']
    
    # Categorize each account based on income
    def categorize_salary(income):
        if income < 20000:
            return 'Low Salary'
        elif income <= 50000:
            return 'Average Salary'
        else:
            return 'High Salary'
    
    # Apply categorization
    accounts['category'] = accounts['income'].apply(categorize_salary)
    
    # Count accounts per category
    category_counts = accounts['category'].value_counts()
    
    # Ensure all categories are represented (with 0 if missing)
    result_data = []
    for category in categories:
        count = category_counts.get(category, 0)
        result_data.append({'category': category, 'accounts_count': count})
    
    return pd.DataFrame(result_data)
```

**Alternative Solution using pd.cut():**
```python
import pandas as pd

def count_salary_categories(accounts: pd.DataFrame) -> pd.DataFrame:
    # Define salary bins and labels
    bins = [-float('inf'), 20000, 50000, float('inf')]
    labels = ['Low Salary', 'Average Salary', 'High Salary']
    
    # Categorize salaries using cut
    accounts['category'] = pd.cut(
        accounts['income'], 
        bins=bins, 
        labels=labels, 
        right=False,  # Left-inclusive intervals
        include_lowest=True
    )
    
    # Count categories and ensure all are present
    category_counts = accounts['category'].value_counts()
    
    result_data = []
    for label in labels:
        count = category_counts.get(label, 0)
        result_data.append({'category': label, 'accounts_count': count})
    
    return pd.DataFrame(result_data)
```

**Alternative Solution using conditions:**
```python
import pandas as pd

def count_salary_categories(accounts: pd.DataFrame) -> pd.DataFrame:
    # Count each category using boolean conditions
    low_count = (accounts['income'] < 20000).sum()
    avg_count = ((accounts['income'] >= 20000) & (accounts['income'] <= 50000)).sum()
    high_count = (accounts['income'] > 50000).sum()
    
    # Create result DataFrame
    result = pd.DataFrame({
        'category': ['Low Salary', 'Average Salary', 'High Salary'],
        'accounts_count': [low_count, avg_count, high_count]
    })
    
    return result
```

**Reasoning**:
1. **Salary Categorization**: Define clear income ranges for each category
2. **Complete Coverage**: Ensure all possible categories appear in results
3. **Zero Handling**: Include categories with zero accounts
4. **Boundary Conditions**: Handle inclusive/exclusive range boundaries correctly
5. **Consistent Output**: Always return all three categories

**Key Concepts**:
- `apply()` for custom categorization logic
- `pd.cut()` for binning continuous data
- `value_counts()` for counting categories
- Boolean indexing for conditional counting
- Handling missing categories in aggregation results

**Categorization Methods Comparison**:
- **apply() with function**: Most readable and flexible
- **pd.cut()**: Optimized for binning operations
- **Boolean conditions**: Most explicit and fastest for simple cases

**Edge Cases Handled**:
- Empty accounts table: All categories show 0
- Missing categories: Explicitly include with 0 count
- Boundary values: Correctly handle 20000 and 50000 thresholds

**Performance Notes**:
- Boolean condition approach is fastest for simple ranges
- `pd.cut()` is more efficient for complex binning scenarios
- `apply()` provides most flexibility but can be slower

**Documentation**:
- [pandas.DataFrame.apply](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html)
- [pandas.cut](https://pandas.pydata.org/docs/reference/api/pandas.cut.html)
- [pandas.Series.value_counts](https://pandas.pydata.org/docs/reference/api/pandas.Series.value_counts.html)
- [Boolean Indexing](https://pandas.pydata.org/docs/user_guide/indexing.html#boolean-indexing)

---

## Day 25: Advanced Filtering

### Problem: Monthly Transactions I
**Difficulty**: Medium

**Description**: 
Calculate monthly transaction statistics.

**Table Schema**:
```sql
Table: Transactions
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| id             | int     |
| country        | varchar |
| state          | enum    |
| amount         | int     |
| trans_date     | date    |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def monthly_transactions(transactions: pd.DataFrame) -> pd.DataFrame:
    # Extract year-month from date
    transactions['month'] = transactions['trans_date'].dt.to_period('M').astype(str)
    
    # Create approved transaction indicator
    transactions['approved'] = (transactions['state'] == 'approved').astype(int)
    transactions['approved_amount'] = transactions['amount'] * transactions['approved']
    
    # Group by month and country
    result = transactions.groupby(['month', 'country']).agg(
        trans_count=('id', 'count'),
        approved_count=('approved', 'sum'),
        trans_total_amount=('amount', 'sum'),
        approved_total_amount=('approved_amount', 'sum')
    ).reset_index()
    
    return result
```

**Reasoning**:
1. **Date Extraction**: Use `dt.to_period()` for month grouping
2. **Conditional Amounts**: Calculate approved amounts separately
3. **Multi-level Grouping**: Group by both time and categorical dimensions
4. **Multiple Aggregations**: Count transactions and sum amounts

**Key Concepts**:
- Date period extraction with `dt.to_period()`
- Conditional amount calculations
- Multi-dimensional grouping
- Complex aggregation patterns

**Documentation**:
- [Date Period Operations](https://pandas.pydata.org/docs/user_guide/timeseries.html#time-span-representation)
- [pandas.Series.dt.to_period](https://pandas.pydata.org/docs/reference/api/pandas.Series.dt.to_period.html)
- [Multi-level GroupBy](https://pandas.pydata.org/docs/user_guide/groupby.html#grouping-with-a-grouper-specification)

---

## Day 26: Data Validation

### Problem: Immediate Food Delivery II
**Difficulty**: Medium

**Description**: 
Calculate percentage of immediate first orders.

**Table Schema**:
```sql
Table: Delivery
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| delivery_id    | int     |
| customer_id    | int     |
| order_date     | date    |
| customer_pref_delivery_date | date |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def immediate_food_delivery(delivery: pd.DataFrame) -> pd.DataFrame:
    # Find first order for each customer
    first_orders = delivery.loc[delivery.groupby('customer_id')['order_date'].idxmin()]
    
    # Check if first order was immediate
    first_orders['immediate'] = (
        first_orders['order_date'] == first_orders['customer_pref_delivery_date']
    ).astype(int)
    
    # Calculate percentage
    immediate_percentage = (first_orders['immediate'].mean() * 100).round(2)
    
    result = pd.DataFrame({'immediate_percentage': [immediate_percentage]})
    
    return result
```

**Reasoning**:
1. **First Record Selection**: Use `idxmin()` to find first order per customer
2. **Date Comparison**: Compare order and preferred delivery dates
3. **Percentage Calculation**: Calculate overall percentage across customers
4. **Single Value Result**: Return single percentage value

**Key Concepts**:
- `idxmin()` for first record selection
- Date equality comparisons
- Mean for percentage calculations
- Single value DataFrame creation

**Documentation**:
- [pandas.DataFrame.idxmin](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.idxmin.html)
- [Date Comparisons](https://pandas.pydata.org/docs/user_guide/timeseries.html#comparisons)
- [pandas.DataFrame.mean](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.mean.html)

---

## Day 27: Complex Analytics

### Problem: Game Play Analysis IV
**Difficulty**: Medium

**Description**: 
Calculate fraction of players who logged in again the day after first login.

**Table Schema**:
```sql
Table: Activity
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| player_id      | int     |
| device_id      | int     |
| event_date     | date    |
| games_played   | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def gameplay_analysis(activity: pd.DataFrame) -> pd.DataFrame:
    # Find first login date for each player
    first_login = activity.groupby('player_id')['event_date'].min().reset_index()
    first_login.columns = ['player_id', 'first_login']
    
    # Calculate next day date
    first_login['next_day'] = first_login['first_login'] + pd.Timedelta(days=1)
    
    # Check if player logged in the next day
    next_day_activity = activity.merge(
        first_login,
        left_on=['player_id', 'event_date'],
        right_on=['player_id', 'next_day'],
        how='inner'
    )
    
    # Calculate fraction
    total_players = first_login['player_id'].nunique()
    returning_players = next_day_activity['player_id'].nunique()
    
    fraction = round(returning_players / total_players, 2) if total_players > 0 else 0
    
    result = pd.DataFrame({'fraction': [fraction]})
    
    return result
```

**Reasoning**:
1. **First Login Identification**: Find each player's first activity date
2. **Date Arithmetic**: Add one day to first login date
3. **Exact Date Matching**: Check for activity on specific next day
4. **Fraction Calculation**: Calculate retention rate

**Key Concepts**:
- Date arithmetic with `pd.Timedelta()`
- Exact date matching in joins
- Retention rate calculations
- Player behavior analysis

**Documentation**:
- [pandas.Timedelta](https://pandas.pydata.org/docs/reference/api/pandas.Timedelta.html)
- [Date Arithmetic](https://pandas.pydata.org/docs/user_guide/timedeltas.html)
- [Merge on Multiple Columns](https://pandas.pydata.org/docs/user_guide/merging.html#joining-on-index)

---

## Day 28: Advanced Joins

### Problem: Number of Unique Subjects Taught by Each Teacher
**Difficulty**: Easy

**Description**: 
Count unique subjects taught by each teacher.

**Table Schema**:
```sql
Table: Teacher
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| teacher_id     | int     |
| subject_id     | int     |
| dept_id        | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def count_unique_subjects(teacher: pd.DataFrame) -> pd.DataFrame:
    # Count unique subjects per teacher
    result = teacher.groupby('teacher_id')['subject_id'].nunique().reset_index()
    result.columns = ['teacher_id', 'cnt']
    
    return result
```

**Reasoning**:
1. **Unique Counting**: Use `nunique()` to count distinct subjects
2. **Simple Grouping**: Group by teacher and count subjects
3. **Column Renaming**: Provide expected output column names

**Key Concepts**:
- `nunique()` for unique value counting
- Simple groupby operations
- Column renaming in results

**Documentation**:
- [pandas.Series.nunique](https://pandas.pydata.org/docs/reference/api/pandas.Series.nunique.html)
- [GroupBy Operations](https://pandas.pydata.org/docs/user_guide/groupby.html)
- [Column Operations](https://pandas.pydata.org/docs/user_guide/basics.html#column-selection-addition-deletion)

---

## Day 29: Data Quality

### Problem: User Activity for the Past 30 Days I
**Difficulty**: Easy

**Description**: 
Find daily active user count for the last 30 days ending 2019-07-27.

**Table Schema**:
```sql
Table: Activity
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| session_id     | int     |
| activity_date  | date    |
| activity_type  | enum    |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def user_activity(activity: pd.DataFrame) -> pd.DataFrame:
    # Define the date range (30 days ending 2019-07-27)
    end_date = pd.to_datetime('2019-07-27')
    start_date = end_date - pd.Timedelta(days=29)  # 30 days total
    
    # Filter activities in date range
    recent_activity = activity[
        (activity['activity_date'] >= start_date) & 
        (activity['activity_date'] <= end_date)
    ]
    
    # Count unique users per day
    result = recent_activity.groupby('activity_date')['user_id'].nunique().reset_index()
    result.columns = ['day', 'active_users']
    
    return result
```

**Reasoning**:
1. **Date Range Definition**: Calculate 30-day window ending on specific date
2. **Date Filtering**: Filter activities within the specified range
3. **Daily Aggregation**: Count unique users per day
4. **Column Naming**: Provide meaningful column names

**Key Concepts**:
- Date range calculations
- Date filtering with boolean indexing
- Daily user aggregation
- Time window analysis

**Documentation**:
- [pandas.to_datetime](https://pandas.pydata.org/docs/reference/api/pandas.to_datetime.html)
- [pandas.Timedelta](https://pandas.pydata.org/docs/reference/api/pandas.Timedelta.html)
- [Time Series Filtering](https://pandas.pydata.org/docs/user_guide/timeseries.html#indexing)

---

## Day 30: Comprehensive Analysis

### Problem: Article Views II
**Difficulty**: Medium

**Description**: 
Find users who viewed at least one article by the same author on the same day.

**Table Schema**:
```sql
Table: Views
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| article_id     | int     |
| author_id      | int     |
| viewer_id      | int     |
| view_date      | date    |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def article_views(views: pd.DataFrame) -> pd.DataFrame:
    # Group by viewer, author, and date to find multiple views
    viewer_author_day = views.groupby(['viewer_id', 'author_id', 'view_date']).size().reset_index(name='view_count')
    
    # Find cases where viewer saw multiple articles by same author on same day
    multiple_views = viewer_author_day[viewer_author_day['view_count'] >= 2]
    
    # Get unique viewers who had this behavior
    result_viewers = multiple_views['viewer_id'].unique()
    
    # Create result DataFrame and sort
    result = pd.DataFrame({'id': sorted(result_viewers)})
    
    return result
```

**Reasoning**:
1. **Multi-dimensional Grouping**: Group by viewer, author, and date
2. **Count Aggregation**: Count articles viewed per group
3. **Threshold Filtering**: Find groups with multiple views
4. **Unique Extraction**: Get unique viewers meeting criteria
5. **Sorting**: Order results for consistent output

**Key Concepts**:
- Complex multi-dimensional grouping
- Count-based filtering
- Unique value extraction
- Result sorting and formatting

**Documentation**:
- [Multi-level GroupBy](https://pandas.pydata.org/docs/user_guide/groupby.html#grouping-with-a-grouper-specification)
- [GroupBy size operations](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.size.html)
- [pandas.unique](https://pandas.pydata.org/docs/reference/api/pandas.unique.html)

---

## Additional Problems: Data Cleaning & Joins

### Problem: Delete Duplicate Emails
**Difficulty**: Easy

**Description**: 
Delete duplicate emails, keeping only the row with the smallest id for each email.

**Table Schema**:
```sql
Table: Person
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
```

**Sample Data**:
| id | email            |
|----|------------------|
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |

**Expected Output**:
| id | email            |
|----|------------------|
| 1  | john@example.com |
| 2  | bob@example.com  |

**Solution**:
```python
import pandas as pd

def delete_duplicate_emails(person: pd.DataFrame) -> pd.DataFrame:
    # Keep the first occurrence (smallest id) for each email
    person_cleaned = person.drop_duplicates(subset=['email'], keep='first')
    
    return person_cleaned
```

**Alternative Solution using groupby():**
```python
import pandas as pd

def delete_duplicate_emails(person: pd.DataFrame) -> pd.DataFrame:
    # Group by email and keep the row with minimum id
    result = person.loc[person.groupby('email')['id'].idxmin()]
    
    return result.sort_values('id')
```

**Key Concepts**:
- `drop_duplicates()` with subset and keep parameters
- `groupby().idxmin()` for keeping minimum values
- Data deduplication strategies

**Documentation**:
- [pandas.DataFrame.drop_duplicates](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html)
- [pandas.DataFrame.idxmin](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.idxmin.html)
- [Data Deduplication](https://pandas.pydata.org/docs/user_guide/duplicates.html)

---

### Problem: Classes With At Least 5 Students
**Difficulty**: Easy

**Description**: 
Find classes that have at least 5 students.

**Table Schema**:
```sql
Table: Courses
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+
```

**Solution**:
```python
import pandas as pd

def find_classes(courses: pd.DataFrame) -> pd.DataFrame:
    # Count students per class
    class_counts = courses.groupby('class').size().reset_index(name='student_count')
    
    # Filter classes with at least 5 students
    result = class_counts[class_counts['student_count'] >= 5][['class']]
    
    return result
```

**Key Concepts**:
- `groupby().size()` for counting rows
- Threshold filtering
- Column selection

**Documentation**:
- [GroupBy size operations](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.size.html)
- [Boolean Filtering](https://pandas.pydata.org/docs/user_guide/indexing.html#boolean-indexing)
- [Column Selection](https://pandas.pydata.org/docs/user_guide/indexing.html#selection-by-label)

---

### Problem: Customer Placing the Largest Number of Orders
**Difficulty**: Easy

**Description**: 
Find the customer who has placed the largest number of orders.

**Table Schema**:
```sql
Table: Orders
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| order_number| int     |
| customer_number| int  |
+-------------+---------+
```

**Solution**:
```python
import pandas as pd

def largest_orders(orders: pd.DataFrame) -> pd.DataFrame:
    # Count orders per customer
    customer_counts = orders.groupby('customer_number').size().reset_index(name='order_count')
    
    # Find customer with maximum orders
    max_orders = customer_counts['order_count'].max()
    result = customer_counts[customer_counts['order_count'] == max_orders][['customer_number']]
    
    return result
```

**Alternative Solution using idxmax():**
```python
import pandas as pd

def largest_orders(orders: pd.DataFrame) -> pd.DataFrame:
    # Count orders per customer
    customer_counts = orders['customer_number'].value_counts()
    
    # Get customer with maximum orders
    top_customer = customer_counts.idxmax()
    
    return pd.DataFrame({'customer_number': [top_customer]})
```

**Key Concepts**:
- `value_counts()` for frequency analysis
- `idxmax()` for finding maximum index
- Maximum value identification

**Documentation**:
- [pandas.Series.value_counts](https://pandas.pydata.org/docs/reference/api/pandas.Series.value_counts.html)
- [pandas.Series.idxmax](https://pandas.pydata.org/docs/reference/api/pandas.Series.idxmax.html)
- [Statistical Functions](https://pandas.pydata.org/docs/user_guide/basics.html#descriptive-statistics)

---

### Problem: Replace Employee ID With The Unique Identifier
**Difficulty**: Easy

**Description**: 
Show the unique ID of each user. If a user does not have a unique ID, show null.

**Table Schema**:
```sql
Table: Employees
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+

Table: EmployeeUNI
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
```

**Solution**:
```python
import pandas as pd

def replace_employee_id(employees: pd.DataFrame, employee_uni: pd.DataFrame) -> pd.DataFrame:
    # Left join to include all employees
    result = employees.merge(employee_uni, on='id', how='left')
    
    # Select only required columns
    return result[['unique_id', 'name']]
```

**Key Concepts**:
- Left joins to preserve all records
- Handling missing values from joins
- Table relationship management

**Documentation**:
- [pandas.DataFrame.merge](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html)
- [Left Join Operations](https://pandas.pydata.org/docs/user_guide/merging.html#database-style-dataframe-or-named-series-joining-merging)
- [Working with Missing Data](https://pandas.pydata.org/docs/user_guide/missing_data.html)

---

## 🔗 Resources

- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Pandas User Guide](https://pandas.pydata.org/docs/user_guide/index.html)
- [Boolean Indexing Guide](https://pandas.pydata.org/docs/user_guide/indexing.html#boolean-indexing)
- [GroupBy Operations](https://pandas.pydata.org/docs/user_guide/groupby.html)

## 📝 Study Notes

### Essential Pandas Methods
- **Filtering**: `df[condition]`, `df.query()`
- **Selection**: `df['col']`, `df[['col1', 'col2']]`
- **Aggregation**: `df.groupby().agg()`, `df.sum()`, `df.count()`
- **Sorting**: `df.sort_values()`, `df.sort_index()`
- **String Operations**: `df['col'].str.method()`
- **Date Operations**: `pd.to_datetime()`, `df.dt.method()`

### Common Patterns
1. **Filter → Select → Transform**: Most common data manipulation pattern
2. **Group → Aggregate → Reset**: Standard aggregation workflow  
3. **Merge → Filter → Select**: Join and filter workflow
