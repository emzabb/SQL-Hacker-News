# Project Description:

## Introduction

Hacker News, a well-known website administered by Y Combinator, has become an integral part of the tech industry's online community. It serves as a platform for sharing the latest news, showcasing innovative projects, seeking answers to burning questions, and much more. With its dedicated user base, Hacker News has emerged as a hub for tech enthusiasts, professionals, and hobbyists to engage in discussions and exchange valuable insights.

In this project, my mission is to dive into a dataset, aptly named 'hacker_news,' which contains stories shared on Hacker News since 2007. This dataset presents an opportunity to uncover and analyze the dynamics of this online community.

## Data Overview

 **hacker_news Table:** title, user, score, timestamp, score

## Project Goals

In the course of this project, we will embark on a comprehensive journey of data exploration, analysis, and discovery. Our primary objectives include finding common trends on the Hacker News site and looking into potentially fake posts.

As I embark on this journey, I am committed to maintaining a rigorous and structured approach to data analysis. I aim to uncover the stories within the data and contribute to the collective knowledge of the tech world.

    -- 1
    
    -- Start by getting a feel for the hacker_news table. Find the top 5 highest scoring stories.
    
    SELECT title, score
    
    FROM hacker_news
    
    ORDER  BY score DESC
    
    LIMIT  5;

![enter image description here](https://i.ibb.co/sbygQ0V/1.png)

  

    -- 2
    
    -- Write multiple queries to find out if it is true that a majority of points are made by a small percentage of users (1-9-90 Rule).
    
      
    
    SELECT  SUM(score)  AS  'total_scores'
    
    FROM hacker_news;
    
    
    
    SELECT  user, SUM(score)  AS  'user_total_score'
    
    FROM hacker_news
    
    GROUP  BY  1
    
    HAVING  SUM(score) > 200
    
    ORDER  BY  2  DESC;
    
    
    
    SELECT  ROUND((((517 + 309 + 304 + 282) / 6366.0) * 1.0), 2)  AS  'percent_score_from_same_users';

![enter image description here](https://i.ibb.co/wNzS9PH/2.png)

  

    -- 3
    
    -- Find how many times users have posted a prank link.
    
      
    
    SELECT  user, COUNT(url)
    
    FROM hacker_news
    
    WHERE url LIKE  'https://www.youtube.com/watch?v=dQw4w9WgXcQ'
    
    GROUP  BY  user
    
    ORDER  BY  COUNT(*)  DESC;

![enter image description here](https://i.ibb.co/9vSDgfp/3.png)

  

    -- 4
    
    -- Determine which sites feed Hacker News most.
    
      
    
    
    SELECT  CASE
    
    WHEN url LIKE  '%github%'  THEN  'GitHub'
    
    WHEN url LIKE  '%medium.com%'  THEN  'Medium'
    
    WHEN url LIKE  '%nytimes.com%'  THEN  'New York Times'
    
    WHEN url LIKE  '%wikipedia.org%'  THEN  'Wikipedia'
    
    WHEN url LIKE  '%newyorker.com%'  THEN  'The New Yorker'
    
    WHEN url LIKE  '%techcrunch.com%'  THEN  'Tech Crunch'
    
    WHEN url LIKE  '%amazon.com%'  THEN  'Amazon'
    
    ELSE  'Other'
    
    END  AS  'Source',
    
    COUNT  (*)  AS  'stories_count'
    
    FROM hacker_news
    
    GROUP  BY  1
    
    ORDER  BY  2;

![enter image description here](https://i.ibb.co/JHTz9Hr/Screen-Shot-2023-11-08-at-13-41-17.png)

  

    -- 5
    
    -- Determine the best time to post a story, start by examining the timestamp column.
    
      
    
    SELECT  timestamp
    
    FROM hacker_news
    
    LIMIT  10;
    
      
    
    -- Use the strftime() function to view the timestamp VS the hour at which a story is posted.
    
    SELECT  timestamp, strftime('%H', timestamp)
    
    FROM hacker_news
    
    GROUP  BY  1
    
    LIMIT  10;

![enter image description here](https://i.ibb.co/LhqnGbw/5.png)

  

    -- Determine the hour, average score, and count of stories for each hour.
    
    SELECT strftime('%H', timestamp),
    
    AVG(score)  AS,
    
    COUNT(title)  AS
    
    FROM hacker_news
    
    GROUP  BY  1;

![enter image description here](https://i.ibb.co/Nt292TW/6.png)

  

    -- Clean up the results and find the best hour to post.
    
    SELECT strftime('%H', timestamp)  AS  'Hour',
    
    ROUND(AVG(score), 1)  AS  'Average Score',
    
    COUNT(*)  AS  'Number of Stories'
    
    FROM hacker_news
    
    WHERE  timestamp  IS  NOT  NULL
    
    GROUP  BY  1
    
    ORDER  BY  2  DESC;

![enter image description here](https://i.ibb.co/pZsPQpp/7.png)
