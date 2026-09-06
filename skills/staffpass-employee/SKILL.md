---
name: staffpass-employee
description: use this when operating under a Staffpass AI employee badge — whoami, gated invoke, approval poll, and health checks against the hosted Staffpass MCP
---

# Staffpass 社員証（employee badge）

手足（Grok Bot / Cursor）から Staffpass Gateway を使うときの手順です。このプラグインは **リモートMCPの薄い接続設定** です。サーバ本体は `https://staffpass.sealith.com` にあります。

## 前提

- 認証は社員証キー `gb_emp_…` のみ（プラグイン変数 `STAFFPASS_EMPLOYEE_KEY`）
- **管理用 `gb_adm_…` は使わない**（別口・混ぜると失敗する）
- 公開ドキュメント: https://staffpass.sealith.com/docs/mcp

## ツール

- `staffpass_whoami` — いまのAI社員・権限・binding
- `staffpass_invoke` — 実行（`tool` + `purpose` + `jobId` 必須）
- `staffpass_get_approval_status` — 承認待ちの確認
- `staffpass_health` — 接続・ランタイム確認

## 手順

1. 危険な操作の前に `staffpass_whoami` で社員証が生きているか確認する
2. 実行は `staffpass_invoke`。必ず `purpose` と `jobId` を付ける
3. 結果が `needs_approval` のときは **承認されるまで完了させない**
   - `approvalId` と `statusToken` で `staffpass_get_approval_status` を poll する
   - `approved` になったら同じ `jobId` で `approvalId` を付けて再 `invoke`
   - `revision_requested` なら指示どおり直して同じ `jobId` + `parentApprovalId` で再提出
4. 不通のときは `staffpass_health`

## やってはいけないこと

- Staffpassが egress を握っている操作を、Slack / Gmail など別コネクタの送信で迂回する
- チャットに `gb_emp_` 生鍵を貼って使い回す（変数／秘密入力に入れる）
- 管理MCPと社員証MCPのヘッダを混ぜる

## 鍵の入手

Staffpass ダッシュボードで AI社員を雇う → 一度だけ表示される `gb_emp_…` をコピー → このプラグインの設定に入れる。
