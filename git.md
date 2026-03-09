
## 「いちばんやさしいgit&githubの教本」を読みながらキーワードやコマンド結果をメモしたもの

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
git restore --stated (ファイル名): ステージングエリアへの登録を取り消し(=git addのキャンセル)  

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

git remote -v: リモートリポジトリの設定確認 git@github.com で始まっていればSSH接続
```bash
$ git remote -v
origin  git@github.com:hoshinon57/ichiyasaGitSample.git (fetch)
origin  git@github.com:hoshinon57/ichiyasaGitSample.git (push)
```

origin: クローン元のリモートリポジトリを表す名前  
Gitが初期値として"origin"を設定し、これは変えることもできるが、普通はoriginのまま。  
Gitでは1つのローカルリポジトリに対してリモートリポジトリを複数設定できるらしい。

start (フォルダ名) でエクスプローラーを起動できる。  
`start .` で作業中のカレントディレクトリを開けるのは便利かも。

### Chapter5
