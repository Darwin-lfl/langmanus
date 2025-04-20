# LangManus

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[English](./README.md) | [简体中文](./README_zh.md) | [日本語](./README_ja.md)

> オープンソースから来て、オープンソースに戻る

LangManusは、オープンソースコミュニティの素晴らしい仕事に基づいて構築された、コミュニティ主導のAI自動化フレームワークです。私たちの目標は、言語モデルをウェブ検索、クロール、Pythonコードの実行などの専門ツールと組み合わせることです。

## デモビデオ

> **タスク**: HuggingFace上のDeepSeek R1の影響指数を計算します。この指数は、フォロワー、ダウンロード、いいねなどの要素の重み付け合計を考慮して設計できます。

[![デモ](./assets/demo.gif)](./assets/demo.mp4)

- [YouTubeで視聴](https://youtu.be/sZCHqrQBUGk)
- [ビデオをダウンロード](https://github.com/langmanus/langmanus/blob/main/assets/demo.mp4)

## 目次
- [クイックスタート](#クイックスタート)
- [アーキテクチャ](#アーキテクチャ)
- [機能](#機能)
- [なぜLangManusなのか？](#なぜlangmanusなのか)
- [セットアップ](#セットアップ)
    - [前提条件](#前提条件)
    - [インストール](#インストール)
    - [設定](#設定)
- [使用方法](#使用方法)
- [Web UI](#web-ui)
- [開発](#開発)
- [貢献](#貢献)
- [ライセンス](#ライセンス)
- [謝辞](#謝辞)

## クイックスタート

```bash
# リポジトリをクローン
git clone https://github.com/langmanus/langmanus.git
cd langmanus

# uvを使用して仮想環境を作成してアクティブ化
uv python install 3.12
uv venv --python 3.12

source .venv/bin/activate  # Windowsの場合: .venv\Scripts\activate

# 依存関係をインストール
uv sync

# 環境を設定
cp .env.example .env
# .envファイルを編集してAPIキーを入力

# プロジェクトを実行
uv run main.py
```

## アーキテクチャ

LangManusは、スーパーバイザーが専門のエージェントを調整して複雑なタスクを達成する階層型マルチエージェントシステムを実装しています。

![LangManusアーキテクチャ](./assets/architecture.png)

システムは以下のエージェントで構成されています。

1. **コーディネーター** - 初期のインタラクションを処理し、タスクをルーティングするエントリーポイント
2. **プランナー** - タスクを分析し、実行戦略を作成
3. **スーパーバイザー** - 他のエージェントの実行を監督および管理
4. **リサーチャー** - 情報を収集および分析
5. **コーダー** - コードの生成および修正を担当
6. **ブラウザー** - ウェブブラウジングおよび情報の取得を実行
7. **レポーター** - ワークフロー結果のレポートおよび要約を生成

## 機能

### コア機能
- 🤖 **LLM統合**
    - Qwenなどのオープンソースモデルのサポート
    - OpenAI互換のAPIインターフェース
    - 異なるタスクの複雑さに対応するマルチティアLLMシステム

### ツールと統合
- 🔍 **検索と取得**
    - Tavily APIを介したウェブ検索
    - Jinaによるニューラル検索
    - 高度なコンテンツ抽出

### 開発機能
- 🐍 **Python統合**
    - 組み込みのPython REPL
    - コード実行環境
    - uvによるパッケージ管理

### ワークフロー管理
- 📊 **可視化と制御**
    - ワークフローグラフの可視化
    - マルチエージェントのオーケストレーション
    - タスクの委任と監視

## なぜLangManusなのか？

私たちはオープンソースの協力の力を信じています。このプロジェクトは、以下のような素晴らしいプロジェクトの助けなしには実現できませんでした。
- [Qwen](https://github.com/QwenLM/Qwen) - オープンソースのLLMを提供
- [Tavily](https://tavily.com/) - 検索機能を提供
- [Jina](https://jina.ai/) - ニューラル検索技術を提供
- その他多くのオープンソースの貢献者

私たちはコミュニティに還元することを約束し、コード、ドキュメント、バグレポート、機能提案など、あらゆる種類の貢献を歓迎します。

## セットアップ

### 前提条件

- [uv](https://github.com/astral-sh/uv) パッケージマネージャー

### インストール

LangManusは、依存関係の管理を簡素化するために[uv](https://github.com/astral-sh/uv)を使用しています。
以下の手順に従って仮想環境を設定し、必要な依存関係をインストールします。

```bash
# ステップ1: uvを使用して仮想環境を作成してアクティブ化
uv python install 3.12
uv venv --python 3.12

source .venv/bin/activate  # Windowsの場合: .venv\Scripts\activate

# ステップ2: プロジェクトの依存関係をインストール
uv sync
```

これらの手順を完了することで、環境が適切に設定され、開発の準備が整います。

### 設定

LangManusは、推論、基本タスク、視覚言語タスクのための3層のLLMシステムを使用しています。プロジェクトのルートに`.env`ファイルを作成し、以下の環境変数を設定します。

```ini
# 推論LLMの設定（複雑な推論タスク用）
REASONING_MODEL=your_reasoning_model
REASONING_API_KEY=your_reasoning_api_key
REASONING_BASE_URL=your_custom_base_url  # オプション

# 基本LLMの設定（簡単なタスク用）
BASIC_MODEL=your_basic_model
BASIC_API_KEY=your_basic_api_key
BASIC_BASE_URL=your_custom_base_url  # オプション

# 視覚言語LLMの設定（画像を含むタスク用）
VL_MODEL=your_vl_model
VL_API_KEY=your_vl_api_key
VL_BASE_URL=your_custom_base_url  # オプション

# ツールAPIキー
TAVILY_API_KEY=your_tavily_api_key
JINA_API_KEY=your_jina_api_key  # オプション

# ブラウザの設定
CHROME_INSTANCE_PATH=/Applications/Google Chrome.app/Contents/MacOS/Google Chrome  # オプション、Chrome実行ファイルのパス
```

> **注意:**
>
> - システムは異なるタイプのタスクに対して異なるモデルを使用します。
>     - 推論LLMは複雑な意思決定と分析のために使用されます。
>     - 基本LLMは簡単なテキストベースのタスクのために使用されます。
>     - 視覚言語LLMは画像理解を含むタスクのために使用されます。
> - すべてのLLMのベースURLを独立してカスタマイズできます。
> - 必要に応じて、各LLMに異なるAPIキーを使用できます。
> - Jina APIキーはオプションです。独自のキーを提供して、より高いレート制限にアクセスできます（APIキーは[jina.ai](https://jina.ai/)で取得できます）。
> - Tavily検索はデフォルトで最大5つの結果を返すように設定されています（APIキーは[app.tavily.com](https://app.tavily.com/)で取得できます）。

`.env.example`ファイルをテンプレートとしてコピーして開始できます。

```bash
cp .env.example .env
```

### プリコミットフックの設定
LangManusには、各コミット前にリンティングとフォーマットチェックを実行するプリコミットフックが含まれています。設定手順は以下の通りです。

1. プリコミットスクリプトを実行可能にする。
```bash
chmod +x pre-commit
```

2. プリコミットフックをインストールする。
```bash
ln -s ../../pre-commit .git/hooks/pre-commit
```

プリコミットフックは自動的に以下を実行します。
- リンティングチェックを実行（`make lint`）
- コードフォーマットを実行（`make format`）
- 再フォーマットされたファイルをステージングに追加
- リンティングまたはフォーマットエラーがある場合、コミットを防止

## 使用方法

### 基本的な実行

デフォルト設定でLangManusを実行します。

```bash
uv run main.py
```

### APIサーバー

LangManusは、ストリーミングサポートを備えたFastAPIベースのAPIサーバーを提供します。

```bash
# APIサーバーを起動
make serve

# または直接実行
uv run server.py
```

APIサーバーは以下のエンドポイントを公開します。

- `POST /api/chat/stream`: LangGraph呼び出し用のチャットエンドポイント（ストリーミングサポート付き）
    - リクエストボディ:
    ```json
    {
      "messages": [
        {"role": "user", "content": "Your query here"}
      ],
      "debug": false
    }
    ```
    - エージェントの応答を含むサーバー送信イベント（SSE）ストリームを返します。

### 高度な設定

LangManusは、`src/config`ディレクトリ内のさまざまな設定ファイルを通じてカスタマイズできます。
- `env.py`: LLMモデル、APIキー、およびベースURLを設定
- `tools.py`: ツール固有の設定を調整（例：Tavily検索結果の制限）
- `agents.py`: チーム構成およびエージェントシステムプロンプトを変更

### エージェントプロンプトシステム

LangManusは、`src/prompts`ディレクトリ内の洗練されたプロンプトシステムを使用して、エージェントの行動と責任を定義します。

#### コアエージェントの役割

- **スーパーバイザー（[`src/prompts/supervisor.md`](src/prompts/supervisor.md)）**: リクエストを分析し、どの専門家が処理するかを決定することでチームを調整し、タスクを割り当てます。タスクの完了とワークフローの移行について決定を下します。

- **リサーチャー（[`src/prompts/researcher.md`](src/prompts/researcher.md)）**: ウェブ検索とデータ収集を通じて情報収集を専門とします。Tavily検索とウェブクロール機能を使用し、数学的計算やファイル操作を避けます。

- **コーダー（[`src/prompts/coder.md`](src/prompts/coder.md)）**: Pythonおよびbashスクリプトに焦点を当てたプロフェッショナルなソフトウェアエンジニアの役割です。以下を処理します。
    - Python��ードの実行と分析
    - シェルコマンドの実行
    - 技術的な問題解決と実装

- **ファイルマネージャー（[`src/prompts/file_manager.md`](src/prompts/file_manager.md)）**: ファイルシステム操作を処理し、markdown形式でコンテンツを適切にフォーマットして保存することに重点を置きます。

- **ブラウザー（[`src/prompts/browser.md`](src/prompts/browser.md)）**: ウェブインタラクションの専門家であり、以下を処理します。
    - ウェブサイトのナビゲーション
    - ページのインタラクション（クリック、入力、スクロール）
    - ウェブページからのコンテンツ抽出

#### プロンプトシステムのアーキテクチャ

プロンプトシステムは、テンプレートエンジン（[`src/prompts/template.py`](src/prompts/template.py)）を使用して以下を行います。
- 役割固有のmarkdownテンプレートをロード
- 変数の置換を処理（例：現在の時間、チームメンバー情報）
- 各エージェントのシステムプロンプトをフォーマット

各エージェントのプロンプトは個別のmarkdownファイルで定義されており、基礎となるコードを変更せずに行動と責任を簡単に変更できます。

## Web UI

LangManusはデフォルトのWeb UIを提供します。

詳細については、[langmanus/langmanus-web](https://github.com/langmanus/langmanus-web)プロジェクトを参照してください。

## 開発

### テスト

テストスイートを実行します。

```bash
# すべてのテストを実行
make test

# 特定のテストファイルを実行
pytest tests/integration/test_workflow.py

# カバレッジを使用して実行
make coverage
```

### コード品質

```bash
# リンティングを実行
make lint

# コードをフォーマット
make format
```

## 貢献

あらゆる種類の貢献を歓迎します！タイプミスの修正、ドキュメントの改善、新機能の追加など、あなたの助けを感謝します。始める方法については、[貢献ガイド](CONTRIBUTING.md)を参照してください。

## ライセンス

このプロジェクトはオープンソースであり、[MITライセンス](LICENSE)の下で利用可能です。

## 謝辞

LangManusを可能にするすべてのオープンソースプロジェクトと貢献者に特別な感謝を捧げます。私たちは巨人の肩に立っています。
