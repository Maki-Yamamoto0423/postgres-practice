## SQL文法【中級編 - テーブル処理・分析】

bookテーブルを作成

```
CREATE TABLE book(
    id SERIAL PRIMARY KEY,
    name VARCHAR(30) NOT NULL,
    author VARCHAR(30) NOT NULL,
    age INTEGER
);

INSERT INTO book(name, author, age) VALUES ('SQL入門', 'Masumi', 25);
INSERT INTO book(name, author, age) VALUES ('Flutter入門', 'Masumi', 25);
INSERT INTO book(name, author) VALUES ('HTML入門', 'Hiro');
INSERT INTO book(name, author, age) VALUES ('GitHub入門2', 'Emma', 22);
INSERT INTO book(name, author, age) VALUES ('Flutter上級', 'Oliva', 45);

```

```
id | name | author | age
----+-------------+--------+-----
1 | SQL入門 | Masumi | 25
2 | Flutter入門 | Masumi | 25
3 | HTML入門 | Hiro |
 4 | GitHub入門2 | Emma | 22
5 | Flutter上級 | Oliva | 45

```

---

## DROP

bookというテーブルを削除

```
DROP TABLE book;
```

```
DROP TABLE
```

---

## ALTER

bookというテーブルの名前をbook2という名前に変更

```
ALTER TABLE book RENAME TO book2;
```

\dtでbook2になっているか確認

```
         List of relations
 Schema | Name  | Type  |  Owner
--------+-------+-------+----------
 public | book2 | table | dev_user

```

---

## SUM

bookというテーブルにおけるageの列の合計を算出する

```
SELECT SUM(age) FROM book;
```

```
 sum
-----
 117
```

---

## MAX

bookというテーブルにおけるageの列の最大値を算出する

```
SELECT MAX(age) FROM book;
```

```
 max
-----
  45
```

---

## MIN

bookというテーブルにおけるageの列の最小値を算出する

```
SELECT MIN(age) FROM book;
```

```
 min
-----
  22

```

---

## AVG

bookというテーブルにおけるageの列の平均を算出する

```
SELECT AVG(age) FROM book;
```

```
         avg
---------------------
 29.2500000000000000
```

---

## COUNT

bookというテーブルにおけるageの合計レコード数を算出する

```
SELECT COUNT(age) FROM book;
```

```
 count
-------
     4

```

## GROUP BY

各著者が書いた本の数を計算する

```
SELECT author, COUNT(author) FROM book GROUP BY author;
```

```
 author | count
--------+-------
 Oliva  |     1
 Masumi |     2
 Hiro   |     1
 Emma   |     1
```

---
