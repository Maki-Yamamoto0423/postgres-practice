## SQL文法【初級編 - データ加工】

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
id | name | author | age
----+-------------+--------+-----
1 | SQL入門 | Masumi | 25
2 | Flutter入門 | Masumi | 25
3 | HTML入門 | Hiro |
4 | GitHub入門2 | Emma | 22
5 | Flutter上級 | Oliva | 45

```

---

## DISTINCT

bookというテーブルにおいて、author列の重複を削除する

```
SELECT DISTINCT author FROM book;

```

```
 author
--------
 Oliva
 Masumi
 Hiro
 Emma

```

---

## ORDER BY

bookというテーブルにおいて、age列の基準に降順（DESC）に並び替えをする

```
SELECT * FROM book ORDER BY age DESC;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  3 | HTML入門    | Hiro   |
  5 | Flutter上級 | Oliva  |  45
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
  4 | GitHub入門2 | Emma   |  22

```

補足
PostgresSQLでは、NULLを最大値のように扱うため、「NULL → 45 → 25 → 22」の結果になった

今回取り組んでいる資料のサンプルは、MySQLはNULLを最小値のように扱うため、「45 → 25 → 22 → NULL」という結果になった

## LIMIT

bookというテーブルにおいて、age列の基準に降順（DESC）に並び替えをした後に先頭から3レコードのみを抽出する

```
SELECT * FROM book ORDER BY age DESC LIMIT 3;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  3 | HTML入門    | Hiro   |
  5 | Flutter上級 | Oliva  |  45
  1 | SQL入門     | Masumi |  25

```

補足
ここも同じくNULLの扱いの違いによりMySQLと実行結果が違う

---

## OFFSET

bookというテーブルにおいて、age列の基準に降順（DESC）に並び替えをした後に先頭から1レコード空けて、3レコード分抽出する

```
SELECT * FROM book ORDER BY age DESC LIMIT 3 OFFSET 1;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  5 | Flutter上級 | Oliva  |  45
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
```

補足
PostgreSQLではORDER BY age DESCを実行した場合、NULLは最大値として扱われ先頭に並ぶ
その状態でOFFSET 1を指定すると先頭レコード（NULL行）が除外されるため、結果には表示されなくなる

---

## CASE

bookというテーブルにおいて、ageが30よりも小さい場合は「30歳より若い」と出力し、それ以外の場合は「30歳以上」と出力するage_statusという列を作成する

```
SELECT *, CASE WHEN age<30 THEN '30歳より若い' ELSE '30歳以上' END AS age_status FROM book;
```

```
 id |    name     | author | age |  age_status
----+-------------+--------+-----+--------------
  1 | SQL入門     | Masumi |  25 | 30歳より若い
  2 | Flutter入門 | Masumi |  25 | 30歳より若い
  3 | HTML入門    | Hiro   |     | 30歳以上
  4 | GitHub入門2 | Emma   |  22 | 30歳より若い
  5 | Flutter上級 | Oliva  |  45 | 30歳以上
```

---

## LENGTH

bookというテーブルにおいて、authorの文字列の長さを出力したauthor_lenという列を作成する

```
SELECT author, LENGTH(author) AS author_len FROM book;
```

```
 author | author_len
--------+------------
 Masumi |          6
 Masumi |          6
 Hiro   |          4
 Emma   |          4
 Oliva  |          5
```

---

## REPLACE

bookというテーブルにおいて、nameの中で「入門」という文字を「初級」という文字に置き換えたname_newという列を作成する

```
SELECT name, REPLACE(name, '入門', '初級') AS name_new FROM book;
```

```
    name     |  name_new
-------------+-------------
 SQL入門     | SQL初級
 Flutter入門 | Flutter初級
 HTML入門    | HTML初級
 GitHub入門2 | GitHub初級2
 Flutter上級 | Flutter上級

```

---

## SUBSTRING

bookというテーブルにおいて、nameの文字列の1番目から3番目の文字を出力するname_extractという列を作成する

```
SELECT name, SUBSTRING(name, 1, 3) AS name_extract FROM book;
```

```
    name     | name_extract
-------------+--------------
 SQL入門     | SQL
 Flutter入門 | Flu
 HTML入門    | HTM
 GitHub入門2 | Git
 Flutter上級 | Flu

```

## ROUND

bookというテーブルにおいて、ageを四捨五入したage_roundという列を作成する

```
SELECT age, ROUND(age, -1) AS age_round FROM book;
```

```
 age | age_round
-----+-----------
  25 |        30
  25 |        30
     |
  22 |        20
  45 |        50
```

補足
桁数指定について

0の場合
意味：小数を四捨五入して整数にする
例：25 → 25

1の場合
意味：小数第1位
25 → 25.0

-1の場合
意味：10の位
例：25 → 30

-2の場合
意味：100の位
例：25 → 0

---

## POWER

bookというテーブルにおいて、ageを2乗したage_powerという列を作成する

```
SELECT age, POWER(age, 2) AS age_power FROM book;
```

```
 age | age_power
-----+-----------
  25 |       625
  25 |       625
     |
  22 |       484
  45 |      2025

```

---

## CAST

bookというテーブルにおいて、ageをINTからFLOATに変換したage_floatという列を作成する

```
SELECT age, CAST(age AS FLOAT(3)) AS age_float FROM book;
```

```
 age | age_float
-----+-----------
  25 |        25
  25 |        25
     |
  22 |        22
  45 |        45

```

補足
PostgreSQL の FLOAT は仕様が違うため、FLOAT(3)は無視される
