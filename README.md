# Staffpass（Cursor / Grok Bot プラグイン）

[Staffpass](https://staffpass.sealith.com) の **AI社員・社員証 MCP** を、Cursor / Grok Bot からワンパッケージでつなぐための薄いプラグインです。

> このリポジトリは **接続設定のラッパー** です。Gateway・承認・監査の本体はホストされた  
> `https://staffpass.sealith.com/api/mcp` にあります。制御プレーンのソースコードは含みません。

## できること

インストール後、次のツールが使えます。

| Tool | 用途 |
|------|------|
| `staffpass_whoami` | いまのAI社員・権限 |
| `staffpass_invoke` | 許可された操作の実行（purpose + jobId） |
| `staffpass_get_approval_status` | 人の承認待ちを確認 |
| `staffpass_health` | 接続確認 |

confirm / send / order などは **人が承認するまで完了しません**。

## インストール

### マーケットプレイス（掲載後）

Cursor の Customize / Marketplace から **Staffpass** を Installし、社員証キーを設定してください。

### カスタム MCP（いますぐ）

1. [Staffpass](https://staffpass.sealith.com/signup) で Org を作成し、ダッシュボードから AI社員を雇う  
2. 一度だけ表示される社員証キー `gb_emp_…` を控える（**`gb_adm_…` ではない**）  
3. Grok Bot / Cursor の Plugins・コネクタでリモート MCP を追加  
   - URL: `https://staffpass.sealith.com/api/mcp`  
   - Header: `Authorization: Bearer gb_emp_…`  
4. 詳しい手順: https://staffpass.sealith.com/docs/mcp

このプラグインを入れる場合は、上記キーを変数 `STAFFPASS_EMPLOYEE_KEY` に設定します（リポジトリに鍵を書かないでください）。

## 含めないもの

- 管理MCP（`gb_adm_…` / `/api/mcp/admin`）— テナント管理はダッシュボード、または別途管理口  
- Staffpass 本体アプリケーションのソース

## 開発者向け構成

```
.cursor-plugin/plugin.json  # マニフェスト + 変数スキーマ
mcp.json                    # リモート MCP（URL + Bearer プレースホルダ）
skills/staffpass-employee/  # 利用手順スキル
assets/logo.svg
LICENSE                     # MIT
```

ローカル確認は Cursor のプラグインローカル配置手順に従ってください。  
公式ドキュメント: https://cursor.com/docs/plugins

## リンク

- 製品: https://staffpass.sealith.com  
- MCPガイド: https://staffpass.sealith.com/docs/mcp  
- トライアル: https://staffpass.sealith.com/signup  
- Sealith: https://www.sealith.com  

## License

MIT © TOKYO307 Inc.
