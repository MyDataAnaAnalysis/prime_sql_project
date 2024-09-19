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
/*
The WITH clause in SQL is used to define Common Table Expressions (CTEs), which are temporary result sets that you can reference within a SELECT, INSERT, UPDATE, or DELETE statement. CTEs make it easier to organize complex queries, improve readability, and break down a problem into simpler parts.
Purpose: This CTE calculates the count of each rating for each show_type.
Components:
RatingCounts: The name of the CTE. It acts like a temporary table for the duration of the query.
SELECT show_type, rating, COUNT(*) AS total_count: Aggregates the data by show_type and rating, counting the occurrences of each rating.
FROM amazon_prime_titles: The source table.
GROUP BY show_type, rating: Groups the results by show_type and rating to compute the count for each combination.
*/
```




