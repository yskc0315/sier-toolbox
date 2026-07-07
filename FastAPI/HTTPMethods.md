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

例

```python
@app.get("/tasks")
```

---

## POST

新しいデータを作成する。

例

```python
@app.post("/tasks")
```

---

## PUT

既存のデータを更新する。

例

```python
@app.put("/tasks/{task_id}")
```

---

## DELETE

既存のデータを削除する。

例

```python
@app.delete("/tasks/{task_id}")
```

---

## CRUDとの対応

| 操作 | HTTP |
|------|------|
| Create | POST |
| Read | GET |
| Update | PUT |
| Delete | DELETE |