# stephia-uptime — 外部からの副監視

[![uptime-check](https://github.com/tatsu223/stephia-uptime/actions/workflows/uptime.yml/badge.svg)](https://github.com/tatsu223/stephia-uptime/actions/workflows/uptime.yml)

stephia.co の公開3サイトを **Mac の外（GitHub Actions）から10分間隔で死活確認**する第二系統の監視。
母艦の Uptime Kuma（60秒間隔・詳細確認用）がスリープ・停電・回線断で止まっている間の空白を埋める。

## 監視対象

- https://master.stephia.co
- https://dashboard.stephia.co
- https://members.stephia.co

## 動き

- 10分ごとに3サイトへ HTTP アクセス（各サイト最大3回リトライ・20秒タイムアウト）
- 異常を検知すると `incident` ラベル付き Issue を自動作成（→ メール通知が届く）
- 復旧を確認すると同じ Issue にコメントして自動クローズ
- ワークフロー自体の失敗もメール通知の対象

## 運用

- 手動実行: Actions タブ → uptime-check → Run workflow
- 監視対象の変更: `.github/workflows/uptime.yml` の `sites=` を編集
- 外部ツール・外部トークン不使用（GitHub 標準の権限だけで動く）
- 母艦側の運用正本: `~/.claude/services/README.md`（2026-07-18 Codex 監査の指摘「スリープ中の監視空白」への手当てとして新設）
