## 07SQL文法【入門編 - 基本文法】

book Tableを作成

```
CREATE TABLE book(
    id SERIAL PRIMARY KEY,
    name VARCHAR(30),
    author VARCHAR(30)
);

INSERT INTO book(name, author) VALUES ('SQL入門','Masumi');
INSERT INTO book(name, author) VALUES ('Flutter入門','Masumi');
INSERT INTO book(name, author) VALUES ('HTML入門','Hiro');

SELECT * FROM book;
```

---

テーブルの中身を全て抽出

```
SELECT * FROM book;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |
  2 | Flutter入門 | Masumi |
  3 | HTML入門    | Hiro   |
```

---

bookテーブルのname列を抽出

```
SELECT name FROM book;
```

```
    name
-------------
 SQL入門
 Flutter入門
 HTML入門

```

---

Masumiの本だけ抽出

```
SELECT * FROM book WHERE author='Masumi';

```

```
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |
  2 | Flutter入門 | Masumi |
```

## UPDATE

```
1. UPDATE book SET age=25 WHERE author='Masumi';
→ author が Masumi の本の年齢(age)を全部 25 に変更する

2. SELECT * FROM book;
→ bookテーブルの中身を全部抽出

```

```
 id |    name     | author | age
----+-------------+--------+-----
  3 | HTML入門    | Hiro   |
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
```

## DALETE

```
1. DELETE FROM book WHERE author='Masumi';
→ bookテーブルのauthorがMasumiのレコードを削除する

2. SELECT * FROM book;
→ bookテーブルの中身を全部抽出
```

```
 id |   name   | author | age
----+----------+--------+-----
  3 | HTML入門 | Hiro   |
```
