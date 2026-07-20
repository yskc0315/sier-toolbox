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

---

# Session

## Sessionとは

- DB操作を管理するオブジェクト。
- INSERT・UPDATE・DELETE・SELECTの命令を管理する。
- 変更内容を保持し、`commit()`でDBへ反映する。

```
Python
    │
Session
    │
Engine
    │
Database
```

---

## SessionLocal

```python
SessionLocal = sessionmaker(bind=engine)
```

- Sessionを生成するためのファクトリ。
- `SessionLocal()`を呼ぶたびに新しいSessionが生成される。

```python
db = SessionLocal()
```

---

# Model

## Base

```python
Base = declarative_base()
```

- SQLAlchemyへ「このクラスをテーブルとして扱う」ことを示す基底クラス。

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

テーブルのカラムを定義する。

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

## primary_key=True

主キーを表す。

---

# create_all()

```python
Base.metadata.create_all(bind=engine)
```

### Base

テーブルとして扱うクラスを管理する。

### metadata

Baseを継承したテーブル一覧。

### create_all()

metadataを基にテーブルを作成する。

### bind=engine

作成するDBをEngineへ関連付ける。

---

# CRUD

## Create（INSERT）

```python
task = Task(...)

db.add(task)
db.commit()
db.close()
```

### Task()

Taskオブジェクト（1レコード）を生成する。

### add()

Sessionへ登録する。

この時点ではDBへ保存されない。

### commit()

Sessionの内容をDBへ反映する。

INSERT文が実行される。

### rollback()

commit前の変更を取り消す。

### close()

Sessionを終了し、DB接続を解放する。

---

## Read（SELECT）

### query()

```python
db.query(Task)
```

Taskクラスに対応するテーブルを検索対象とするQueryを作成する。

この時点ではSQLは実行されない。

---

### all()

```python
tasks = db.query(Task).all()
```

Queryを実行し、全件取得する。

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

### first()

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

---

### filter()

```python
db.query(Task).filter(Task.id == 1)
```

SQLのWHERE句に相当する。

```python
Task.id == 1
```

はTrue/Falseではなく、WHERE句を構築するための条件オブジェクトを生成する。

複数条件は

```python
.filter(
    Task.completed == False,
    Task.difficulty >= 3
)
```

のように記述でき、SQLではAND条件となる。

---

### order_by()

昇順

```python
.order_by(Task.difficulty)
```

```sql
ORDER BY difficulty ASC
```

降順

```python
.order_by(Task.difficulty.desc())
```

```sql
ORDER BY difficulty DESC
```

---

### all()とfirst()の違い

| メソッド | 戻り値 | データなし |
|----------|--------|-----------|
| all() | list[Task] | [] |
| first() | Task | None |

---

### Noneの扱い

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

を実行すると

```
AttributeError
```

が発生する。

---

## Update（UPDATE）

```python
task = db.query(Task).filter(Task.id == 1).first()

task.completed = True
task.exp = 300

db.commit()
```

### ポイント

- `update()`メソッドは使用しない。
- Sessionが変更を検知（Change Tracking）する。
- `commit()`時にUPDATE文が自動生成される。
- 複数項目を変更しても、通常は1回のUPDATE文で反映される。

---

## Delete（DELETE）

```python
task = db.query(Task).filter(Task.id == 1).first()

db.delete(task)

db.commit()
```

### ポイント

- `delete()`ではSessionへ削除予定として登録するだけ。
- `commit()`でDELETE文が実行される。
- `commit()`しなければDBから削除されない。

---

# SQLAlchemyの全体像

```
Taskオブジェクト
        │
        ▼
Session
（変更内容を管理）
        │
commit()
        │
        ▼
Engine
        │
        ▼
Database
```

SessionはDBへ直接命令するのではなく、**オブジェクトの状態（追加・更新・削除）を管理し、`commit()`時にEngine経由でDBへ反映する。**

---

# SQLとSQLAlchemyの対応

| SQLAlchemy | SQL |
|------------|-----|
| `query(Task)` | `SELECT * FROM tasks` |
| `filter(Task.id == 1)` | `WHERE id = 1` |
| `order_by(Task.id)` | `ORDER BY id ASC` |
| `order_by(Task.id.desc())` | `ORDER BY id DESC` |
| `add()` | `INSERT` |
| 値を書き換える | `UPDATE` |
| `delete()` | `DELETE` |
| `commit()` | トランザクション確定・DBへ反映 |