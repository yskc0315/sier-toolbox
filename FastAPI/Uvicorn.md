# Uvicorn

## Uvicornとは

FastAPIを動かすASGIサーバー。

---

## 起動

uvicorn app:app --reload

### 左側のapp

Pythonモジュール名（app.py）

### 右側のapp

FastAPIオブジェクト

内部イメージ

import app

server = app.app

---

## --reload

ソースコード変更時に自動でサーバーを再起動する。

開発中のみ利用する。

---

## docs

FastAPIでは

http://127.0.0.1:8000/docs

へアクセスするとSwagger UIが自動生成される。