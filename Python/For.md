# for

## for文とは

リストやタプルなどのコレクションから要素を1つずつ取り出して処理を行う構文。

---

## 基本構文

```python
for task in tasks:
    print(task)
```

taskには、リスト内の要素が順番に代入される。

---

## イメージ

tasks

```
[
    Task(id=1),
    Task(id=2),
    Task(id=3)
]
```

処理

```
1回目
task = Task(id=1)

2回目
task = Task(id=2)

3回目
task = Task(id=3)
```

---

## 用途

・一覧表示

・検索

・集計

・更新

など、リスト内のすべての要素を処理する際に使用する。