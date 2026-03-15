
## 「いちばんやさしいGit&GitHubの教本」を読みながらキーワードやコマンド結果をメモしたもの

リポジトリ：コミットをためていく場所  
クローン：リポジトリをコピーすること  
ローカルリポジトリ：手元のPC内に作成する自分専用のリポジトリ  
リモートリポジトリ：ネットワーク上に存在するリポジトリ 共同で使う目的  

ワークツリー：ワーキングツリーや作業ディレクトリとも呼ばれる。変更するファイルを保持する場所  
ステージングエリア：コミットするファイルを登録する場所  
Gitディレクトリ：コミットを格納する場所  

改行コード設定：  
リモートリポジトリにあるファイルが CRLF か LF かで、またプロジェクトで合意を取って決める感じかな。  
core.autocrlf trueにすると、コミットするときにCRLFをLFに自動変換し、チェックアウト時にLFをCRLFに自動変換する。  

Git Bashにてコマンドラインの先頭に表示される `$` は「プロンプト」と呼び、これは自分で入力する必要は無い。  
ルートディレクトリ：'/' C:\の1つ上にある  
ホームディレクトリ：'~'  
カレントディレクトリ：'.'  
pwd：カレントディレクトリの絶対パスを出力する  
`$ ls Documents` とすると、そのディレクトリ内を表示する。  

バージョン表示
```bash
$ git --version
git version 2.36.0.windows.1
```
`$ git config --list` にて、config部分をサブコマンド、--list部分をオプションと呼ぶ。  

Gitの設定値一覧、および特定の設定値を確認  
`$ git config --list`  
`$ git config user.name`

設定値がどのスコープに由来するかを確認  
`$ git config --list --show-origin`  

設定値の削除  
`$ git config --local --unset user.name`

user.nameとuser.emailはコミットに記録される。

コミットメッセージ入力時など、Git操作にて利用するエディタをVSCodeに指定する。  
スコープはlocalではなくglobalで良いと思う。  
```bash
$ git config core.editor
code --wait
```

デフォルトブランチを変更  
`$ git config --global init.defaultBranch main`

### Chapter3
git add: ステージングエリアに登録  
git commit: コミット  
git status / diff / log: 状態を確認  

git add . : カレントディレクトリ配下の全てを対象  
git add (ディレクトリ名): ディレクトリ配下のすべてを対象  
ステージングエリアに登録されていれば、git statusにて`Changes to be committed:`の欄に表示される。  

git diff: ワークツリーとステージングエリアの比較  
git diff --cached: ステージングエリアとGitディレクトリの比較
git diff <ブランチ名>: 現在のブランチとの比較

(vscodeでの)コミットメッセージ入力にて、#から始まる行はコメント行となり、コミットログには反映されない。  
git commit -m "(1行コミットメッセージ)": vscodeに遷移せずコミット  
vscodeに遷移せず、2行以上のコミットメッセージを入力したい場合：  
```bash
$ git commit -m "update
> 
> 3行目入力
> 4行目入力"
```

git restore: 変更の取り消し  
git restore (ファイル名): ワークツリーの変更の取り消し(=git addする前の変更)  
git restore --staged (ファイル名): ステージングエリアへの登録を取り消し(=git addのキャンセル)  

git rm: Git管理下のファイルの削除。「削除した状態」を登録するので、この後コミットする  

git log: コミット履歴の確認  
git log -p: 差分付き  

### Chapter4
GitHubで使用できる認証方式は以下の2つ。

| 通信プロトコル | 認証方法 |
|-----|-----|
| SSH | 公開鍵 |
| HTTPS | ユーザー名とアクセストークン |

公開鍵の場合、自分のPCにて秘密鍵と公開鍵のペアを作り、公開鍵をGitHubに登録する。  

git remote -v: リモートリポジトリの設定確認 git@github.com で始まっていればSSH接続、https:// ならHTTPS接続
```bash
(SSH接続の例)
$ git remote -v
origin  git@github.com:hoshinon57/ichiyasaGitSample.git (fetch)
origin  git@github.com:hoshinon57/ichiyasaGitSample.git (push)

(HTTPS接続の例)
$ git remote -v
origin  https://github.com/hoshinon57/study-Git.git (fetch)
origin  https://github.com/hoshinon57/study-Git.git (push)
```

origin: クローン元のリモートリポジトリを表す名前  
Gitが初期値として"origin"を設定し、これは変えることもできるが、普通はoriginのまま。  
Gitでは1つのローカルリポジトリに対してリモートリポジトリを複数設定できるらしい。

start (フォルダ名) でエクスプローラーを起動できる。  
`start .` で作業中のカレントディレクトリを開けるのは便利かも。

***
※以下しばらくWindows10の話。  
HTTPS接続のとき、ユーザー名とパスワードをgitコマンド操作にて聞かれたことが無いな…？という疑問が発生した。  
ChatGPTによると以下とのこと。  
> Git for Windows では、デフォルトで  
> Git Credential Manager (GCM)  
> が有効になっており、初回認証時の情報を Windowsの資格情報ストアに保存します。  

Windowsのスタートメニューから「資格情報マネージャー」を開き、  
`git:https://github.com`  
がそれ。  
これを削除した上で**git push**すると、認証を求められる。空pushでもOK.  
※GitHubのPublicリポジトリでは、pullやfetchなど読み取りコマンドは認証不要のようで、上記で認証は求められない。pushは書き込み操作なので認証必須。  

SSH接続のとき、本の手順の通りに生成すると、git操作ごとにパスフレーズ入力を求められて面倒。  
セキュリティは低下するが、手っ取り早いのは「パスフレーズ無しで鍵生成」かなと思う。  
```bash
$ ssh-keygen.exe -t ed25519 -C (メールアドレス)
Generating public/private ed25519 key pair.
Enter file in which to save the key (/c/Users/****/.ssh/id_ed25519): /c/Users/****/.ssh/github/id_ed25519
Enter passphrase (empty for no passphrase): ★ここで空入力
Enter same passphrase again:
```
サービスごとに鍵を分けたかったため、上記では生成フォルダをデフォルトから変えている(githubフォルダ)。  
これだとSSH接続に失敗する。`~/.ssh/id_ed25519`, `~/.ssh/id_rsa`などしかデフォルトでは見に行かないため。  
これを解決するには　`~/.ssh/config` を作成する。  
SSHの接続設定を書くファイルで、「github.comに接続するときはこの鍵を使う」といった設定ができる。  
```bash
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github/id_ed25519
  IdentitiesOnly yes
```
これでSSH接続が成功する。
```bash
$ ssh -T git@github.com
Hi hoshinon57! You've successfully authenticated, but GitHub does not provide shell access.
```

前述の通りセキュリティ面ではよろしくない(鍵ファイル盗まれたら終わり)ので、Windowsにある常駐型のssh-agentを使う手段がある。  
ChatGPT回答をほぼ転記したのみで、未実施。  
```bash
(PowerShell(管理者)にて)
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
⇒ssh-agentがPC起動時に自動起動

(Git Bashにて)
$ ssh-add ~/.ssh/id_ed25519
⇒鍵登録 (パスフレーズ入力求められる)
```
***

### Chapter5
git branch <ブランチ名>: ブランチを作成  
git switch <ブランチ名>: ブランチを切り替え  
git statusでも現在のブランチを確認できる  
```bash
$ git status
On branch main
```

[注意]GitHubのプルリクエストにて、フォークを行ったときはリポジトリの指定も必要。  
自分のアカウントのリモートリポジトリを選択しなおす必要がある。  

プルリクエストのレビュアーに指定できるのは、リポジトリごとの「コラボレーター」となっているアカウントのみ。  
リポジトリのSettings->Collaboratorsから登録する。  

マージには3種類ある。  
- マージコミット：トピックブランチのコミットを全て保持する  
- スカッシュマージ：トピックブランチのコミットを1つのコミットにまとめ、マージする  
マージ後はトピックブランチのコミット単位での履歴が確認できなくなる
- リベース：ブランチの枝分かれ元を変更(リベース)してからマージする  
コミット履歴が一直線になる

スカッシュ、リベースは履歴の書き換えを行うことになる点に注意。

プルとフェッチ：  
どちらもリモートリポジトリの内容がローカルリポジトリに反映される。  
フェッチはそこまで。プルはさらにワークツリーへの反映(マージ)まで行われる。  
"プル＝フェッチ＋マージ" ともいえる。

GitHubフロー / Gitフロー：  
作業内容ごとにブランチを使い分ける。  
1つのブランチで複数の作業を行うと、問題の切り分けや部分的な切り戻しがやりづらくなってしまう。  

### Chapter6
git switch -c <ブランチ名>: ブランチを作成＆スイッチ  
git checkout -b <ブランチ名> でもOK  
ローカルでブランチを作成した場合、初回プッシュ時にリモートにもブランチを作成し、紐づけする作業が必要。(必須ではないが、普通はやるっぽい)  
git push -u origin <ブランチ名>  

git reset <ハッシュ値>,  
git reset HEAD^ ,
git reset HEAD@{2} ,等:  
現在のブランチの先頭(HEAD)を特定のコミット地点に移動させる。  
コミットを「なかったことにできる」。  
HEAD^で1つ前のコミットを示す(だいたい)。HEAD^^で2つ前。  

git reflog: ローカルリポジトリにてHEADやブランチが指すコミットの履歴を表示

git push -f:  
ローカルでgit resetしたものをリモートへプッシュする場合。  
歴史の改ざんは基本的にNGだし、自身の使用経験も少ないため注意。

git merge <ブランチX>: 現在のブランチに、ブランチXをマージ  

### Chapter7
(実践が少ないので、実際にやる際は要チェック。GitHubの仕様変更や、リポジトリごとのルールの違いなどあるため)  
プルリクエストにて `#1` と書くと、そのIssueにリンクされる。  
また `fix #1` と書くと、プルリクエストのマージ時にIssueを自動でcloseしてくれる。  
