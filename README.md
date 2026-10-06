# 🎬 Netflix Data Analysis Using SQL

## 📌 Project Overview

This project focuses on analysing Netflix's movies and TV shows dataset using SQL. The aim of the project is to explore the dataset, solve real-world analytical questions, and extract meaningful insights about Netflix's content library.

Through this project, I applied SQL techniques to analyse content distribution, ratings, genres, release trends, countries, actors, directors, and other key characteristics of Netflix content.

---

## 🎯 Objectives

The main objectives of this project are:

* Analyse the distribution of Movies and TV Shows on Netflix.
* Identify the most common content ratings.
* Explore content based on release year and country.
* Analyse movie durations and TV show seasons.
* Identify frequently represented genres and content categories.
* Explore actors and directors featured in Netflix content.
* Categorise content based on specific keywords and descriptions.
* Apply SQL techniques to answer practical analytical questions.

---

## 🛠️ Tools & Technologies

* **PostgreSQL** – Database management and SQL analysis
* **pgAdmin 4** – Database administration and query execution
* **SQL** – Data querying, transformation, and analysis

---

## 📂 Dataset

The dataset contains information about Movies and TV Shows available on Netflix.

### Key Columns

* `show_id` – Unique ID for each title
* `type` – Movie or TV Show
* `title` – Name of the content
* `director` – Director of the title
* `casts` – Actors featured in the content
* `country` – Country where the content was produced
* `date_added` – Date the content was added to Netflix
* `release_year` – Original release year
* `rating` – Content rating
* `duration` – Movie duration or number of TV show seasons
* `listed_in` – Genre/category
* `description` – Description of the content

---

## 🗄️ Database Schema

The following table structure was used to import and analyse the Netflix dataset in PostgreSQL:

```sql
CREATE TABLE netflix (
    show_id VARCHAR(10),
    type VARCHAR(10),
    title VARCHAR(150),
    director VARCHAR(250),
    casts VARCHAR(1000),
    country VARCHAR(150),
    date_added VARCHAR(50),
    release_year INT,
    rating VARCHAR(10),
    duration VARCHAR(15),
    listed_in VARCHAR(100),
    description VARCHAR(250)
);
```

---

## 🔍 Business Questions

The 15 queries in [solutions.sql](solutions.sql) explore:

1. How many Movies and TV Shows are in the dataset?
2. What is the most common rating within each content type?
3. Which titles were released in 2020?
4. Which five country values have the highest title counts after splitting multi-country entries?
5. How do Movies rank by their recorded duration?
6. Which titles have an added date within the last five years, relative to the execution date?
7. Which titles contain the exact split director value `Rajiv Chilaka`?
8. Which TV Shows have more than five seasons?
9. How many titles are associated with each split genre value?
10. Which five release years account for the largest percentages of titles whose country field is exactly `India`?
11. Which titles have a genre field ending in `Documentaries`?
12. Which titles have a NULL director field?
13. Which titles mention `Salman Khan` in their cast and have a release year greater than the execution year minus ten?
14. Which ten split actor values occur most frequently among titles whose country field is exactly `India`?
15. How many descriptions contain the substrings `kill` or `violence`, using case-insensitive matching?

---

## 💡 SQL Concepts Used

This project demonstrates the use of:

* `SELECT`
* `WHERE`
* `GROUP BY`
* `ORDER BY`
* Aggregate function: `COUNT()`
* `LIMIT`
* `CASE` statements
* Common Table Expressions (CTEs)
* Window functions: `RANK() OVER (PARTITION BY ...)`
* String manipulation functions
* Date functions
* `UNNEST`
* `STRING_TO_ARRAY`
* Data type conversion
* Pattern matching

---

## 📊 Selected Query Examples and Business Takeaways

The examples below demonstrate how the SQL answers catalogue-analysis questions. Results were independently checked against the repository's [netflix_titles.csv](netflix_titles.csv), containing **8,807 unique titles**. They describe this dataset snapshot, not Netflix's current catalogue.

### 1. What is the balance between Movies and TV Shows?

```sql
SELECT
    type,
    COUNT(*) AS total_titles
FROM netflix
GROUP BY type;
```

| Content type | Titles |
|---|---:|
| Movie | 6,131 |
| TV Show | 2,676 |

**Result:** Movies represent **69.62%** of titles, compared with **30.38%** for TV Shows.

**Business takeaway:** The catalogue is weighted towards movies by title count. This provides a baseline for reviewing format diversity, but a TV Show can contain multiple seasons and episodes, so title counts do not measure viewing hours or audience demand.

**SQL demonstrated:** Aggregation and `GROUP BY`. The query is equivalent to Question 1, with a descriptive output alias.

### 2. Which content rating is most common within each format?

```sql
WITH RatingCounts AS (
    SELECT
        type,
        rating,
        COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT
        type,
        rating,
        rating_count,
        RANK() OVER (
            PARTITION BY type
            ORDER BY rating_count DESC
        ) AS rank
    FROM RatingCounts
)
SELECT
    type,
    rating AS most_frequent_rating
FROM RankedRatings
WHERE rank = 1;
```

| Content type | Most frequent rating |
|---|---|
| Movie | TV-MA |
| TV Show | TV-MA |

**Result:** `TV-MA` is the most frequent rating for both formats. The underlying groups contain **2,062 movies** and **1,145 TV shows**; these counts are calculated in the CTE but are not displayed by the final query.

**Business takeaway:** Mature-audience content is the largest individual rating group in both formats. This helps describe audience suitability and catalogue composition, but does not establish which content viewers prefer.

**SQL demonstrated:** CTEs and the `RANK()` window function. Partitioning ranks ratings separately for each format, and `RANK()` preserves ties.

### 3. Which release years contribute the most India-only titles?

```sql
SELECT
    country,
    release_year,
    COUNT(show_id) AS total_release,
    ROUND(
        COUNT(show_id)::numeric /
        (
            SELECT COUNT(show_id)
            FROM netflix
            WHERE country = 'India'
        )::numeric * 100,
        2
    ) AS avg_release
FROM netflix
WHERE country = 'India'
GROUP BY country, release_year
ORDER BY avg_release DESC
LIMIT 5;
```

| Country | Release year | Titles | Share of India-only titles |
|---|---:|---:|---:|
| India | 2017 | 101 | 10.39% |
| India | 2018 | 94 | 9.67% |
| India | 2019 | 87 | 8.95% |
| India | 2020 | 75 | 7.72% |
| India | 2016 | 73 | 7.51% |

**Result:** Of the **972 titles** whose country field is exactly `India`, 2017 contributes the largest share.

**Business takeaway:** This identifies the release-year concentration of the India-only subset and supports a review of its age profile. It does not measure when Netflix acquired the titles, annual production across India, or viewing performance.

**Interpretation note:** Despite the alias `avg_release`, the calculation returns a **percentage**, not an average. The exact-country filter excludes multi-country entries involving India, and the query includes both Movies and TV Shows.

**SQL demonstrated:** A scalar subquery, aggregation, numeric conversion, percentage calculation, rounding and top-five selection.

### 4. How many descriptions contain “kill” or “violence”?

```sql
SELECT
    category,
    COUNT(*) AS content_count
FROM (
    SELECT
        CASE
            WHEN description ILIKE '%kill%'
              OR description ILIKE '%violence%'
            THEN 'Bad'
            ELSE 'Good'
        END AS category
    FROM netflix
) AS categorized_content
GROUP BY category;
```

| SQL category | Meaning | Titles |
|---|---|---:|
| Bad | Description contains either substring | 342 |
| Good | Description contains neither substring | 8,465 |

**Result:** **342 titles (3.88%)** match at least one keyword.

**Business takeaway:** A simple text rule can create an initial review list for content tagging. Manual review would be needed before using it to assess suitability.

**Interpretation note:** `Good` and `Bad` are labels used by the SQL, not judgements about content quality or safety. Substring matching can include unrelated words such as “skill” and miss relevant descriptions that use different wording.

**SQL demonstrated:** `CASE`, case-insensitive `ILIKE`, substring matching and grouped counts.

---

## 📝 Interpretation and Query Limitations

- **Catalogue presence is not popularity.** The dataset contains title metadata, not viewing figures, revenue or subscriber behaviour.
- **Country, genre and actor counts need care.** The current queries split comma-separated fields without trimming whitespace. Values such as `India` and ` India` may form separate groups. Titles with multiple countries or genres can contribute to more than one group.
- **Some headings in the SQL are broader than their implementation.** Questions 3, 11, 13 and 14 do not explicitly filter to Movies. Question 13 returns matching rows rather than a count.
- **The duration query returns all Movies.** Question 5 sorts numeric durations in descending order but does not use `LIMIT 1` or `NULLS LAST`, so the first row should not automatically be reported as the longest valid Movie.
- **Missing metadata requires explicit handling.** Question 12 checks SQL NULL values only; blank strings are not included.
- **Rolling date results change over time.** Questions 6 and 13 use `CURRENT_DATE`. Their results depend on when the queries run and on the dataset's coverage.

---

## 📁 Repository Structure

```text
Netflix-SQL-Data-Analysis/
│
├── README.md
├── solutions.sql
└── netflix_titles.csv
```

* **README.md** – Overview and documentation of the project.
* **solutions.sql** – Contains the database schema and SQL queries used to answer the 15 business questions.
* **netflix_titles.csv** – Dataset used for the analysis.

---

## 🚀 How to Run the Project

1. Install **PostgreSQL** and **pgAdmin 4**.
2. Create a new database in PostgreSQL.
3. Create the Netflix table using the provided database schema.
4. Import the `netflix_titles.csv` dataset into the table.
5. Open `solutions.sql` in pgAdmin 4.
6. Execute the SQL queries to explore the analysis and results.

---

## 📈 Project Outcome

This project helped me strengthen my SQL skills by working with a real-world dataset and solving multiple analytical problems. It provided practical experience in data exploration, aggregation, filtering, string manipulation, date analysis, and writing SQL queries to answer business-related questions.

The project also demonstrates my ability to work with structured datasets, translate analytical questions into SQL queries, and extract useful insights from data.

---

## 👤 Author

**Vaibhav Panchal**

Aspiring Data Analyst with skills in **SQL, Power BI, Excel, and Python**.

This project is part of my data analytics portfolio, demonstrating my ability to use SQL to analyse datasets and generate meaningful insights.
