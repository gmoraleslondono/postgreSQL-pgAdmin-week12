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
