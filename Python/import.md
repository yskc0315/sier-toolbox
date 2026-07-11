# モジュール
- 1つの.pyファイルをモジュールと呼ぶ。
- 例：models.py、database.py

# パッケージ
- モジュールをまとめたフォルダ。
- 例：sqlalchemy、sqlalchemy.orm

# ライブラリ
- 機能をまとめたもの。
- SQLAlchemyやFastAPIなど。

# importの流れ

from sqlalchemy import create_engine

1. Pythonはsqlalchemyパッケージを探す
2. sqlalchemy/__init__.pyを読み込む
3. __init__.pyで公開(export)されているcreate_engineを取得する

# __init__.py

役割
- パッケージの入口（受付）
- 外部へ公開する機能(API)を管理する

例

from sqlalchemy import create_engine

は、sqlalchemy/__init__.pyがcreate_engineを公開しているため利用できる。

# 絶対インポート

from database import engine

プロジェクトのルートから探す。

# 相対インポート

from .database import engine

現在のパッケージから探す。

相対インポートは、パッケージとして実行されることが前提。

# なぜ.venvを書かないのか

Pythonはimport時に自動で検索パス(sys.path)を参照する。

検索先の例
- 実行中のプロジェクト
- .venv/Lib/site-packages
- Python標準ライブラリ

そのため

from sqlalchemy import create_engine

と書くだけで良い。