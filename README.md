# Apple-Store--SQL-Analysis

**Data:** Data about the apps of all genre from the Apple Store
**Project:** Analysing the data to provide actionable recommendations to App Developers on what type of apps are benefical to create.
**Target Audience:** App Developers

**SQL Code:**
CREATE TABLE appleStore_description AS

SELECT * from appleStore_description1

UNION ALL

SELECT * from appleStore_description2

union ALL

SELECT * from appleStore_description3

UNION ALL

SELECT * from appleStore_description4

** Exploratory Data Analysis **

-- Check number of Unique Apps in AppleStore --

select count(DISTINCT id) as Unique_Appids
from AppleStore

select count(DISTINCT id) as Unique_Appids
from appleStore_description

-- Missing Values --

select COUNT(*) as MissingValues
from AppleStore
where track_name is NULL or user_rating is null or prime_genre is null

select COUNT(*) as MissingValues
from appleStore_description
where track_name is NULL or app_desc is null

-- Apps per genre

SELECT prime_genre, count(*) as Number_of_apps
FROM AppleStore
Group by prime_genre
order by Number_of_apps DESC

-- Understanding User Ratings --
select min(user_rating) as Min_Rating,
max(user_rating) as Max_Rating,
avg(user_rating) AS Avg_Rating
from AppleStore

** Data Analysis **

-- Checking whether paid apps have more user rating than unpaid apps --

select CASE

      when price > 0 then "PAID"
      else "FREE"
      END as App_Type,
 avg(user_rating) as Avg_Rating
 from AppleStore
 group by App_Type
 
 -- Checking if apps that support more languages are more highly rated --
 
 select CASE
            when lang_num <10 then "Less than 10 languages"
            when lang_num BETWEEN 10 and 30 then " 10-30 languages"
            else "More than 30 languages"
            end as Supported_Languages,
            avg(user_rating) as Avg_Ratings
 from AppleStore
 GROUP BY Supported_Languages
 Order by Avg_Ratings DESC
 
 -- Check which genres have lowest rating --
 
 select prime_genre, avg(user_rating) as Avg_Ratings
 from AppleStore
 GROUP by prime_genre
 order by Avg_Ratings ASC
 
 -- Check if there is a co-relation between app description length and user ratings --
 
 SELECT case 
 when length(b.app_desc)< 500 then "Short Desc"
 when length(b.app_desc) BETWEEN 500 and 1000 then "Medium Desc"
 else "Long Desc"
 end as Description_Length, avg(user_rating) as Avg_Ratings
 
 from AppleStore as a 
 join appleStore_description as b 
 on a.id= b.id
 
 GROUP by Description_Length
 Order By Avg_Ratings DESC
 
 -- 5 Top rated apps in each genre --
 
 select prime_genre, track_name, user_rating, a.Rank
 
 from (
   SELECT prime_genre, track_name, user_rating,
   rank() over(partition by prime_genre order by user_rating DESC, rating_count_tot DESC) as Rank
   
   from AppleStore ) as a 
   
WHERE a. Rank BETWEEN 1 and 5

 -- Top rated apps in each genre --
 
 select prime_genre, track_name, user_rating
 
 from (
   SELECT prime_genre, track_name, user_rating,
   rank() over(partition by prime_genre order by user_rating DESC, rating_count_tot DESC) as Rank
   
   from AppleStore ) as a 
   
WHERE a. Rank=1
   

**Reccomendations after Analysis:**
--> Paid Vs Free Apps: Paid apps have higher ratings on average than unpaid apps. This could be beacause paid apps have more user engagement and are generraly perceived to have better features than their free
competitors. So, there is generally no harm in charging for your app as long as the app features live upto the cost.

--> Languages Supported: Your app need not support a very extensive amount of languages. But make sure the right languages are supported.

--> Market: The genres of Books and Finance have lower user ratings. This presents an open market. Making high- quality apps in this genre could ensure success.

--> App Description: A detailed, well-crafted app description has higher probability of making the app popular.

--> High competition: Entertainment and Games genre have more number of apps, so market penetration maybe difficult.
