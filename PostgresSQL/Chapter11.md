## SQL文法【中級編 - 複数テーブルの処理】

book_1、book_2テーブルを同時に作成

```
CREATE TABLE book_1(
    id SERIAL PRIMARY KEY,
    name VARCHAR(30) NOT NULL,
    author VARCHAR(30) NOT NULL,
    age INTEGER
);

CREATE TABLE book_2 (LIKE book_1 INCLUDING ALL);

INSERT INTO book_1(name, author, age) VALUES ('SQL入門', 'Masumi', 25);
INSERT INTO book_1(name, author, age) VALUES ('Flutter入門', 'Masumi', 25);
INSERT INTO book_1(name, author) VALUES ('HTML入門', 'Hiro');

INSERT INTO book_2(name, author, age) VALUES ('SQL入門', 'Masumi', 25);
INSERT INTO book_2(name, author, age) VALUES ('GitHub入門2', 'Emma', 22);
INSERT INTO book_2(name, author, age) VALUES ('Flutter上級', 'Oliva', 45);

SELECT * FROM book_1;
SELECT * FROM book_2;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
  3 | HTML入門    | Hiro   |

 id |    name     | author | age
----+-------------+--------+-----
  4 | SQL入門     | Masumi |  25
  5 | GitHub入門2 | Emma   |  22
  6 | Flutter上級 | Oliva  |  45
```

---

## UNION

2つのテーブルの行をまとめて、重複する行は1つにして取得する

```
SELECT * FROM book_1 UNION SELECT * FROM book_2;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  2 | Flutter入門 | Masumi |  25
  3 | HTML入門    | Hiro   |
  4 | SQL入門     | Masumi |  25
  5 | GitHub入門2 | Emma   |  22
  6 | Flutter上級 | Oliva  |  45
  1 | SQL入門     | Masumi |  25

```

補足
PostgreSQLの UNION は内部的に重複除去処理を行ってくれる
その方法はHash方式とSort方式の２種類がある
どちらの方式も出力順の保証はしてくれない

SQL入門のデータが重複しているはずなのに消えないのはなぜか？
→ IDが違うことによって別行として扱われているので、重複していないとみなされていた

---

## INTERSECT

2つのテーブルに共通して存在する「完全一致の行だけ」を取得する

```
SELECT * FROM book_1 INTERSECT SELECT * FROM book_2;
```

```
 id | name | author | age
----+------+--------+-----
(0 rows)
```

補足
book_1テーブルとbook_2テーブルに全列一致する行が存在しないため、結果が0件になった

---

## EXCEPT

book_1 にあって book_2 に無い行だけ返す

```
SELECT * FROM book_1 EXCEPT SELECT * FROM book_2;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  2 | Flutter入門 | Masumi |  25
  1 | SQL入門     | Masumi |  25
  3 | HTML入門    | Hiro   |
```

補足
book_1テーブルの中身がそのまま残る結果になった
並び順がおかしいのは、ORDER BY を書いてないから

---

## CROSS JOIN

2つのテーブルの全行の組み合わせ（直積）を取得する処理

```
SELECT * FROM book_1 CROSS JOIN book_2;
```

```
 id |    name     | author | age | id |    name     | author | age
----+-------------+--------+-----+----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25 |  4 | SQL入門     | Masumi |  25
  1 | SQL入門     | Masumi |  25 |  5 | GitHub入門2 | Emma   |  22
  1 | SQL入門     | Masumi |  25 |  6 | Flutter上級 | Oliva  |  45
  2 | Flutter入門 | Masumi |  25 |  4 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25 |  5 | GitHub入門2 | Emma   |  22
  2 | Flutter入門 | Masumi |  25 |  6 | Flutter上級 | Oliva  |  45
  3 | HTML入門    | Hiro   |     |  4 | SQL入門     | Masumi |  25
  3 | HTML入門    | Hiro   |     |  5 | GitHub入門2 | Emma   |  22
  3 | HTML入門    | Hiro   |     |  6 | Flutter上級 | Oliva  |  45
```

補足
イメージ
(book_1の各行) × (book_2の全行)

テストデータを増やしたいときや、全パターンを作成したいときに使う

---

## INNER JOIN

名前（name）が同じ行だけを、2つのテーブルから結合して取得する

```
SELECT * FROM book_1 INNER JOIN book_2 ON book_1.name = book_2.name;
```

```
 id |  name   | author | age | id |  name   | author | age
----+---------+--------+-----+----+---------+--------+-----
  1 | SQL入門 | Masumi |  25 |  4 | SQL入門 | Masumi |  25
```

補足

name が一致した行が1組しかなかったため、その1組だけが結合されて出力された

CROSS JOIN：全組み合わせ
INNER JOIN：一致した行だけ

---

## LEFT JOIN

同じ name の行があれば結合し、無ければ左テーブルだけ表示する

```
SELECT * FROM book_1 LEFT JOIN book_2 ON book_1.name = book_2.name;
```

```
 id |    name     | author | age | id |  name   | author | age
----+-------------+--------+-----+----+---------+--------+-----
  1 | SQL入門     | Masumi |  25 |  4 | SQL入門 | Masumi |  25
  2 | Flutter入門 | Masumi |  25 |    |         |        |
  3 | HTML入門    | Hiro   |     |    |         |        |
```

補足
SQL入門
→ book_1、book_2どちらにも存在するので結合

Flutter入門
→ book_2にはないので結果NULL

HTML入門
→ book_2にはないので結果NULL

---

## RIGHT JOIN

同じ name の行があれば結合し、無ければ右テーブルだけ表示する

```
SELECT * FROM book_1 RIGHT JOIN book_2 ON book_1.name = book_2.name;
```

```
 id |  name   | author | age | id |    name     | author | age
----+---------+--------+-----+----+-------------+--------+-----
  1 | SQL入門 | Masumi |  25 |  4 | SQL入門     | Masumi |  25
    |         |        |     |  5 | GitHub入門2 | Emma   |  22
    |         |        |     |  6 | Flutter上級 | Oliva  |  45
```

補足
LEFT JOIN と同じ仕組みを、基準テーブルを右にして実行している

---

## FULL JOIN

両方のテーブルを全部出して、名前が一致する行だけ結合する

```
SELECT * FROM book_1 FULL JOIN book_2 ON book_1.name = book_2.name;
```

```
 id |    name     | author | age | id |    name     | author | age
----+-------------+--------+-----+----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25 |  4 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25 |    |             |        |
  3 | HTML入門    | Hiro   |     |    |             |        |
    |             |        |     |  6 | Flutter上級 | Oliva  |  45
    |             |        |     |  5 | GitHub入門2 | Emma   |  22
```

補足
LEFT JOIN と RIGHT JOIN を合体したイメージ

FULL JOIN の動作
① 一致する行 → 結合
② 左だけある行 → 右NULL
③ 右だけある行 → 左NULL

---
