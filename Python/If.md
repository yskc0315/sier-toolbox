# if

## if文とは

条件によって処理を分岐する構文。

---

## 基本構文

```python
if task.id == task_id:
    print("Found")
```

条件がTrueの場合のみ処理が実行される。

---

## 比較演算子

==

値が等しいか比較する。

例

```python
task.id == task_id
```

---

## インデント

Pythonではインデントによって処理範囲を表す。

```python
if condition:
    print("実行")

print("必ず実行")
```

インデント外の処理は、条件に関係なく実行される。