# コンテナ・バージョン管理・LLM練習用リポジトリ
## codespace 作成直後にやること
### 1. LLMセットアップ
- ターミナルで以下のコマンドを打ってEnter
```bash
bash bonsai_setup.sh
```
- どのモデルを使うか尋ねられたら, 8B, 4B, 1.7Bから選択
  - 1.7Bは早く動くが精度は低い
  - 4Bは動きが遅いが精度が改善されている
  - 8BはCodespaseではおすすめしない

### 2. LLM起動
- ターミナルをもう一つ起動
  - 1. でのターミナルはサーバとして起動している
- ターミナルで以下のコマンドを打ってEnter
```bash
bash bonsai_run.sh
```
- チャット窓が開くので, 自由に会話する

### 3. LLMの話し方を変えたい
- interface.pyの, messages =の行にシステムプロンプトが書いてあるので, 好きに改造する

## [全手順を手作業でやる場合だけ実行する]codespace 作成直後にやること
### 0. 必要なpythonファイルの作成
- まず, interface.pyを一番上の階層に作成する
  - interface.pyは, 本リポジトリの中にありますので, 内容コピペ
  - (時間があればここで)コミット, マージしてみよう
    - 左のソース管理アイコンを押し, interface.pyの+ボタンを押す。メッセージで変更内容を簡潔に書き, コミットボタンを押す
      - メッセージを間違えたらターミナルから git commit --amend -m "正しいメッセージ"
    - さらに変更を同期ボタンを押す
      - ここで変更がGitHubに同期される
    
### 1. 仮想環境を導入してローカルLLM・Bonsaiのセットアップ
- 以下の手順を, codespace上のターミナルで1行ずつ行う
```Bash
git clone https://github.com/PrismML-Eng/Bonsai-demo.git
cd Bonsai-demo
uv venv
source .venv/bin/activate
uv pip install openai
export BONSAI_MODEL=1.7B
./setup.sh
```
  - git cloneで, BonsaiのリポジトリからBonsai一式を落としてくる
  - cd Bonsai-demo で落としてきたBonsaiのフォルダに移動
  - uvで仮想環境venvを作成
    - 今回のPython環境はシステム管理されていて, そのままでは後述のopenaiをインストールできないため, 仮想環境venvの中にインストールする
    - Dockerによる仮想環境内で, さらにuvによる仮想環境venvを作るイメージ
  - source .venv/bin/activateで仮想環境venvに入る
  - uv pip install openaiで仮想環境venv下にopenaiライブラリをインストール
  - export BONSAI_MODEL=1.7Bで, セットアップ時に使うBonsaiのモデルサイズを 1.7B に指定する
  - ./setup.shで, Bonsaiのセットアップスクリプトを走らせる

### 2. Bonsaiサーバの起動
```Bash
scripts/start_llama_server.sh
```
- scripts/start_llama_server.shで, scripts フォルダ内の start_llama_server.sh を実行する

### 3. 別のターミナルを開いて, 対話用プログラムを起動
```Bash
cd Bonsai-demo
source .venv/bin/activate
uv pip install openai
python3 ../interface.py
```
- cd Bonsai-demo で落としてきたBonsaiのフォルダに移動
- source .venv/bin/activateで仮想環境venvに入る
- python3 interface.pyで対話用プログラムを起動
  - コマンドがpython3なことに注
  プロジェクト概要を追記