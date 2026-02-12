## SQL文法【初級編 - 絞り込み】

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

## IS NOT NULL

ageがNULLではないレコードをbookから抽出

```
SELECT * FROM book WHERE age IS NOT NULL;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
  4 | GitHub入門2 | Emma   |  22
  5 | Flutter上級 | Oliva  |  45
```

---

## AND

nameが"SQL入門"かつauthorが"Masumi"のレコードをbookから抽出

```
SELECT * FROM book WHERE name='SQL入門' AND author='Masumi';
```

```
 id |  name   | author | age
----+---------+--------+-----
  1 | SQL入門 | Masumi |  25
```

---

## OR

nameが"SQL入門"もしくはauthorが"Masumi"のレコードをbookから抽出する

```
SELECT * FROM book WHERE name='SQL入門' OR author='Masumi';
```

```
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
```

---

## IN

(20, 25, 30)のいずかに一致するageを持ったレコードをbookから抽出する

```
SELECT * FROM book WHERE age IN (20, 25, 30);
```

```
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
```

---

## NOT IN

(20, 25, 30)のいずれでもないageを持ったレコードをbookから抽出する

```
SELECT * FROM book WHERE age NOT IN (20, 25, 30);
```

```
 id |    name     | author | age
----+-------------+--------+-----
  4 | GitHub入門2 | Emma   |  22
  5 | Flutter上級 | Oliva  |  45
```

---

## LIKE

後方一致検索
"Flutter"の前後に0文字以上の文字列があるレコードをbookから抽出する

```
SELECT * FROM book WHERE name LIKE '%Flutter%';
```

```
 id |    name     | author | age
----+-------------+--------+-----
  2 | Flutter入門 | Masumi |  25
  5 | Flutter上級 | Oliva  |  45
```

前方一致検索
"入門"の前に0文字以上の文字列があり、後に1文字の文字列があるレコードをbookから抽出する

```
FROM book WHERE name LIKE '%入門_';
```

```
 id |    name     | author | age
----+-------------+--------+-----
  4 | GitHub入門2 | Emma   |  22
```

---

## BETWEEN

20から30歳の間のageのレコードをbookから抽出する

```
SELECT * FROM book WHERE age BETWEEN 20 AND 30;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
  4 | GitHub入門2 | Emma   |  22
```
