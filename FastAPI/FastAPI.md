# FastAPI

## FastAPIとは

Python製Web APIフレームワーク。

---

## 最小構成

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def hello():
    return {"message": "Hello Task Raid!"}
```

## 各行の意味

### from fastapi import FastAPI

FastAPIクラスを読み込む。

---

### app = FastAPI()

FastAPIオブジェクトを生成する。

Task RaidというWebアプリそのものを表す。

---

### @app.get("/")

URL「/」へGETリクエストが来たら、

次の関数を実行する。

---

### def hello()

helloという関数を定義する。

---

### return

レスポンスとしてJSONを返す。