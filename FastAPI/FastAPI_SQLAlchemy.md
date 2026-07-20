# FastAPI + SQLAlchemy

## 概要

FastAPIとSQLAlchemyを組み合わせることで、REST APIからデータベースを操作できる。

役割は以下のように分かれる。

```
Client
    │
HTTP
    ▼
FastAPI(app.py)
    │
Depends(get_db)
    ▼
Session
    │
SQLAlchemy(crud.py)
    │
Engine
    ▼
SQLite
```

---

# ディレクトリ構成

```
app.py         : APIエンドポイント
schemas.py     : リクエスト・レスポンス用(Pydantic)
crud.py        : DB操作
models.py      : SQLAlchemyモデル
database.py    : DB接続(Session・Engine)
```

---

# 各ファイルの役割

## app.py

責務

* URL定義
* リクエスト受付
* CRUD呼び出し
* レスポンス返却

DB操作は書かない。

例

```python
@app.get("/tasks")
def get_tasks(db: Session = Depends(get_db)):
    return crud.get_tasks(db)
```

---

## schemas.py

Pydanticモデルを定義する。

例

* TaskCreate
* TaskUpdate
* TaskResponse

役割

* リクエストのバリデーション
* レスポンスの型定義

---

## models.py

SQLAlchemyモデルを定義する。

DBテーブルとの対応を表す。

例

```python
class Task(Base):
```

これはDBテーブルそのものを表す。

---

## crud.py

DB操作のみを記述する。

例

* SELECT
* INSERT
* UPDATE
* DELETE

FastAPIを知らない。

Depends()は使用しない。

---

## database.py

DB接続設定を管理する。

保持するもの

* Engine
* SessionLocal

---

# Depends(get_db)

Session生成をFastAPIへ依頼する。

```python
db: Session = Depends(get_db)
```

FastAPIが

```
Session生成
↓
APIへ渡す
↓
close()
```

まで管理する。

---

# Session

Sessionは変更内容を管理する。

Create

```
Task生成
↓
add()
↓
commit()
↓
refresh()
```

Update

```
query()
↓
取得済みオブジェクト変更
↓
commit()
```

Delete

```
query()
↓
delete()
↓
commit()
```

---

# CRUD一覧

## Create

```python
db.add(task)
db.commit()
db.refresh(task)
```

---

## Read

```python
db.query(Task).all()
```

```python
db.query(Task).filter(Task.id == task_id).first()
```

---

## Update

```python
task.completed = True
db.commit()
```

Sessionが変更を検知するためadd()不要。

---

## Delete

```python
db.delete(task)
db.commit()
```

---

# add()が必要な理由

新規オブジェクトはSession管理外である。

```
Task()
↓
add()
↓
Session管理対象
```

---

# Updateでadd()不要な理由

query()で取得した時点でSession管理対象となる。

変更をcommit()するとUPDATE文が自動生成される。

---

# response_model

FastAPIがSQLAlchemyオブジェクトをPydanticへ変換する。

```
SQLAlchemy Task
        │
        ▼
TaskResponse
        │
        ▼
JSON
```

---

# PydanticモデルとSQLAlchemyモデルの違い

TaskCreate

* API入力

TaskResponse

* API出力

Task(SQLAlchemy)

* DBテーブル

役割が異なるため分離する。

---

# リファクタリング

役割ごとに責務を分離する。

```
app.py
↓
crud.py
↓
SQLAlchemy
```

さらに

```
schemas.py
```

を追加することで

* API
* DB
* データ形式

を独立させられる。

---

# 学んだこと

* DependsによるDI
* Session管理
* CRUD実装
* SQLAlchemyモデル
* Pydanticモデル
* response_model
* 責務分離
* リファクタリング
