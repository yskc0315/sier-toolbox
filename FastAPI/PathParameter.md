# Path Parameter

## パスパラメータとは

URLの一部を変数として受け取る仕組み。

例

```python
@app.put("/tasks/{task_id}")
def update_task(task_id: int):
```

---

## URL例

```
PUT /tasks/3
```

↓

```
task_id = 3
```

となる。

---

## 用途

・ID指定

・ユーザー指定

・商品指定

など、特定のリソースを指定する場合に利用する。

---

## 例

```python
@app.get("/users/{user_id}")
```

```
GET /users/10
```

↓

```
user_id = 10
```