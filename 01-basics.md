# History of Relational Databases

NIST : National Institure of Standard of Technology
Elizabeth Fong

SQL is non procedural - It focuses on the results rather than the procedures

The critical part of the database is called CRUD - Create - Read - Update - Delete

Data is modelled as a network

SQL Server - Microsoft
MySQL - Bought by Oracle
Oracle - Gold Standard, most difficult
Postgres - Open Source

# Elizabeth Fong: Creating the SQL Database Standards

There were multiple databases from multiple vendors

Elizabeth Fong

Metadata is also called as the Schema

# Operations

## INSERT

```bash
INSERT INTO tablename (columns..) VALUES (values);

INSERT INTO users (name, email) VALUES ('name','example@gmail.com');
```

## UPDATE

```bash
UPDATE users SET name='Sreeraj' WHERE email='sreeraj_ec@hotmail.com';
```

> Always include `WHERE` otherwise it will update the entire table

## DELETE

```bash
DELETE FROM tablename WHERE value='value';

DELETE FROM users WHERE email='example@gmail.com';
```

## SELECT

```bash
# Basic
SELECT * FROM tablename;

SELECT * FROM users;
SELECT * FROM users WHERE email='example@gmail.com';
```

## ORDER BY

```bash
SELECT * FROM users ORDER BY email;
SELECT * FROM users ORDER BY email DESC;
```

> It is Ascending by default. Use ASC or DESC for sorting

## LIKE and Wildcards

```bash
SELECT * FROM WHERE name LIKE 'pattern-to-find';
SELECT * FROM users WHERE email LIKE '%gmail.com%';
```

## LIMIT and OFFSET for Pagination

```bash
SELECT * FROM users ORDER BY email LIMIT 2 OFFSET 5;
```

> This is for optimising the performance

## COUNT

```bash
SELECT  COUNT(*) FROM tablename;
SELECT  COUNT(*) FROM users;
```

# Summary

- SQL is not procedural: no loops or if statements, but commands behave like loops internally.
- SQL is an abstraction: You don’t need to know how the data is physically accessed.
- It was designed to be simple and powerful
