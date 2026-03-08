
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