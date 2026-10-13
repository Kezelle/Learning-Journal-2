# Learning-Journal-2

## Section 1: What I Learned
As a Data Quality Analyst at a financial company, my primary responsibility was originally focused on validating data accuracy. However, our role expanded significantly when our team took on the additional task of extracting critical data for the Sales Team. Initially, we relied on a basic HDB UI for our data needs, but over time, our infrastructure modernized, and we transitioned to more advanced platforms like DBeaver and Snowflake.
Because our team members come from non-technical backgrounds, we were initially handed pre-written, templated scripts to execute these data extractions. Thanks to the structured training at this Uplift bootcamp, I finally understand the foundational logic behind the SQL queries we use every day. I now clearly grasp the distinct functionalities of essential commands:
- `SELECT`: Explicitly defines which data columns will be displayed in our final targeted results.
- `FROM`: Identifies the exact source database or table where the information lives.
- `LIMIT`: Restricts the overall output size, which we set to 1,000,000 to align with the maximum row capacity of a standard Excel spreadsheet.

## Section 2: A Query I Am Proud Of

```sql
SELECT 
    i.CustomerId,
    c.FirstName,
    c.LastName,
    t.Name as TrackName,
    i.InvoiceID,
    ii.InvoiceLineID,
    ii.TrackID,
    t.UnitPrice
FROM customers c 
LEFT JOIN invoices i ON c.CustomerId = i.CustomerId 
LEFT JOIN invoice_items ii ON i.InvoiceId = ii.InvoiceId 
LEFT JOIN tracks t ON ii.TrackId = t.TrackId 
WHERE c.CustomerID = 5;
```


To execute the Chinook exercise, I structured the query to follow standard data analysis workflows, dividing the logic into three functional parts:
- Column Selection: I initiated the code by explicitly stating the specific metrics and fields required for the final output. By identifying each individual column name, I established exactly what the Sales Team will see, noting the original tables they belong to.
-  Table Linking: The FROM clause establishes the customers database as our primary anchor, utilizing the alias "c" to keep the script concise and readable. I applied a series of `LEFT JOIN` commands to sequentially merge relevant transactional details from the invoices, invoice_items, and tracks datasets without losing any underlying customer records.
-  Data Filtering: Finally, I implemented a strict `WHERE` clause restriction to isolate the dataset, ensuring the targeted output exclusively displays information tied to CustomerID #5.

Section 3: A Mistake or Struggle

-- 🚫 Broken Query

`SELECT` t.GenreId, AVE(t.UnitPrice) AS Ave._Unit_Price <br/>
`FROM `tracks t <br/>
`GROUP BY` t.GenreId;<br/>

🚨 Error Message

SQL Error: [SQLITE_ERROR] SQL error or missing database (near ".": syntax error)


🔍 The Cause

1. Incorrect Function Name
There is no standard SQL function called AVE(). The standard aggregate function to calculate an average is AVG().

2. Invalid Column Alias
The identifier Ave._Unit_Price contains a period (.). In SQL, the dot notation is a reserved operator used to separate a table or schema name from a column name (e.g., table.column). The database parser interprets Ave as a table name, triggering a syntax error.

The Solution

To fix the query, swap AVE for AVG and remove the period from the alias. If you want to keep spaces or special characters in an alias, wrap it in double quotes ("") or square brackets ([]).

## Section 4: Connecting to the Real World

As mentioned earlier, our company handles a huge volume of data. Before we modernized and streamlined our workflow, we used to extract massive datasets into hundreds of Excel files per folder. We learned the hard way how inefficient this was when we discovered we were occasionally missing a column or two after an extraction. This oversight required massive manual effort to open and append the files, or worse, forced us to redo the entire process.

Today, we use SQL to answer business questions directly from our database, eliminating the need for manual, spreadsheet-based workarounds.

1. Two Questions the Business Can Answer with SQL

- The Missing Data Problem: "How do we reconstruct historical reports to include missing columns without re-extracting everything?"
  We used a `LEFT JOIN` on reextracted_missing_columns and extracted_data to seamlessly append missing metrics.
  
- Data Cleansing: "How do we filter out weekend records and inverted pricing?"
  We applied a `WHERE` clause (`WHERE` Ask > Bid) to isolate weekdays and purge faulty market data.

2. Why SQL is Still Widely Used for Working with Data

SQL remains the gold standard in the industry because of its unmatched scale and efficiency. As our team's experience demonstrates, traditional tools like Excel completely break down, crash, or become a logistical nightmare when handling huge amounts of data split across hundreds of files. It ensures data integrity by eliminates manual human copy-paste errors and allows teams to manipulate massive datasets using simple logical commands rather than intensive manual labor.

### Section 5: Self-Assessment

📊 SQL Skills Self-Assessment

!<img width="604" height="257" alt="image" src="https://github.com/user-attachments/assets/30edfdcf-3a5e-49bf-aca2-f73f4c09258c" />

I gave my self an assessment of 3 for Basic `SELECT`, `WHERE`, `ORDER BY`, AGGREGATION (`COUNT`, `SUM`, `AVG`, `GROUP BY`, `HAVING`), JOINS(2 or more tables) since I was able to get the code running after listening to the recording, following the instructions and rewriting the code a couple of times to address the errors. I was able to answer up to Part C #3 and realized that it just follow the same logic as the basic ones presented earlier, but using a different way to show the same result. I just had to follow the syntax properly to make it work. I was unable to finish numbers 4 & 5 in Part C due to time constraints.

## Section 6: Goals and Next Steps

One SQL skill I want to strengthen is advanced data cleansing and wrangling, specifically handling edge cases like filtering out holidays, correcting inverted prices, and implementing Last Good Value (LGV) logic. To practice this, I will write mock SQL scripts using sample financial datasets to build clean data pipelines.

A new topic I am highly curious about is Window Functions. I want to learn how functions like LAG() and LEAD() can look at adjacent rows to help me build a database directory query, allowing me to pinpoint exactly which file contains a specific date’s record without manually opening them.

My specific, measurable goal is: By next Friday, I will write two queries—one that implements LGV logic to replace inverted prices and one that filters out weekends  from used case extracts.
