# SQLAlchemy

## SQLAlchemyとは

Pythonからリレーショナルデータベース（RDBMS）を操作するためのライブラリ。

Pythonのオブジェクトとデータベースのテーブルを対応付けるORM（Object Relational Mapping）を提供する。

対応しているデータベース例

- SQLite
- PostgreSQL
- MySQL
- SQL Server

---

## ORMとは

ORM（Object Relational Mapping）とは、

Pythonのクラスとデータベースのテーブルを対応付ける仕組みである。

例

Python

```python
class Task:
    ...
```

↓

データベース

```
tasks テーブル
```

SQLAlchemyがこの対応付けを行う。

---

## Engine

データベースへ接続するための窓口。

```python
engine = create_engine(DATABASE_URL)
```

役割

- データベースへの接続情報を保持する
- SQLiteやPostgreSQLなどへ接続する
- Sessionから利用される

---

## Session

データベース操作（CRUD）を担当するオブジェクト。

```python
SessionLocal = sessionmaker(bind=engine)
```

Sessionでは主に以下を行う。

- データ追加
- データ取得
- データ更新
- データ削除

例

```python
session.add(task)
session.commit()
session.query(Task)
session.delete(task)
```

---

## DATABASE_URL

接続先のデータベースを指定する文字列。

SQLiteの場合

```python
DATABASE_URL = "sqlite:///task_raid.db"
```

SQLite以外も指定可能。

例

- SQLite
- PostgreSQL
- MySQL

---

## create_engine()

Engineを生成する関数。

```python
engine = create_engine(DATABASE_URL)
```

※ この時点ではデータベースファイルは作成されない。

実際にデータベースへアクセスしたタイミングで生成される。

---

## sessionmaker()

Sessionを生成するためのファクトリ。

```python
SessionLocal = sessionmaker(bind=engine)
```

利用時は

```python
session = SessionLocal()
```

としてSessionを生成する。

---

## 現在の構成

```
FastAPI
    │
    ▼
Python
    │
    ▼
Session
    │
    ▼
Engine
    │
    ▼
SQLite
    │
    ▼
task_raid.db
```

---

## 今後学ぶこと

- declarative_base()
- Base
- Model
- Column
- Integer
- String
- Boolean
- SQLAlchemy ORM