## SQL文法【上級編 - トランザクション処理】

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

## BEGIN / COMMIT

トランザクション成功ver.
Emmaの年齢を10歳増やし、Olivaの年齢を10歳減らす

```
BEGIN;

UPDATE book SET age = age + 10 WHERE author = 'Emma';
UPDATE book SET age = age - 10 WHERE author = 'Oliva';

COMMIT;

SELECT * FROM book;
```

```
BEGIN
UPDATE 1
UPDATE 1
COMMIT
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
  3 | HTML入門    | Hiro   |
  4 | GitHub入門2 | Emma   |  32
  5 | Flutter上級 | Oliva  |  35

```

補足  

出力結果の説明

BEGIN
→ トランザクション開始成功（変更はDBにはまだ保存されていない）

UPDATE 1
→ Emmaの行を1件更新した（Emma: 22 → 32）

UPDATE 1
→ Olivaの行を1件更新した（Oliva: 45 → 35）

COMMIT
→ トランザクション確定（DBに変更が保存される）
ここでエラーだとトランザクションは失敗になるため、全部取り消しになる

トランザクション失敗ver.

Emmaの年齢を10歳増やす
Olivaの年齢を10歳減らす

```
BEGIN;

UPDATE book SET age = age + 10 WHERE author = 'Emma';

-- 存在しないカラムを更してわざとエラーを起こす--
UPDATE book SET ages = age - 10 WHERE author = 'Oliva';

COMMIT;
```

```
COMMIT;
BEGIN
UPDATE 1
ERROR:  column "ages" of relation "book" does not exist
LINE 1: UPDATE book SET ages = age - 10 WHERE author = 'Oliva';
                        ^
ROLLBACK
```

補足  
ここでbookテーブルを確認したら、失敗例では更新が取り消されることが確認できた

---

## ROLLBACK

```
BEGIN;

UPDATE book SET age = age + 10 WHERE author = 'Emma';
UPDATE book SET age = age - 10 WHERE author = 'Oliva';

ROLLBACK;

SELECT * FROM book;
```

```
BEGIN
UPDATE 1
UPDATE 1
ROLLBACK
 id |    name     | author | age
----+-------------+--------+-----
  1 | SQL入門     | Masumi |  25
  2 | Flutter入門 | Masumi |  25
  3 | HTML入門    | Hiro   |
  4 | GitHub入門2 | Emma   |  22
  5 | Flutter上級 | Oliva  |  45

```

補足  
PostgreSQLは「未確定トランザクションは安全のため自動で破棄」してくれる  
安全確認・可読性のため、実務では ROLLBACK を明示するのが推奨されている

---

## SAVEPOINT

```
BEGIN;

-- Masumi の年齢を26に更新 --
UPDATE book SET age = 26 WHERE author = 'Masumi';

-- セーブポイント作成 --
SAVEPOINT temp_savepoint;

-- Emma の年齢を +10 --
UPDATE book SET age = age + 10 WHERE author = 'Emma';

-- Oliva の年齢を -10 --
UPDATE book SET age = age - 10 WHERE author = 'Oliva';

-- セーブポイントまでロールバック（EmmaとOlivaの更新を取り消す） --
ROLLBACK TO SAVEPOINT temp_savepoint;

-- Hiro の年齢を28に更新 --
UPDATE book SET age = 28 WHERE author = 'Hiro';

-- トランザクション確定 --
COMMIT;

-- 確認 --
SELECT * FROM book;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  4 | GitHub入門2 | Emma   |  22
  5 | Flutter上級 | Oliva  |  45
  1 | SQL入門     | Masumi |  26
  2 | Flutter入門 | Masumi |  26
  3 | HTML入門    | Hiro   |  28
```

---

## TRUNCATE

```
TRUNCATE TABLE book;
INSERT INTO book(name, author, age) VALUES ('Flutter上級', 'Oliva', 45);
SELECT * FROM book;
```

```
 id |    name     | author | age
----+-------------+--------+-----
  6 | Flutter上級 | Oliva  |  45
```

補足  
TRUNCATE は「テーブル全体を一気に初期化したいとき」に使う  
トランザクションや外部キー、IDシーケンスに注意する

トランザクション
→ トランザクション内で使っても途中で ROLLBACK できない

外部キー
→ 他のテーブルから外部キーで参照されている場合、直接 TRUNCATE はできないため、
参照整合性を保つために CASCADE を付ける必要がある。

注意点：CASCADE を付けると、参照しているテーブルのデータもまとめて削除される

IDシーケンス
→ SERIAL / AUTO_INCREMENT の列は、TRUNCATE すると IDがリセットされる

---
