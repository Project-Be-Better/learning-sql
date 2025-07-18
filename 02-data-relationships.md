Certainly! Here's your cleaned SQL notes (Markdown-formatted, no dividers or emojis), ready to paste into GitHub or OneNote:

---

# SQL Notes: Keys, Joins, and Relationships

## Database Keys

### Key Types

| Key Type         | Purpose                                                                       | Notes                                              |
| ---------------- | ----------------------------------------------------------------------------- | -------------------------------------------------- |
| Primary Key (PK) | Uniquely identifies a row in a table. Acts as the "handle" or internal ID.    | Generally an integer auto-increment field          |
| Logical Key      | A natural identifier meaningful to the outside world. Used for search/display | What the outside world uses for lookup             |
| Foreign Key (FK) | Links a row in one table to the PK of another. Used for relationships.        | Generally an integer key pointing to another table |

### Primary Key Rules

- Never use your logical key as the primary key.
- Logical keys can and do change, albeit slowly (e.g., after name change, marriage, etc.)
- Relationships based on string fields are less efficient than those based on integers.

### Foreign Key

- A foreign key is a column containing a key that points to the primary key of another table.
- When all primary keys are integers, then all foreign keys should be integers. This is very good for performance.

---

## Normal Forms

- 1NF
- 2NF
- 3NF

---

## Creating the Music Database

We designed a normalized model with four main tables:

- `artist`: has `id` and `name` (logical key is name)
- `album`: has `id`, `title` (logical key), and `artist_id` (foreign key)
- `genre`: has `id` and `name` (logical key)
- `track`: has `id`, `title`, `length`, `rating`, `count`, `album_id`, and `genre_id`

### SQL Features and Commands

| Feature                  | Command                                                     | Note                                                            |
| ------------------------ | ----------------------------------------------------------- | --------------------------------------------------------------- |
| SERIAL PRIMARY KEY       | `id INTEGER NOT NULL DEFAULT PRIMARY KEY`                   | Automatically generates unique ID (1, 2, 3…) and sets it as PK  |
| UNIQUE on logical key    | (add `UNIQUE` constraint on a logical field like `name`)    | Ensures natural keys remain unique                              |
| FOREIGN KEY + REFERENCES | `artist_id INTEGER REFERENCES artist(id)`                   | Column refers to a PK in another table                          |
| ON DELETE CASCADE        | `artist_id INTEGER REFERENCES artist(id) ON DELETE CASCADE` | Deleting an artist will delete all related albums automatically |

---

## Using JOIN Across Tables

Once data is normalized, joins let us reconstruct meaningful views across tables.

### INNER JOIN: Connecting Matching Records

```sql
SELECT album.title, artist.name
FROM album
JOIN artist ON album.artist_id = artist.id;
```

### Full View with Multiple Joins

```sql
SELECT track.title, artist.name, album.title, genre.name
FROM track
JOIN album ON track.album_id = album.id
JOIN artist ON album.artist_id = artist.id
JOIN genre ON track.genre_id = genre.id;
```

---

## Many-to-Many Relationships

In the real world:

- A track might appear on multiple albums (e.g., original, compilation, soundtrack).
- An album can have multiple artists.
- A student can enroll in many courses, and a course can have many students.

These are many-to-many relationships — not just one-to-many.

### What is a Junction Table?

A separate table that:

- Connects two tables using foreign keys
- Models the relationship between them
- May contain extra data (e.g., user's role in a course)

---

## Hash Index vs BTREE

**Question:** Which index is best for exact match lookup but not good for prefix or sorting?

**Correct Answer:** `HASH`

| Index Type | Best For            | Weaknesses                    |
| ---------- | ------------------- | ----------------------------- |
| HASH       | Fast exact match    | Not good for sorting or range |
| BTREE      | Match, sort, ranges | Slightly slower for exact     |

---

## SQL Exercise Snippets

### Creating `make` and `model` tables

```sql
CREATE TABLE make(
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
);

DROP TABLE IF EXISTS model;

CREATE TABLE model(
  id SERIAL,
  name VARCHAR(128),
  make_id INTEGER REFERENCES make(id) ON DELETE CASCADE,
  PRIMARY KEY (id)
);

INSERT INTO make(name) VALUES('Nissan');
INSERT INTO make(name) VALUES ('Suzuki');

INSERT INTO model(name, make_id) VALUES('Altima Hybrid',1);
INSERT INTO model(name, make_id) VALUES('Altima SR',1);
INSERT INTO model(name, make_id) VALUES('Altima SR/Platinum',1);
INSERT INTO model(name, make_id) VALUES('Samurai Convertible',2);
INSERT INTO model(name, make_id) VALUES('Samurai Hardtop',2);
```

### Query joining make and model

```sql
SELECT make.name, model.name
FROM model
JOIN make ON model.make_id = make.id
ORDER BY make.name LIMIT 5;
```

---

### DROP TABLE with CASCADE

```sql
DROP TABLE course CASCADE;
```

**Meaning**: Deletes `course` **and all dependent objects** (e.g., foreign key references to `course`). Use with caution!

# Grade

![](img/02-99-grade.png)
