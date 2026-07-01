# Commit

## commitとは

コミットとは、現在の変更内容をローカルリポジトリへ保存すること。

作業履歴を残す役割を持つ。

---

## コミットの流れ

作業

↓

git add

↓

git commit

↓

git push

---

## コミット

git commit -m "コミットメッセージ"

### オプション

-m

コミットメッセージをコマンドラインから指定する。

---

## コミットメッセージ

コミットメッセージは、そのコミットで何を行ったのかを簡潔に表す。

例

Initialize Task Raid project

Add player API

Fix login bug

---

## コミットの修正

git commit --amend

直前のコミットを修正する。

コミットメッセージも変更できる。

### コミットメッセージを変更しない場合

git commit --amend --no-edit

直前のコミットへ変更を追加する。

メッセージは変更しない。

---

## ポイント

- コミットは「動く単位」で行う。
- コミットメッセージは、変更内容が分かるように書く。
- GitHubへPushする前であれば、`git commit --amend` による修正が可能。

---

### 誤ってコミットした場合は、

1. `.gitignore` を作成する。
2. `git rm -r --cached .venv` を実行する。
3. `git commit --amend --no-edit` で直前のコミットを修正する。

Push前であれば、履歴をきれいに修正できる。

---

## git commit --amend と git rm --cached の違い

### git rm --cached

Gitの管理対象からファイルを外す。

作業フォルダのファイルは削除されない。

ステージングエリアの内容を変更するコマンド。

---

### git commit --amend

直前のコミットを作り直す。

コミットメッセージの修正や、ステージングエリアの内容を反映したコミットの更新に利用する。

---

### 違い

git rm --cached

→ コミットする内容（ステージングエリア）を変更する。

git commit --amend

→ コミットそのものを作り直す。