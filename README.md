## アプリケーション概要

本アプリケーションは、社内向けのヘルプデスク業務を効率化するための
チケット管理システムです。

社員からの問い合わせをチケットとして登録・管理し、
サポート担当者および管理者が対応状況を一元的に把握・更新できることを目的としています。

ユーザーの権限（社員／サポート担当／システム管理者）に応じて
利用可能な機能や表示内容が切り替わる設計となっています。

## 利用想定

- 社員は、業務上の問い合わせやトラブルをチケットとして登録し、
　自身が作成したチケットの状況を確認できます。

- サポート担当者は、社員から登録されたチケットを取得・担当し、
　ステータス更新やコメントを通じて対応を行います。

- システム管理者は、全チケットの管理に加えて、
　ユーザーアカウントの登録・停止・再開などの管理操作を行います。

## 使用技術

### バックエンド
- Python
- FastAPI
- SQLAlchemy
- Alembic

### データベース
- MySQL

### 開発・実行環境
- Docker
- Docker Compose
- Uvicorn

### 依存管理
- Poetry

### 開発・品質管理
- pytest
- ruff

## ディレクトリ構成
```bash
src/kakeibo_be/
├─ api/
├─ core/
├─ exceptions/
├─ handlers/
├─ loggers/
├─ logic/
├─ migrations/
├─ models/
├─ repositories/
├─ store/
└─ tests/
```
### 主要ディレクトリの役割

- api：FastAPI のエンドポイント定義を管理する API 層
- core：DB接続や環境設定など、アプリ全体の基盤処理
- exceptions：アプリ独自の例外クラス定義
- handlers：例外処理など、アプリ全体のハンドリング処理
- loggers：ログ出力に関する共通設定・処理
- logic：日付計算など、共通で利用するロジック
- migrations：Alembic による DB マイグレーション管理
- models：SQLAlchemy を用いた ORM モデル（テーブル定義）
- repositories：DBアクセス処理（CRUD・検索など）を集約
- store：Enum や定数など、全体で共有する定義
- tests：テスト用パッケージ雛形

<details>
<summary>ディレクトリ構成の方針</summary>

<h3>api</h3>

<pre><code>src/kakeibo_be/api/
</code></pre>

<p>
FastAPI のエンドポイント定義を配置します。<br />
HTTP リクエスト/レスポンスの受け渡しを担当し、<br />
ビジネスロジックや DB アクセスの詳細は持ちません。
</p>

<hr />

<h3>core</h3>

<pre><code>src/kakeibo_be/core/
</code></pre>

<p>
アプリケーション全体で共通となる基盤処理を管理します。<br />
DB接続設定や環境変数の読み込みなどを配置します。
</p>

<hr />

<h3>exceptions</h3>

<pre><code>src/kakeibo_be/exceptions/
</code></pre>

<p>
アプリケーション独自の例外クラスを定義するフォルダです。<br />
業務エラーなどを <code>BusinessException</code> として型で表現し、<br />
ビジネスロジックや API 層から <code>raise</code> するための例外定義を管理します。
</p>

<hr />

<h3>handlers</h3>

<pre><code>src/kakeibo_be/handlers/
</code></pre>

<p>
API 実行中に発生する例外を捕捉し、<br />
共通のレスポンス形式でクライアントへ返すための例外処理を集約するフォルダです。<br />
ルーターやビジネスロジックとは分離し、例外処理だけを担当する層として独立させています。
</p>

<hr />

<h3>loggers</h3>

<pre><code>src/kakeibo_be/loggers/
</code></pre>

<p>
ログ出力に関する共通設定・処理を管理します。<br />
アプリ全体で一貫したログ管理を行います。
</p>

<hr />

<h3>logic</h3>

<pre><code>src/kakeibo_be/logic/
</code></pre>

<p>
アプリ内で共通利用されるロジックや計算処理を配置します。<br />
日付計算などのユーティリティ的処理を管理します。
</p>

<hr />

<h3>migrations</h3>

<pre><code>src/kakeibo_be/migrations/
</code></pre>

<p>
Alembic を用いた DB マイグレーション管理用ディレクトリです。<br />
テーブル定義（models）を元に、スキーマ変更を管理します。
</p>

<hr />

<h3>models</h3>

<pre><code>src/kakeibo_be/models/
</code></pre>

<p>
SQLAlchemy を用いた ORM モデルを定義・管理します。<br />
</p>

<hr />

<h3>repositories</h3>

<pre><code>src/kakeibo_be/repositories/
</code></pre>

<p>
データベースへのアクセス処理（CRUD・検索）を集約します。<br />
API やロジック層から DB 操作を分離することで、責務を明確にします。
</p>

<hr />

<h3>store</h3>

<pre><code>src/kakeibo_be/store/
</code></pre>

<p>
アプリ全体で共有される定義を配置します。
</p>

<ul>
  <li>enum：Enum 定義</li>
  <li>定数・共通データ構造</li>
</ul>

<hr />

<h3>tests</h3>

<pre><code>src/kakeibo_be/tests/
</code></pre>

<p>
テストコードを配置するテスト用パッケージ雛形です。<br />
単体テストや API テストを管理します。
</p>

</details>



## 前提条件
- Docker
- Docker Compose（v2）
- Python 3.14.0
- Poetry 2.x.x

<details>
<summary>前提条件・環境構築手順（Python / Poetry が未インストールの場合）</summary>

<h3>必要な環境</h3>

<ul>
  <li>OS：macOS / Windows / Linux（動作確認・手順例は macOS）</li>
  <li>Python：<strong>3.14.0</strong></li>
  <li>パッケージ管理ツール：<strong>Poetry 2.x.x</strong></li>
</ul>

<p>
本プロジェクトでは、Python のバージョン差異による不具合を防ぐため、
<strong>Python のバージョンを完全に揃えること</strong>を前提としています。
</p>

<p>
以下では、<strong>pyenv を利用して Python を管理する方法</strong>を例に説明します。
</p>

<p>
Windows / Linux 環境については、各OSに適した Python バージョン管理ツールを使用し、
上記と同一の Python バージョンを設定してください。
</p>

<hr />

<h3>1. Homebrew のインストール（未導入の場合）</h3>

<pre><code>/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
</code></pre>

<p>
Homebrew は macOS 用のパッケージ管理システムです。
</p>

<p>
インストール後、以下で確認してください。
</p>

<pre><code>brew --version
</code></pre>

<p>
<strong>成功条件：</strong><br />
以下のように Homebrew のバージョンが表示されること。
</p>

<pre><code>Homebrew 5.0.12
</code></pre>

<hr />

<h3>2. pyenv のインストール</h3>

<pre><code>brew install pyenv
</code></pre>

<p>
pyenv は、Python 本体ではなく、
<strong>Python のバージョンを管理・切り替えるためのツール</strong>です。
</p>

<hr />

<h3>3. pyenv の初期設定（zsh）</h3>

<p>
まず、現在使用しているシェルを確認します。
</p>

<pre><code>echo $SHELL
</code></pre>

<p>
<strong>成功条件：</strong><br />
<code>/bin/zsh</code> または <code>/usr/bin/zsh</code> が表示されること。
</p>

<p>
次に、<code>~/.zshrc</code> をエディタで開きます。<br />
ファイルが存在しない場合は新規作成します。
</p>

<pre><code>touch ~/.zshrc
open ~/.zshrc
</code></pre>

<p>
以下の内容を <code>~/.zshrc</code> に追記してください。
</p>

<pre><code>export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
</code></pre>

<p>
編集後、ターミナルを再起動するか、以下のコマンドで設定を反映します。
</p>

<pre><code>source ~/.zshrc
</code></pre>

<hr />

<h3>4. インストール可能な Python バージョンの確認</h3>

<pre><code>pyenv install --list
</code></pre>

<p>
<strong>成功条件：</strong><br />
出力一覧の中に <code>3.14.0</code> が含まれていること。
</p>

<hr />

<h3>5. Python 3.14.0 のインストール</h3>

<pre><code>pyenv install 3.14.0
</code></pre>

<p>
※ バージョン番号の末尾に英字が付くもの（例: 3.14.0t）は使用しません。
</p>

<hr />

<h3>6. 使用する Python バージョンの切り替え</h3>

<pre><code>pyenv global 3.14.0
</code></pre>

<p>
切り替え後、以下で確認してください。
</p>

<pre><code>python -V
</code></pre>

<p>
<strong>成功条件：</strong>
</p>

<pre><code>Python 3.14.0
</code></pre>

<pre><code>pyenv versions
</code></pre>

<p>
<strong>成功条件：</strong><br />
<code>*</code> が付いた Python が <code>3.14.0</code> であり、以下のように表示されること。
</p>

<pre><code>  system
  3.13.9
* 3.14.0 
</code></pre>

<hr />

<h3>7. Poetry 2.x.x のインストール</h3>

<pre><code>brew install poetry
</code></pre>

<p>
インストール後、以下で確認してください。
</p>

<pre><code>poetry --version
</code></pre>

<p>
<strong>成功条件：</strong>
</p>

<pre><code>Poetry (version 2.x.x)
</code></pre>

<p>
コマンドが見つからない場合は、Poetry のインストール先が PATH に含まれているか確認してください。
</p>

<hr />

<h3>8.（任意）インストール状況の最終確認</h3>

<p>以下のコマンドを実行し、期待される出力が表示されることを確認してください。</p>

<pre><code>brew --version
python -V
pyenv versions
poetry --version
</code></pre>

<p><strong>確認ポイント（例）</strong></p>

<pre><code>Homebrew 5.x.x
Python 3.14.0
  system
* 3.14.0
Poetry (version 2.x.x)
</code></pre>

<p>
※ バージョン番号の細かい差異は問題ありません。  <br />
※ pyenv で指定した Python バージョンに <code>*</code> が付いていることを確認してください。
</p>

</details>


### 1. リポジトリをクローン
```bash
git clone https://github.com/nana-ichimaru/Helpdesk-BE.git
cd Helpdesk-BE
```

### 2. Poetry で Python を指定して依存関係を入れる
```bash
poetry install
```

### 3. 環境変数を設定（.env を作成）
アプリケーションおよびデータベース接続に必要な環境変数を設定します。  
`.env` ファイルにはパスワードなどの機密情報が含まれるため、git では管理されません。

リポジトリに含まれている `.env.example` をコピーして `.env` を作成し、  
必要に応じて各項目の値を自分の環境に合わせて編集してください。

### ４. Docker（依存サービス）を起動
```bash
docker compose up -d
```

## テーブル追加手順

新しいテーブル（モデル）を追加した場合は、  
`helpdesk_be/models/db/__init__.py` にモデルを import して公開対象に含めてください。

```bash
from helpdesk_be.models.db.xxx import XxxModel

__all__ = ["XxxModel"]
```

<details>
  <summary>この手順が必要な理由（Alembic の自動マイグレーション対応）</summary>

  <p>
    Alembic の <code>--autogenerate</code> は、
    SQLAlchemy の <code>Base.metadata</code> に登録されている
    テーブル定義を元に差分を生成します。
  </p>

  <p>
    モデルクラスは <strong>import されてクラス定義が評価された時点</strong>で
    <code>Base.metadata</code> に登録されるため、
    Alembic 実行時にモデルが import されていないと
    「テーブルが存在しない」と判断されてしまいます。
  </p>

  <p>
    <code>models/db/__init__.py</code> にモデルを集約し、
    <code>env.py</code> からまとめて import することで、
    追加したテーブルが確実に Alembic に認識されるようにしています。
  </p>
</details>