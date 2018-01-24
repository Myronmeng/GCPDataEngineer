# Comparison between standard and legacy SQL
refer: https://cloud.google.com/bigquery/docs/reference/standard-sql/migrating-from-legacy-sql#timestamp_differences
## Syntax difference
### Composability using WITH clauses and SQL functions
### Subqueries in the SELECT list and WHERE clause
### Correlated subqueries
### ARRAY and STRUCT data types
### Inserts, updates, and deletes
### COUNT(DISTINCT <expr>) is exact and scalable, providing the accuracy of EXACT_COUNT_DISTINCT without its limitations
### Automatic predicate push-down through JOINs
### Complex JOIN predicates, including arbitrary expressions
### Type difference
Legacy SQL types have an equivalent in standard SQL and vice versa. In some cases, the type has a different name. The following table lists each legacy SQL data type and its standard SQL equivalent.

Legacy SQL	Standard SQL	Notes
BOOL	      BOOL	
INTEGER	    INT64	
FLOAT	      FLOAT64	
STRING	    STRING	
BYTES	      BYTES	
RECORD	    STRUCT	
REPEATED	  ARRAY	
TIMESTAMP	  TIMESTAMP	    See TIMESTAMP differences
DATE	      DATE	        Legacy SQL has limited support for DATE
TIME	      TIME	        Legacy SQL has limited support for TIME
DATETIME	  DATETIME	    Legacy SQL has limited support for DATETIME
### Legacy SQL allows reserved keywords in some places that standard SQL does not. For example, the following query fails due to a Syntax error using standard SQL:
```sql
#standardSQL
SELECT
  COUNT(*) AS rows
FROM
  `bigquery-public-data.samples.shakespeare`;
```
rows as a reserved keywords. In legacy sql it is ok but in standard, it needs to be replaced by:
```sql
SELECT
  COUNT(*) AS `rows`
```
### Project-qualified table names
```sql
#legacySQL
SELECT
  word
FROM
  [bigquery-public-data:samples.shakespeare]
LIMIT 1;
#standardSQL
SELECT
  word
FROM
  `bigquery-public-data.samples.shakespeare`
LIMIT 1;
```
### Trailing commas in the SELECT list
Unlike legacy SQL, standard SQL does not permit trailing commas prior to the FROM clause. For example, the following query is invalid:
```sql
#standardSQL
SELECT
  word,
  corpus,  -- Error due to trailing comma
FROM
  `bigquery-public-data.samples.shakespeare`
LIMIT 1;
```
