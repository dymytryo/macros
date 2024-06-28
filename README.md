# Documentation for DBT Macros

## `get_columns`

### Overview
The `get_columns` macro dynamically retrieves column details from a specified schema and table in Snowflake. It supports optional exclusion of certain columns and applies type-specific transformations to ensure compatibility and optimal performance in SQL queries.

### Usage
```jinja
{{ get_columns(schema_name, table_name, exclude_columns=none) }}
```

### Parameters
- **schema_name** (string): The name of the schema from which to retrieve column details.
- **table_name** (string): The name of the table from which to retrieve column details.
- **exclude_columns** (list, optional): A list of column names to exclude from the output. Default is `none`, which means no columns are excluded.

### Returns
- A list of column expressions, potentially with SQL transformations. Returns an empty list if no columns are found or if the input parameters are invalid.

### Example
```jinja
{{ get_columns('public', 'users', exclude_columns=['password', 'ssn']) }}
```

## `starburst_etl_macro`

### Overview
The `starburst_etl_macro` uses the `get_columns` macro to fetch and prepare column expressions for a SQL `SELECT` statement. It constructs a complete query for data extraction and transformation tasks, handling schema and table specifics dynamically.

### Usage
```jinja
{{ starburst_etl_macro(schema_name, table_name, exclude_columns=none) }}
```

### Parameters
- **schema_name** (string): The name of the schema containing the table to query.
- **table_name** (string): The name of the table to query.
- **exclude_columns** (list, optional): A list of column names to exclude from the SQL SELECT statement. Default is `none`.

### Returns
- A string containing a SQL `SELECT` statement. If no valid columns are present, returns a dummy SQL statement (`SELECT 1`).

### Example
```jinja
{{ etl_macro('public', 'users', exclude_columns=['password', 'ssn']) }}
```

### Configuration Settings
Add any global configurations in your `dbt_project.yml` under the `vars` section. For instance, you can configure the default VARCHAR length:

```yaml
vars:
  default_varchar_length: 65535
```

### Notes
- Ensure all input parameters are validated to avoid SQL injection risks.
- These macros are designed specifically for Snowflake but can be adapted for other SQL dialects by adjusting the query syntax and handling in the macros.
