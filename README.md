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
* Identify popular genres and content categories.
* Explore actors and directors featured in Netflix content.
* Categorise content based on specific keywords and descriptions.
* Apply SQL techniques to answer practical business questions.

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

## 🔍 Business Questions

The project answers the following analytical questions:

1. Count the number of Movies and TV Shows.
2. Find the most common rating for Movies and TV Shows.
3. List all Movies released in a specific year.
4. Find the top 5 countries with the most content on Netflix.
5. Identify the longest Movie.
6. Find content added in the last 5 years.
7. Find all Movies and TV Shows by a specific director.
8. List TV Shows with more than 5 seasons.
9. Count the number of content items in each genre.
10. Find the average number of Netflix content releases in India by year and return the top 5 years with the highest average content releases.
11. List all Movies that are Documentaries.
12. Find all content without a director.
13. Find how many Movies an actor appeared in during the last 10 years.
14. Find the top 10 actors who have appeared in the highest number of Movies produced in India.
15. Categorise Netflix content as **Good** or **Bad** based on the presence of keywords such as `kill` and `violence` in the description.

---

## 💡 SQL Concepts Used

This project demonstrates the use of:

* `SELECT`
* `WHERE`
* `GROUP BY`
* `ORDER BY`
* `HAVING`
* Aggregate functions such as `COUNT()`
* `DISTINCT`
* `LIMIT`
* `CASE` statements
* Common Table Expressions (CTEs)
* String manipulation functions
* Date functions
* `UNNEST`
* `STRING_TO_ARRAY`
* Data type conversion
* Pattern matching

---

## 📊 Key Insights

* Analysed the overall distribution of Movies and TV Shows available on Netflix.
* Identified countries contributing the highest volume of Netflix content.
* Explored the most frequently occurring content ratings and genres.
* Analysed content release patterns across different years.
* Identified actors and directors with significant representation in the dataset.
* Examined content duration and TV show season patterns.
* Used text-based analysis to categorise content based on keywords in descriptions.

---

## 📁 Repository Structure

```text
Netflix-SQL-Data-Analysis/
│
├── netflix_titles.csv
├── netflix_analysis.sql
└── README.md
```

---

## 🚀 How to Run the Project

1. Install PostgreSQL and pgAdmin 4.
2. Create a new database in PostgreSQL.
3. Create the Netflix table using the SQL script.
4. Import the `netflix_titles.csv` dataset into the table.
5. Open the SQL query file in pgAdmin 4.
6. Execute the queries to explore the analysis and results.

---

## 📈 Project Outcome

This project helped me strengthen my SQL skills by working with a real-world dataset and solving multiple analytical problems. It provided practical experience in data exploration, aggregation, filtering, string manipulation, date analysis, and writing SQL queries to answer business-related questions.

---

## 👤 Author

**Vaibhav Panchal**

Aspiring Data Analyst with skills in SQL, Power BI, Excel, and Python.

This project is part of my data analytics portfolio, demonstrating my ability to use SQL to analyse datasets and generate meaningful insights.
