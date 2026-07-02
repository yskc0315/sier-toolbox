# BaseModel

## BaseModelとは

Pydanticが提供する親クラス。

FastAPIでは、リクエスト・レスポンスのデータモデルを定義するために使用する。

---

## 例

class Task(BaseModel):
    title: str

---

## Javaとの比較

Python

class Task(BaseModel):

Java

class Task extends BaseModel

(BaseModel) はコンストラクタではなく、継承を表す。

---

## BaseModelの役割

・JSON → Pythonオブジェクトへ変換

・Pythonオブジェクト → JSONへ変換

・型チェック

・入力チェック（バリデーション）

---

## FastAPIでの流れ

JSON

↓

Task(BaseModel)

↓

Pythonオブジェクト

↓

処理

↓

Pythonオブジェクト

↓

JSON