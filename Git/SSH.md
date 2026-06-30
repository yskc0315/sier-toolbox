# SSH

## SSHとは

SSH（Secure Shell）は、安全にリモートコンピュータへ接続するためのプロトコル。

GitHubではHTTPSの代わりにSSHで認証できる。

---

## SSH鍵とは

SSHでは

- 公開鍵（GitHubへ登録）
- 秘密鍵（PC内に保管）

のペアで認証する。

秘密鍵は絶対に公開しない。

---

## 今回実行したコマンド

### SSH接続確認

ssh -T git@github.com

意味

- ssh：SSHクライアントを起動
- -T：シェルを起動せず認証だけ行う
- git：GitHub側で用意されているSSHユーザー
- github.com：接続先

---

### 公開鍵表示

cat ~/.ssh/id_ed25519.pub

意味

cat：ファイル内容を表示するコマンド

表示された公開鍵をGitHubへ登録する。

---

## GitHubのSSHユーザーが「git」である理由

GitHubへSSH接続するときは

git@github.com

を使用する。

ここでの「git」はLinuxユーザー名であり、GitHubが用意している固定のユーザーである。

自分のGitHubアカウント名ではない。