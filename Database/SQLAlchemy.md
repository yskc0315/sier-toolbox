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

# Session

## Sessionとは

Sessionは、Pythonオブジェクトの変更を管理するためのオブジェクトである。

役割
- Pythonオブジェクトの追加・更新・削除を管理する
- commit()時にDBへ変更を反映する
- rollback()時に未確定の変更を取り消す

SessionはDBと直接通信するのではなく、Engineを利用してDBへ処理を依頼する。

---

## Engineとの違い

| 項目 | 役割 |
|------|------|
| Engine | DBとの接続・SQL実行を担当する |
| Session | Pythonオブジェクトの変更を管理する |

---

# SessionLocal

```python
SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine
)
```

SessionLocalはSessionではない。

Sessionを生成するためのFactory（工場）である。

Sessionを生成するには以下のように呼び出す。

```python
db = SessionLocal()
```

dbにはSessionオブジェクトが格納される。

---

# add()

```python
db.add(task)
```

add()を実行しただけではDBへ保存されない。

Sessionが「保存予定」として管理するだけである。

---

# commit()

```python
db.commit()
```

現在のトランザクションを確定し、DBへ反映する。

commit()が成功すると、その変更はDBへ永続化される。

commit後は、その変更をrollback()で取り消すことはできない。

---

# rollback()

```python
db.rollback()
```

現在のトランザクションで未確定の変更を取り消す。

例

```python
db.add(task)
db.rollback()
```

この場合、taskはDBへ保存されない。

---

# Transaction

Transaction（トランザクション）は、一連のDB処理をまとめた単位である。

- commit()：変更を確定する
- rollback()：変更を取り消す

Sessionは複数のTransactionを扱える。

例

```python
db.add(task1)
db.commit()

db.add(task2)
db.commit()
```

1回目のcommit()でtask1はDBへ保存される。

2回目のcommit()でエラーが発生しても、task1は既に確定しているため削除されない。

---

# sys.path

```python
import sys

print(sys.path)
```

sys.pathは、Pythonがimport先を検索するパスの一覧である。

import時は、上から順番に検索される。

例えば

```python
from database import engine
```

の場合、

まずプロジェクトフォルダを検索し、database.pyを見つける。

```python
from sqlalchemy import create_engine
```

の場合、

site-packages内のsqlalchemyパッケージを検索する。

仮想環境が有効な場合は、

```
.venv/Lib/site-packages
```

が検索対象となる。