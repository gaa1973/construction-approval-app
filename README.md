# 朝礼・TBM-KY管理アプリ / Morning Meeting & TBM-KY Manager

> PowerApps + SharePoint + Power Automate で構築した現場朝礼・TBM-KY記録の承認管理システム

![demo](demo.gif)

---

## 概要 / Overview

建設現場の朝礼報告書を、電話・口頭・メールで行っていた承認連絡を
**アプリ一本で一元管理**するモバイルアプリ。

| 課題 | 解決 |
|------|------|
| 承認連絡が電話・口頭・メールで混在 | アプリで一元管理・通知 |
| 差し戻し理由が口頭で消えていた | アプリに記録・即時通知 |
| 承認状況が不透明 | 一覧画面でステータスを色分け可視化 |

---

## 主な機能 / Features

### 1. 朝礼記録の登録（Screen 1）
- 現場名・日付・出席者・体調確認・備考をスマホから入力
- 送信と同時に Power Automate 承認フローが自動起動

### 2. 承認状況の一覧表示（Screen 2）
- SharePoint リストをリアルタイム反映
- ステータスを色分けで一目確認
  - 🟢 承認済み（緑）
  - 🔵 未承認（青）
  - 🟡 差し戻し（黄）

### 3. 差し戻し対応（Screen 3）
- 差し戻し理由を即時確認
- フォームを修正して再提出

### 4. 自動承認フロー（Power Automate）
- 朝礼登録と同時にメール通知
- 承認 → SharePoint に「承認済み」を書き戻し
- 差し戻し → 理由コメントを SharePoint に自動書き戻し・アプリに反映

---

## 技術構成 / Tech Stack

| カテゴリ | 技術 |
|---------|------|
| フロントエンド | Power Apps（Canvas App / モバイルレイアウト） |
| データストア | SharePoint Online（朝礼記録リスト） |
| 業務自動化 | Power Automate（承認フロー） |
| ホスティング | Microsoft 365 / SharePoint |

---

## アーキテクチャ / Architecture

```
[現場担当者]
    │ スマホで入力・送信
    ▼
[PowerApps Screen1]
    │ SharePoint に書き込み
    ▼
[SharePoint: 朝礼記録リスト]
    │ アイテム作成トリガー
    ▼
[Power Automate: 朝礼承認フロー]
    │ メール通知
    ▼
[所長（承認者）]
    ├── 承認 → ステータス: 承認済み
    └── 差し戻し → ステータス: 差し戻し + 理由コメント
                    │
                    ▼
              [担当者へ通知・Screen3で確認・修正・再提出]
```

---

## SharePoint リスト構成

| 列名 | 種類 | 備考 |
|------|------|------|
| タイトル（現場名） | テキスト | |
| 日付 | 日付と時刻 | |
| 出席者 | 複数行テキスト | |
| 体調確認 | はい/いいえ | |
| 備考 | 複数行テキスト | |
| ステータス | 選択肢 | 未承認 / 承認済み / 差し戻し |
| 差し戻し理由 | 複数行テキスト | Power Automate が書き込む |

---

## セットアップ / Setup

> ⚠️ このアプリは Microsoft 365 / SharePoint 環境が必要です

1. SharePoint サイトに「朝礼記録」リストを上記列構成で作成
2. Power Apps Studio でアプリをインポート or 手動構築
3. Power Automate で承認フローを作成・対象リストに接続
4. Power Apps でデータソースを自環境の SharePoint リストに変更
5. アプリを公開・共有

---

## 作者 / Author

**shinobu** — Power Platform 学習中 / PL-900 取得済み

[![GitHub](https://img.shields.io/badge/GitHub-gaa1973-black?logo=github)](https://github.com/gaa1973)

---

## License

MIT
