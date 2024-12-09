## Create a table with 4 columns (id, name, email, created_at)

```
CREATE TABLE users (
id SERIAL PRIMARY KEY, // auto increment
name VARCHAR(50) NOT NULL, // maximum length of 50
email VARCHAR(150) UNIQUE NOT NULL,
created_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

## Show table and content

```
SELECT * FROM users;
```

## Insert new transaction

```
INSERT INTO users (name, email)
VALUES
('Vera Mora', 'vera@gmail.com'),
('Gloria Tomas', 'tomas@hotmail.com');
```

## Update a transaction

```
UPDATE users
SET name = 'Gloria Glloria'
WHERE id = 2;
```

## Delete a transaction

```
DELETE FROM users
WHERE id = 1;
```

## Foreign key

```
CREATE TABLE profiles (
id SERIAL PRIMARY KEY,
user_id INT UNIQUE,
FOREIGN KEY (user_id) REFERENCES users(id),
bio TEXT
);
```

## Using the foreign key

```
INSERT INTO profiles (user_id, bio)
VALUES
(4, 'loves coding'); // 4 user id already exist in the table users
```

## Create several tables at the same time

```
CREATE TABLE players (
id SERIAL PRIMARY KEY,
name VARCHAR(100),
join_date DATE
);

CREATE TABLE games(
id SERIAL PRIMARY KEY,
title VARCHAR(100),
genre VARCHAR(100)
);
```

## Create a filtered table

### relation between 2 tables

```
SELECT users.name, profile.bio
FROM users
JOIN profiles ON user.id = profile.user_id
```

### relation between 3 tables

```
SELECT players.name, games.title, score
FROM scores
INNER JOIN players ON players.id = scores.player_id
INNER JOIN games ON games.id = scores.game_id
```

## composite primary key.

It uses two columns to get a row unique.
Having the student id and the course id as a compositions, it allows the same student sing up for different courses and it allows the same course to have different students.
So, student can enroll in multiple courses, but since the id should be unique a student can not enroll twice to the same course.

```
CREATE TABLE student_courses(
student_id INT,
course_id INT,
PRIMARY KEY (student_id, course_id),
FOREIGN KEY (student_id) REFERENCES students(id),
FOREIGN KEY (course_id) REFERENCES courses(id),
);
```

## Adding data to a composite table

```
INSERT INTO student_courses (student_id, course_id)
VALUES
(1,1), (1,2), (2,1);
```

## create a composite table

```
SELECT students.name, courses.title
FROM students
JOIN student_courses ON student.id = student_courses.student_id
JOIN courses ON courses.id  = student_courses.course_id
```

## Group by

This will count the number of books by genre

```
SELECT genre,
COUNT(*) as book_count
FROM books
GROUP BY genre;
```

## JOIN ON

Find all books and their respective authors

```
SELECT books.title, authors.name
FROM books
JOIN authors ON books.author_id = authors.id;
```

## ORDER BY

It will look for books published before 2000, and then order them by published year

```
SELECT title, published_year
FROM books
WHERE published_year < 2000
ORDER BY published_year DESC;
```

## GROUP BY and ORDER BY

It will find the top 3 players with the highest total scores across all games.

```
SELECT player_id, SUM(score) AS total_score
FROM scores
GROUP BY player_id
ORDER BY total_score DESC
LIMIT 3;
```

## LEFT OUTER JOIN

It will list all players who haven’t played any games yet

```
SELECT players.id, players.name
FROM players
LEFT OUTER JOIN scores ON players.id = scores.player_id
WHERE scores.player_id IS NULL;
```

## GROUP BY and COUNT()

It will find out which game genre is the most popular among players.

```
SELECT games.genre, COUNT(scores.player_id) AS player_count
FROM games
JOIN scores ON games.id = scores.game_id
GROUP BY games.genre
ORDER BY player_count DESC
LIMIT 1;
```

## WHERE

It will find all players who joined in the last 30 days

```
SELECT id, name, join_date
FROM players
WHERE join_date >= NOW() - INTERVAL '30 days';
```

## JOIN and GROUP BY

It will find out which game each player has played the most times. Show the player’s name and the game title.

```
SELECT
    players.name AS player_name,
    games.title AS game_title,
    COUNT(scores.id) AS play_count
FROM
    scores
JOIN
    players ON players.id = scores.player_id
JOIN
    games ON games.id = scores.game_id
GROUP BY
    players.name, games.title
ORDER BY
    play_count DESC;
```
