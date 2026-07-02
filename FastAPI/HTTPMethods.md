# HTTP Methods

## HTTPメソッドとは

HTTP通信で「どのような操作を行うか」を表すルール。

URLだけではなく、HTTPメソッドもAPIを識別する要素となる。

例）

GET /tasks

POST /tasks

同じURLでも別のAPIとして扱われる。

---

## GET

データを取得する。

例）

GET /tasks

特徴

・サーバの状態を変更しない
・一覧取得や詳細取得に使用する

---

## POST

新しいデータを登録する。

例）

POST /tasks

特徴

・サーバの状態を変更する
・JSONなどのデータを送信する

---

## まとめ

GET：取得

POST：登録

PUT：更新

DELETE：削除