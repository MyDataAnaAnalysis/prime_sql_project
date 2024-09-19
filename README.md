# Amazon_Prime Movies and TV shows Data Analysis Using SQL
![Prime Logo](https://github.com/MyDataAnaAnalysis/prime_sql_project/blob/main/prime_img)

## Overview
This project involves a comprehensive analysis of Amazon Prime Video's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## Objectives
 - Examine how content is distributed between movies and TV shows.
 -  Determine the most frequent ratings for movies and TV shows.
 -  Compile and review content based on release years, countries of origin, and durations.
 -   nvestigate and classify content based on specific criteria and keywords.

## Data source
The data for this project is obtained from the Kaggle dataset:

- **Dataset Link** [Amazon Prime Movies and TV Shows Dataset](https://www.kaggle.com/datasets/shivamb/amazon-prime-movies-and-tv-shows)

## Schema
```
CREATE TABLE amazon_prime_titles (
    show_id VARCHAR(50) PRIMARY KEY,
    type VARCHAR(50),
    title VARCHAR(255),
    director VARCHAR(255),
    Featured_artists TEXT,
    country VARCHAR(255), -- Increased from 100 to 255
    date_added DATE,
    release_year INT,
    rating VARCHAR(50),
    duration VARCHAR(50),
    listed_in VARCHAR(255),
    description TEXT
);
```
```
--Confirm the Row Count of the CSV File

select * from amazon_prime_titles;

Select 
 Count (*) as total_content
From amazon_prime_titles;

--Check for the distinct show_type

SELECT DISTINCT show_type AS show_type
FROM amazon_prime_titles;
```

## Business Problems ans Solutions

```
--1. Count the number of Movies vs TV Shows

Select 
 show_type,
 count(*) as total_content
 from amazon_prime_titles
 Group by show_type
```
```
-- 2. Find the most common rating for movies and TV shows

WITH RatingCounts AS (
    SELECT 
        show_type, 
        rating,
        COUNT(*) AS total_count
    FROM amazon_prime_titles
    GROUP BY show_type, rating
),
RankedRatings AS (
    SELECT 
        show_type,
        rating,
        total_count,
        RANK() OVER (PARTITION BY show_type ORDER BY total_count DESC) AS ranking
    FROM RatingCounts
)
SELECT 
    show_type, 
    rating,
    total_count,
    ranking
FROM RankedRatings
WHERE ranking = 1
ORDER BY show_type;

NB:
The "WITH" clause in SQL is used to define Common Table Expressions (CTEs), which are temporary result sets that you can reference within a SELECT, INSERT, UPDATE, or DELETE statement. CTEs make it easier to organize complex queries, improve readability, and break down a problem into simpler parts.
Purpose: This CTE calculates the count of each rating for each show_type.
Components:
RatingCounts: The name of the CTE. It acts like a temporary table for the duration of the query.
SELECT show_type, rating, COUNT(*) AS total_count: Aggregates the data by show_type and rating, counting the occurrences of each rating.
FROM amazon_prime_titles: The source table.
GROUP BY show_type, rating: Groups the results by show_type and rating to compute the count for each combination.

```
```
-- 3. List all movies released in a specific year (e.g., 2020)

  Select *
  from amazon_prime_titles
  where 
   show_type = 'Movie'
   And
   release_year = 2020
```
```
-- 4. Find the top 5 countries with the most content on amazonPrime

-- Query to find the top 5 countries with the most content
WITH CountryCounts AS (
    SELECT
        value AS country,
        COUNT(*) AS content_count
    FROM
        amazon_prime_titles
    CROSS APPLY
        STRING_SPLIT(country, ',')
    GROUP BY
        value
)
SELECT TOP 5
    country,
    content_count
FROM
    CountryCounts
ORDER BY
    content_count DESC;
```
```
-- 5. Identify the longest movie

Select * From amazon_prime_titles
 where 
  show_type = 'Movie'
  And
  duration = (Select Max(duration) from amazon_prime_titles)
 
 --6. Find content added in the last 5 years

 Select 
  * From amazon_prime_titles
 where 
   date_added >= DATEADD(YEAR, -5, GETDATE());
```
```
--7. Find all the movies/TV shows by director 'Josh Webber'!

Select * From amazon_prime_titles
where director Like '%Mark Knight%'
```
```
--8. List all TV shows with more than 5 seasons

SELECT *
FROM 
    amazon_prime_titles
WHERE 
    show_type = 'TV Show'
    AND CAST(LEFT(duration, CHARINDEX(' ', duration + ' ') - 1) AS INT) > 5;
```
```
--9. Count the number of content items in each genre

SELECT 
    value AS genre,
    COUNT(show_id) AS total_content
FROM 
    amazon_prime_titles
CROSS APPLY 
    STRING_SPLIT(listed_in, ',')
GROUP BY 
    value;
```
```
-- 10.Find each year and the average numbers of content release in United Kingdom on prime. 
--return top 5 year with highest avg content release!

Select release_year,
Count(*) 
From amazon_prime_titles
where country = 'United Kingdom'
Group by release_year

SELECT Top 5
    release_year,
    COUNT(*) AS content_count,
	AVG(COUNT(*)) OVER () AS avg_content_release
From amazon_prime_titles
Where country = 'United Kingdom'
GROUP BY release_year
Order By content_count Desc;
```
```
--11. List all movies that are documentaries

Select Title, listed_in
From amazon_prime_titles
Where listed_in Like 'Documentary'
```
```
--12. Find all content without a director

Select *
From amazon_prime_titles
where director is Null
```
```
--13. Find how many movies actor 'Salman Khan' appeared in last 10 years!

Select 
    Featured_artists,
    COUNT(*) AS movie_count
From 
    amazon_prime_titles
Where 
    Featured_artists LIKE '%Salman Khan%'
    AND release_year >= YEAR(GETDATE()) - 10
Group By 
    Featured_artists;
```
```
--14. Find the top 10 actors who have appeared in the highest number of movies produced in United States.

WITH ActorMovies AS (
    SELECT 
        TRIM(value) AS Actor
    FROM 
        amazon_prime_titles
    CROSS APPLY 
        STRING_SPLIT(Featured_artists, ',')
    WHERE 
        country LIKE '%United States'
)
SELECT 
    TOP 10
    Actor AS Actors,
    COUNT(*) AS total_content
FROM 
    ActorMovies
GROUP BY 
    Actor
ORDER BY 
    total_content DESC;
```
```
--15.Categorize the content based on the presence of the keywords 'kill' and 'violence' in 
--the description field. Label content containing these keywords as 'Bad' and all other 
--content as 'Good'. Count how many items fall into each category.

WITH CategorizedContent AS (
    SELECT
        CASE
            WHEN Descriptions LIKE '%kill%' OR Descriptions LIKE '%violence%'
                THEN 'Bad'
            ELSE 'Good'
        END AS category
    FROM amazon_prime_titles
)
SELECT 
    category,
    COUNT(*) AS item_count
FROM CategorizedContent
GROUP BY category;
```










