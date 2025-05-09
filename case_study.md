# Video Game Sales Analysis

## Dataset

You will be working with the following dataset: [Video Game Sales](https://www.kaggle.com/datasets/gregorut/videogamesales?resource=download)

## Business Question
How can game developers and publishers optimize their strategy to maximize global sales by understanding the performance of different game genres, platforms, and publishers?

*To answer the above question, use the following SQL queries to explore the dataset and address the following questions:*

Which genres contribute the most to global sales?

SQL:
```sql
SELECT 
    genre,
    SUM(global_sales) total_sales
FROM vgs 
GROUP BY 
    genre
ORDER BY 
    2 DESC
```
Findings:
```findings
Action games are the most successful, generating sales of $1.8B
```
Which platforms generate the highest global sales?

SQL:
```sql
SELECT 
    platform,
    SUM(global_sales) total_sales
FROM vgs 
GROUP BY 
    platform
ORDER BY 
    2 DESC
```
Findings:
```findings
PS2 generates the highest sales of $1.3B.
```
Which publishers are the most successful in terms of global sales?

SQL:
```sql
SELECT 
    publisher,
    SUM(global_sales) total_sales
FROM vgs 
GROUP BY 
   publisher
ORDER BY 
    2 DESC
```
Findings:
```findings
Nintendo is the most successful, generating global sales of $1.8B
```
How does success vary across regions (North America, Europe, Japan, Others)?

SQL:
```sql
SELECT 
    SUM(na_sales) na,
    SUM(eu_sales) eu,
    SUM(jp_sales) jp,
    SUM(other_sales) other
FROM vgs 
```
Findings:
```findings
Sales in North America is highest at $4.4B, followed by EU at $2.4B, Japan racks in $1.3B, and the rest of the world generates $798M.
```
What are the trends over time in game sales by genre and platform?

SQL:
```sql
WITH 
    base AS (
        SELECT 
            year,
            genre,
            SUM(global_sales) total_sales
        FROM vgs 
        GROUP BY 
            year,
            genre
    )
SELECT 
    base.year,
    base.genre,
    base.total_sales
FROM base
    JOIN (
        SELECT 
            year,
            MAX(total_sales) max_sales
        FROM base
        GROUP BY 
            year
    ) max ON base.year=max.year
        AND base.total_sales=max.max_sales
ORDER BY 
    base.year;

WITH 
    base AS (
        SELECT 
            year,
            platform,
            SUM(global_sales) total_sales
        FROM vgs 
        GROUP BY 
            year,
            platform
    )
SELECT 
    base.year,
    base.platform,
    base.total_sales
FROM base
    JOIN (
        SELECT 
            year,
            MAX(total_sales) max_sales
        FROM base
        GROUP BY 
            year
    ) max ON base.year=max.year
        AND base.total_sales=max.max_sales
ORDER BY 
    base.year
```
Findings:
```findings
Genre-wise, action has been the top genre since 2001. Previous to that, platform was more popular.

PS has been largely the dominant platform since the 1990s, although there were brief spells that it was overtaken by Xbox and Wii.
```
Which platforms are most successful for specific genres?

SQL:
```sql
WITH 
    base AS (
        SELECT 
            genre,
            platform,
            SUM(global_sales) total_sales,
            ROW_NUMBER() OVER (PARTITION BY genre ORDER BY SUM(global_sales) DESC) rank
        FROM vgs 
        GROUP BY 
            genre,
            platform
    )
SELECT 
    *
FROM base 
WHERE 
    rank<4
ORDER BY 
    genre,
    rank
```
Findings:
```findings
Taking the top 3 platform from the top 3 genres, PS is overwhelmingly the most popular platform. Also, as PS3 appears as one of the top 3 amongst these top 3 genres, it might be advisable to design the game for PS3
```
## Deliverables:
- SQL Queries: Provide all the SQL queries you used to answer the business questions.
- Summary of Findings: For each question, summarise your key findings and recommendations based on your analysis.

## Submission

- Submit the GitHub URL of your assignment to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
