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

## Business Problems ans Solutions



