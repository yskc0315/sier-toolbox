# .gitignore

## .gitignoreとは

Gitで管理しないファイル・ディレクトリを指定するためのファイル。

## なぜ必要か

Pythonプロジェクトでは

- .venv
- __pycache__
- *.pyc

など、自動生成されるファイルは管理しない。

理由

- 容量が大きい
- OSによって内容が異なる
- いつでも再生成できる

## 例

```gitignore
.venv/
__pycache__/
*.pyc