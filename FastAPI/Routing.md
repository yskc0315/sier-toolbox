# Routing

## ルーティングとは

リクエストされたURLに応じて、実行する処理を振り分けること。

FastAPIではデコレータを使用してルーティングを定義する。

---

## 例

```python
@app.get("/")
def hello():
    return {"message": "Hello"}
```

GET / が呼び出された場合に、hello() が実行される。

---

```python
@app.get("/tasks")
def get_tasks():
    return []
```

GET /tasks が呼び出された場合に、get_tasks() が実行される。

---

## APIとは

APIとは、プログラムが外部へ公開している機能（窓口）のこと。

FastAPIでは、

GET /tasks

のようなURLとHTTPメソッドの組み合わせがAPIとなる。

Pythonの関数はAPIではなく、APIから呼び出される処理である。

---

## デコレータの役割

```python
@app.get("/tasks")
```

は

「GET /tasks が来たら、この関数を実行する」

という対応付け（ルーティング）を行っている。