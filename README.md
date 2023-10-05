# Apple-Store--SQL-Analysis

**Data:** Data about the apps of all genre from the Apple Store<br>
**Project:** Analysing the data to provide actionable recommendations to App Developers on what type of apps are benefical to create.<br>
**Target Audience:** App Developers<br>
<br><br>

**SQL Code:**<br>
CREATE TABLE appleStore_description AS

SELECT * from appleStore_description1

UNION ALL

SELECT * from appleStore_description2

union ALL

SELECT * from appleStore_description3

UNION ALL

SELECT * from appleStore_description4

**Exploratory Data Analysis** <br>

-- Check number of Unique Apps in AppleStore --

select count(DISTINCT id) as Unique_Appids
from AppleStore
<br>
select count(DISTINCT id) as Unique_Appids
from appleStore_description
<br><br>

-- Missing Values --

select COUNT(*) as MissingValues
from AppleStore
where track_name is NULL or user_rating is null or prime_genre is null
<br>

select COUNT(*) as MissingValues
from appleStore_description
where track_name is NULL or app_desc is null
<br>

-- Apps per genre
<br>

SELECT prime_genre, count(*) as Number_of_apps
FROM AppleStore
Group by prime_genre
order by Number_of_apps DESC
<br>

-- Understanding User Ratings --
select min(user_rating) as Min_Rating,
max(user_rating) as Max_Rating,
avg(user_rating) AS Avg_Rating
from AppleStore

** Data Analysis **

-- Checking whether paid apps have more user rating than unpaid apps --

select CASE<br>


      when price > 0 then "PAID" <br>

      else "FREE"<br>

      END as App_Type,<br>

 avg(user_rating) as Avg_Rating<br>

 from AppleStore<br>

 group by App_Type<br>

 <br>
<br>

 -- Checking if apps that support more languages are more highly rated --
 
 select CASE<br>

            when lang_num <10 then "Less than 10 languages"<br>

            when lang_num BETWEEN 10 and 30 then " 10-30 languages"<br>

            else "More than 30 languages"<br>

            end as Supported_Languages,<br>

            avg(user_rating) as Avg_Ratings<br>

 from AppleStore<br>

 GROUP BY Supported_Languages<br>

 Order by Avg_Ratings DESC<br>
<br>
<br>

 
 -- Check which genres have lowest rating --
 
 select prime_genre, avg(user_rating) as Avg_Ratings<br>

 from AppleStore<br>

 GROUP by prime_genre<br>

 order by Avg_Ratings ASC<br>
<br>
<br>

 
 -- Check if there is a co-relation between app description length and user ratings --
 
 SELECT case <br>

 when length(b.app_desc)< 500 then "Short Desc"<br>

 when length(b.app_desc) BETWEEN 500 and 1000 then "Medium Desc"<br>

 else "Long Desc"<br>

 end as Description_Length, avg(user_rating) as Avg_Ratings<br>

 
 from AppleStore as a <br>

 join appleStore_description as b <br>

 on a.id= b.id<br>

 
 GROUP by Description_Length<br>

 Order By Avg_Ratings DESC<br>
<br>
<br>

 
 -- 5 Top rated apps in each genre --
 
 select prime_genre, track_name, user_rating, a.Rank<br>

 
 from (<br>

   SELECT prime_genre, track_name, user_rating,<br>

   rank() over(partition by prime_genre order by user_rating DESC, rating_count_tot DESC) as Rank<br>

   
   from AppleStore ) as a <br>

   
WHERE a. Rank BETWEEN 1 and 5<br>
<br>
<br>


 -- Top rated apps in each genre --
 
 select prime_genre, track_name, user_rating<br>

 
 from (<br>

   SELECT prime_genre, track_name, user_rating,<br>

   rank() over(partition by prime_genre order by user_rating DESC, rating_count_tot DESC) as Rank<br>

   
   from AppleStore ) as a <br>

   
WHERE a. Rank=1<br>
<br>
<br>

   

**Reccomendations after Analysis:**
--> Paid Vs Free Apps: Paid apps have higher ratings on average than unpaid apps. This could be beacause paid apps have more user engagement and are generraly perceived to have better features than their free
competitors. So, there is generally no harm in charging for your app as long as the app features live upto the cost.

--> Languages Supported: Your app need not support a very extensive amount of languages. But make sure the right languages are supported.

--> Market: The genres of Books and Finance have lower user ratings. This presents an open market. Making high- quality apps in this genre could ensure success.

--> App Description: A detailed, well-crafted app description has higher probability of making the app popular.

--> High competition: Entertainment and Games genre have more number of apps, so market penetration maybe difficult.
