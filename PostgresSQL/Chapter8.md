## SQL文法【初級編 - 絞り込み

bookテーブルを作成

```
CREATE TABLE book(
    id SERIAL PRIMARY KEY,
    name VARCHAR(30) NOT NULL,
    author VARCHAR(30) NOT NULL,
    age INT
);

INSERT INTO book(name, author, age) VALUES ('SQL入門', 'Masumi', 25);
INSERT INTO book(name, author, age) VALUES ('Flutter入門', 'Masumi', 25);
INSERT INTO book(name, author) VALUES ('HTML入門', 'Hiro');
INSERT INTO book(name, author, age) VALUES ('GitHub入門2', 'Emma', 22);
INSERT INTO book(name, author, age) VALUES ('Flutter上級', 'Oliva', 45);

SELECT * FROM book;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
  3 | HTML入門    | Hiro   |
  4 | GitHub入門2 | Emma   |  22
  5 | Flutter上級 | Oliva  |  45
```

---

## IS NULL

ageがNULLであるレコードをbookから抽出

```
SELECT * FROM book WHERE age IS NULL;
```

```
 id |   name   | author | age
----+----------+--------+-----
  3 | HTML入門 | Hiro   |
```

---
