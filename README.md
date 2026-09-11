# stephia-uptime — 外部からの副監視

[![uptime-check](https://github.com/tatsu223/stephia-uptime/actions/workflows/uptime.yml/badge.svg)](https://github.com/tatsu223/stephia-uptime/actions/workflows/uptime.yml)

stephia.co の公開3サイトを **Mac の外（GitHub Actions）から10分間隔で死活確認**する第二系統の監視。
母艦の Uptime Kuma（60秒間隔・詳細確認用）がスリープ・停電・回線断で止まっている間の空白を埋める。

## 監視対象

- https://master.stephia.co
- https://dashboard.stephia.co
- https://members.stephia.co

## 動き

- 10分ごと（毎時 3,13,23,33,43,53 分）に3サイトへ HTTP アクセス（各サイト最大3回リトライ・20秒タイムアウト）
- 異常を検知すると `incident` ラベル付き Issue を自動作成（メールが届くかは GitHub の通知設定次第。既定では自分のリポジトリの Issue は通知対象）
- 障害が続く間は追加コメントを出さない（実行失敗が10分ごとの生存信号）。全サイト復旧で、開いている incident Issue をすべてコメント付きで自動クローズ
- 3サイト全滅かつ照合先（api.github.com）にも届かない時は、ランナー側の回線異常とみなして Issue を作らず実行失敗だけにする

## 運用

- 手動実行: Actions タブ → uptime-check → Run workflow
- 監視対象の変更: `.github/workflows/uptime.yml` の `sites=` を編集
- 第三者ツール・長期保存トークン不使用（GitHub が実行ごとに発行する一時トークンだけで動く）
- **制約**: 公開リポジトリは60日間更新が無いと定刻実行が自動停止する（GitHub 仕様）。対策として `.github/workflows/keepalive.yml` が毎週月曜 03:17 UTC に有効化の API を叩き、監視本体と自分自身の停止を防ぐ（2026-09-11 追加・空コミットは作らない）。それでも止まったら Actions タブから有効化し直す
- **検証状態（2026-07-18）**: 正常系（3サイト応答あり）の試走は合格。障害時の Issue 作成・復旧クローズ・メール到達は未実測
- 母艦側の運用正本: `~/.claude/services/README.md`（2026-07-18 Codex 監査の指摘「スリープ中の監視空白」への手当てとして新設）
