# 仮想環境

## 仮想環境とは

Pythonライブラリをプロジェクトごとに隔離する仕組み。

---

## 作成

python -m venv .venv

意味

python

Pythonを起動

-m

モジュールを実行

venv

仮想環境作成モジュール

.venv

作成するディレクトリ名

---

## .venvとは

Python本体ではなく、

プロジェクト専用のライブラリ管理領域。

---

## 有効化

source .venv/Scripts/activate

PATHを書き換え、

仮想環境内のPythonを利用する。

---

## activateとは

source .venv/Scripts/activate

現在のシェルで仮想環境を有効化する。

実際には

- PATHを書き換える
- VIRTUAL_ENVを設定する
- PS1を書き換える

という処理を行っている。

その結果

(.venv)

がプロンプトへ表示される。

## deactivate

deactivate

仮想環境を終了する。