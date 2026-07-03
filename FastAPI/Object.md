# Object

## オブジェクトとは

データと機能（メソッド）を持つもの。

Pythonではほぼすべてがオブジェクトである。

---

## 属性（Attribute）

オブジェクトが保持しているデータ。

例

```python
task.title
task.difficulty
task.completed
```

Taskオブジェクト

```
Task
├── id
├── title
├── difficulty
├── completed
└── exp
```

---

## メソッド（Method）

オブジェクトが持つ機能。

例

```python
tasks.append(new_task)
```

append() はリストが持つメソッド。

---

## AttributeとMethodの違い

属性

値を保持する。

例

```python
task.title
```

メソッド

処理を実行する。

例

```python
tasks.append(task)
```

---

## Javaとの比較

Python

```python
task.title
```

Java

```java
task.getTitle();
```

Pythonではgetterを書かずに属性へ直接アクセスすることが多い。

---

## 今日学んだこと

TaskCreate

↓

Task

↓

tasks（リスト）

↓

append()

Taskはデータを持つ。

tasksはTaskオブジェクトを保持するリスト。

append()はリストのメソッドである。