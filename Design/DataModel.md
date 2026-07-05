# Data Model

## データモデルとは

アプリケーションで扱うデータの構造（設計図）。

FastAPIではPydanticのBaseModelを継承して定義する。

例

```python
class TaskCreate(BaseModel):
    title: str
    difficulty: int
```

---

## モデルを分ける理由

入力と出力では必要な項目が異なるため。

例

ユーザー入力

```json
{
    "title": "FastAPIを学ぶ",
    "difficulty": 3
}
```

システム内部

```text
id = 1
title = "FastAPIを学ぶ"
difficulty = 3
completed = False
exp = 300
```

---

## TaskCreate

役割

ユーザーから受け取るデータ。

持つ項目

・title

・difficulty

---

## Task

役割

システム内部で扱うデータ、およびAPIのレスポンス。

持つ項目

・id

・title

・difficulty

・completed

・exp

---

## 設計で重要な考え方

各項目について

「誰が値を決めるのか」

を考える。

ユーザーが決めるもの

・title

・difficulty

システムが決めるもの

・id

・completed

・exp

責任を分離することで、安全で保守しやすい設計になる。