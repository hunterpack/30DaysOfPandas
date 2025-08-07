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

---

## Day 4: Data Transformation

### Problem: Article Views
**Difficulty**: Easy

**Description**: 
Find authors who viewed their own articles, sorted by id.

**Solution**:
```python
import pandas as pd

def article_views(views: pd.DataFrame) -> pd.DataFrame:
    # Find rows where author_id equals viewer_id
    self_views = views[views['author_id'] == views['viewer_id']]
    
    # Get unique author_ids and sort them
    unique_authors = self_views['author_id'].drop_duplicates().sort_values()
    
    # Create result DataFrame
    result = pd.DataFrame({'id': unique_authors})
    
    return result
```

**Reasoning**:
1. **Column Comparison**: Compare two columns within the same DataFrame
2. **Deduplication**: Use `drop_duplicates()` to get unique values
3. **Sorting**: Use `sort_values()` for ascending order
4. **DataFrame Creation**: Build new DataFrame with renamed column

**Key Concepts**:
- Column-to-column comparison
- `drop_duplicates()` - Remove duplicate values
- `sort_values()` - Sort DataFrame/Series
- `pd.DataFrame()` - Create new DataFrame

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

---

## Day 15: Complex Joins

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

---

## Day 18: Advanced String Operations

### Problem: Triangle Judgement
**Difficulty**: Easy

**Description**: 
Determine if three lengths can form a triangle.

**Table Schema**:
```sql
Table: Triangle
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| x              | int     |
| y              | int     |
| z              | int     |
+----------------+---------+
```

**Solution**:
```python
import pandas as pd

def triangle_judgement(triangle: pd.DataFrame) -> pd.DataFrame:
    # Apply triangle inequality theorem
    triangle['triangle'] = triangle.apply(
        lambda row: 'Yes' if (
            row['x'] + row['y'] > row['z'] and
            row['x'] + row['z'] > row['y'] and
            row['y'] + row['z'] > row['x']
        ) else 'No',
        axis=1
    )
    
    return triangle
```

**Reasoning**:
1. **Mathematical Logic**: Apply triangle inequality theorem
2. **Row-wise Operations**: Use `apply()` with `axis=1`
3. **Conditional Assignment**: Complex if-else logic in lambda

**Key Concepts**:
- `apply()` with complex logic
- Mathematical conditions in pandas
- String result assignment

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
