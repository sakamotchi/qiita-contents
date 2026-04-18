# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## このリポジトリについて

[`@qiita/qiita-cli`](https://github.com/increments/qiita-cli) で管理する Qiita 記事のコンテンツリポジトリ。アプリケーションコードは無く、YAML Front Matter 付きの Markdown ファイルが「ソース」であり、`main` / `master` ブランチから自動で Qiita に公開される。

## コマンド

すべてローカルインストールされた CLI を介して実行する (`npx qiita ...`)。

- `npx qiita preview` — ローカルプレビューサーバーを起動 (デフォルト http://localhost:8888 。`qiita.config.json` で設定)。初回起動時に Qiita 上の既存記事を取得する。
- `npx qiita new <basename>` — `public/<basename>.md` に Front Matter テンプレート付きの新規記事を作成する。
- `npx qiita pull` — Qiita から記事ファイルを取得して同期する。
- `npx qiita publish <basename>` — 単一記事を投稿/更新する。`--force` (`-f`) でローカル内容を強制反映。
- `npx qiita publish --all` — すべての記事を投稿/更新する。
- `npx qiita login` — トークンの初回登録 (リポジトリ外に保存される)。

## 記事のモデル

- 記事は `public/*.md` に配置する。1 ファイル = 1 Qiita 記事。サブディレクトリは CLI の規約外。
- 各ファイルは YAML Front Matter で始まる。重要なフィールド:
  - `id: null` → 初回公開時に記事の UUID が書き込まれる。**一度設定されたら手動で編集しないこと** — CLI がローカルファイルと Qiita 上の記事を紐付けるキーとなる。
  - `updated_at: ""` → 公開時に CLI によって上書きされる。
  - `private: true|false` — 限定共有記事か公開記事か。
  - `ignorePublish: true` — `publish` コマンドでこのファイルをスキップする。`--all` 実行時や GitHub Actions 経由でも公開したくない下書きに使う。
  - `tags`, `title`, `organization_url_name`, `slide` — そのまま。
- **削除は対称ではない**: `.md` ファイルを削除しても Qiita 上の記事は削除されない。削除は qiita.com 上で行う必要がある。

## 公開パイプライン

`.github/workflows/publish.yml` が `main` / `master` への push 時 (および `workflow_dispatch`) に `increments/qiita-cli/actions/publish@v1` を実行する。リポジトリシークレット `QIITA_TOKEN` が必要。デフォルトブランチへの merge は、`ignorePublish` 以外のすべての記事を公開/更新する — **merge = 本番デプロイ** として扱うこと。

Concurrency は in-progress のジョブをキャンセルしない設定なので、連続 push は破棄されず順次実行される。

## `qiita.config.json`

- `includePrivate: false` — `pull` で限定共有記事をダウンロードしない。ローカルで限定共有記事を編集したい場合は一時的に切り替えること。意図しない限り切り替えた値をコミットしない。
- `host` / `port` — プレビューサーバー専用。

## 参考リンク

- [Qiita CLI の使い方 (Qiita 公式記事)](https://qiita.com/Qiita/items/32c79014509987541130)
- [increments/qiita-cli (GitHub リポジトリ)](https://github.com/increments/qiita-cli)
