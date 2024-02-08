
# INSTAGRAM USER ANALYTICS

This project is about analysing user behaviour on the Instagram platform and providing insights to the product and management teams to improve the user experience, launch successful marketing campaigns, and assess the platform's performance. The analysis will involve querying the provided 
database to find information such as the oldest users, inactive users, contest winners, commonly used hashtags, and user engagement metrics.


# APPROACH:
1. Understanding the database schema and data
2. Identifying the relevant data to answer the questions posed by the product and management teams
3. Writing SQL queries to extract the relevant data
4. Cleaning and analyzing the data using statistical and visualization tools
5. Providing insights and recommendations based on the analysis.

# INSIGHTS:
1. Finding the oldest users of Instagram can help in rewarding the most loyal users and improve brand loyalty.
2. Identifying users who have never posted on Instagram can help in reminding them to start posting and increase user engagement.
3. Identifying the winner of a contest based on the most likes can help in declaring the winner and increasing brand engagement.
4. Identifying the top 5 most commonly used hashtags can help in improving brand visibility and reach on Instagram.
5. Knowing the day of the week with the most user registrations can help in scheduling an ad campaign for better user engagement and reach.
6. Calculating the average number of posts per user can help in assessing user engagement and activity on the platform.
7. Identifying and analyzing the data on bots and fake accounts can help in identifying potential issues and improving the platform's credibility.

# RESULTS:
I have demonstrated the capability to provide insights and information based on a given database schema. This can be helpful for businesses or individuals looking to analyse their data and derive insights to inform decision-making. Overall, my contribution to this project has been to showcase the 
power of SQL queries for data analysis.

# Queries

# 1. Find the 5 oldest users of the Instagram from the database provided

SELECT username, created_at
FROM users
ORDER BY created_at ASC
LIMIT 5;

# 2. Find the users who have never posted a single photo on Instagram

SELECT users.id, users.username\
FROM users\
LEFT JOIN photos ON users.id = photos.user_id\
WHERE photos.user_id IS NULL;

# 3. Identify the winner of the contest and provide their details to the team

SELECT photos.id AS photo_id, users.username\
FROM photos\
LEFT JOIN users ON photos.user_id = users.id\
WHERE photos.id = (\
SELECT likes.photo_id\
FROM likes\
GROUP BY likes.photo_id\
ORDER BY COUNT(likes.user_id) DESC\
LIMIT 1\
);

# 4. Your Task: Identify and suggest the top 5 most commonly used hashtags on the platform

SELECT tags.tag_name, COUNT(*) AS tag_count\
FROM tags\
GROUP BY tags.tag_name\
ORDER BY tag_count DESC\
LIMIT 5;

# 5. Your Task: What day of the week do most users register on? Provide insights on when to schedule an ad campaign

SELECT DAYNAME('created_at') AS day_of_week, COUNT(*) AS num_users\
FROM users\
GROUP BY DAYNAME('created_at')\
ORDER BY num_users DESC\
LIMIT 1;

# 6. Provide how many times does average user posts on Instagram. Also, provide the total number of photos on Instagram/total number of users

SELECT COUNT(*) AS total_photos, COUNT(DISTINCT user_id) AS\
total_users,\
COUNT(*) / COUNT(DISTINCT user_id) AS photos_per_user\
FROM photos;

# 7. Provide data on users (bots) who have liked every single photo on the site (since any normal user would not be able to do this).

SELECT user_id FROM likes\
GROUP BY user_id\
HAVING COUNT(DISTINCT photo_id) = (SELECT COUNT(*) FROM photos);

