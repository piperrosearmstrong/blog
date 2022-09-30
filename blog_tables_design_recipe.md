# Blog Tables Design Recipe

## 1. Extract nouns from the user stories or specification

```
As a blogger
So I can write interesting stuff
I want to write posts having a title.

As a blogger
So I can write interesting stuff
I want to write posts having a content.

As a blogger
So I can let people comment on interesting stuff
I want to allow comments on my posts.

As a blogger
So I can let people comment on interesting stuff
I want the comments to have a content.

As a blogger
So I can let people comment on interesting stuff
I want the author to include their name in comments.
```

```
Nouns:
post, title, content, comment, comment_content, comment author
```

## 2. Infer the Table Name and Columns

Put the different nouns in this table. Replace the example with your own nouns.

| Record                | Properties          |
| --------------------- | ------------------  |
| post                  | title, content
| comment               | content, author

1. Name of the first table (always plural): `posts` 

    Column names: `title`, `content`

2. Name of the second table (always plural): `comments` 

    Column names: `content`, `author`

## 3. Decide the column types.

```
Table: posts
id: SERIAL
title: text
content: text

Table: comments
id: SERIAL
content: text
author: text
```

## 4. Decide on The Tables Relationship

1. Can one post have many comments? (Yes)
2. Can one comment have many posts? (No)

You'll then be able to say that:

1. **[Posts] has many [Comments]**
2. And on the other side, **[Comments] belongs to [Posts]**
3. In that case, the foreign key is in the table [Comments]

```
-> Therefore, the foreign key is on the comments table. (post_id)
```

## 4. Write the SQL.

```sql
-- EXAMPLE
-- file: posts_table.sql

-- Replace the table name, columm names and types.

-- Create the table without the foreign key first.
CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  title text,
  content text
);

-- Then the table with the foreign key first.
CREATE TABLE comments (
  id SERIAL PRIMARY KEY,
  content text,
  author text,
-- The foreign key name is always {other_table_singular}_id
  post_id int,
  constraint fk_post foreign key(post_id)
    references posts(id)
    on delete cascade
);

```

## 5. Create the tables.

```bash
psql -h 127.0.0.1 database_name < albums_table.sql
```
