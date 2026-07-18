# SQLAlchemy

## SQLAlchemyとは

- PythonからRDBMSを操作するためのORM（Object Relational Mapping）ライブラリ。
- SQLを書かなくても、Pythonオブジェクトを操作する感覚でDBを操作できる。

```
Pythonオブジェクト
        │
        ▼
SQLAlchemy（ORM）
        │
        ▼
SQL
        │
        ▼
RDBMS
```

---

# Engine

## Engineとは

- SQLAlchemyとDBを接続するための窓口。
- DBとの接続情報を保持する。
- Sessionから命令を受け取り、DBへSQLを送信する。

```python
engine = create_engine(DATABASE_URL)
```

```
Session
    │
    ▼
Engine
    │
    ▼
SQLite
```

---

# Session

## Sessionとは

- DB操作を管理するオブジェクト。
- INSERT・UPDATE・DELETE・SELECTなどの命令を管理する。

```python
db = SessionLocal()
```

SessionはEngineへ命令を渡す。

```
Python
    │
Session
    │
Engine
    │
SQLite
```

---

## SessionLocal()

```python
SessionLocal = sessionmaker(bind=engine)
```

- Sessionを生成するためのクラス（工場）。
- `SessionLocal()`を呼ぶたびに新しいSessionが生成される。

```python
db = SessionLocal()
```

FastAPIでも、基本的には1リクエストごとに新しいSessionを生成する。

---

# Model

## Base

```python
Base = declarative_base()
```

- SQLAlchemyへ「このクラスをテーブルとして扱う」ことを示すための基底クラス。

---

## Task

```python
class Task(Base):
```

- Baseを継承したクラスはテーブルとして扱われる。

---

## __tablename__

```python
__tablename__ = "tasks"
```

- DB上のテーブル名。

---

## Column

```python
id = Column(Integer, primary_key=True)
```

Columnはテーブルの列（カラム）を表す。

---

### Integer

整数型

### String

文字列型

### Boolean

真偽値型

SQLiteでは

```
False = 0
True  = 1
```

として保存される。

---

### primary_key=True

主キーを表す。

---

# create_all()

```python
Base.metadata.create_all(bind=engine)
```

### Base

テーブルとして扱うクラスを管理している。

### metadata

Baseを継承したテーブル一覧。

### create_all()

metadataを基にテーブルを作成する。

### bind=engine

作成するDBをEngineへ関連付ける。

---

# Create（INSERT）

## INSERTの流れ

```python
db = SessionLocal()

task = Task(
    title="勉強",
    difficulty=3,
    completed=False,
    exp=100
)

db.add(task)

db.commit()

db.close()
```

---

## Task()

Taskオブジェクト（＝1レコード）を生成する。

まだDBへ保存されない。

---

## add()

```python
db.add(task)
```

Sessionへ登録する。

この時点ではDBアクセスは行われない。

---

## commit()

```python
db.commit()
```

Sessionの内容をDBへ反映する。

トランザクションを確定する。

---

## rollback()

```python
db.rollback()
```

commit前であれば変更を取り消せる。

commit後は取り消せない。

---

## close()

```python
db.close()
```

Sessionを終了する。

DB接続を解放する。

---

# Read（SELECT）

## query()

```python
db.query(Task)
```

Taskクラスに対応するテーブルを検索対象とするQueryを作成する。

この時点ではSQLは実行されない。

---

## all()

```python
tasks = db.query(Task).all()
```

Queryを実行する。

戻り値

```python
list[Task]
```

データが存在しない場合

```python
[]
```

が返る。

---

## first()

```python
task = db.query(Task).first()
```

最初の1件を取得する。

戻り値

```python
Task
```

または

```python
None
```

ORDER BYを書かない限り、どのレコードが返るかは保証されない。

---

## filter()

```python
db.query(Task).filter(Task.id == 1)
```

WHERE句を追加する。

```python
Task.id == 1
```

はTrue/Falseではなく、

WHERE句を生成するための条件オブジェクトである。

---

## ORMの考え方

Taskは

- PythonではTaskクラス
- SQLAlchemyではtasksテーブル

を表す。

そのため

```python
db.query(Task)
```

は

「tasksテーブルを検索する」

という意味になる。

---

## all()とfirst()の違い

| メソッド | 戻り値 | データなし |
|----------|--------|-----------|
| all() | list[Task] | [] |
| first() | Task | None |

---

## Noneの扱い

```python
task = db.query(Task).filter(Task.id == 999).first()
```

存在しない場合

```python
task is None
```

となる。

そのまま

```python
task.title
```

とすると

```
AttributeError
```

となる。

そのため

```python
if task is None:
    ...
```

で判定する。

---

# SQLAlchemyの全体像

```
Python

Taskオブジェクト
        │
        ▼
Session
        │
        ▼
Engine
        │
        ▼
SQLite（PostgreSQL）
```

FastAPIを導入すると

```
Browser
        │
        ▼
FastAPI
        │
        ▼
Session
        │
        ▼
Engine
        │
        ▼
SQLite（PostgreSQL）
```