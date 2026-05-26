# N225BrokerBridge-runtime

N225BrokerBridge を**自分の PC で動かすための知識セット**。

> ⚠️ **このリポジトリ単体では動きません**
> [`N225BrokerBridge-public`](https://github.com/takezo1004/N225BrokerBridge-public) (コード本体) と組み合わせて使います。

---

## このリポジトリの位置づけ

```
[購読者の PC]
│
├── git clone N225BrokerBridge-public          ← コード本体 (動かない)
│
├── git clone N225BrokerBridge-runtime         ← このリポ (動かす知識)
│
└── Claude Code を起動 → /setup 実行
        ↓
    runtime の CLAUDE.md と .claude/commands/setup.md が動き出す
        ↓
    Claude Code が対話で:
       - public リポの場所を確認
       - 前提ツール (.NET 8 / kabu Station / Cloudflare Tunnel / Python) の確認 + インストール支援
       - DPAPI 暗号化で認証情報を保存
       - C# プロジェクトをビルド
       - Python venv 作成 + 依存物適用
       - 動作確認
```

---

## このリポに含まれるもの

```
N225BrokerBridge-runtime/
├── CLAUDE.md              ← Claude Code が最初に読む基本ガイド
├── README.md              ← このファイル
├── .gitignore
├── .claude/
│   ├── commands/
│   │   ├── setup.md       ← /setup: 全自動セットアップ
│   │   ├── verify.md      ← /verify: 動作確認
│   │   ├── diagnose.md    ← /diagnose: トラブル時の自動診断
│   │   └── analyze.md     ← /analyze: 朝の市場分析
│   └── skills/
│       ├── kabu-config/        ← kabu Station 設定支援
│       ├── tv-webhook/         ← TradingView Webhook 設定支援
│       └── cloudflare-tunnel/  ← Cloudflare Tunnel 設定支援
├── docs/                  ← 詳細な手順書 (将来追加)
└── templates/             ← 設定ファイルテンプレ (将来追加)
```

---

## 使い方 (購読者向け)

### 1. 両方のリポを clone

```powershell
cd C:\Users\<your-name>\
git clone https://github.com/takezo1004/N225BrokerBridge-public.git
git clone https://github.com/takezo1004/N225BrokerBridge-runtime.git
```

### 2. Claude Code を起動

```powershell
cd N225BrokerBridge-runtime
claude
```

### 3. `/setup` を実行

```
> /setup
```

Claude Code が `CLAUDE.md` と `.claude/commands/setup.md` を読み込んで、対話的にセットアップを進めます。

---

## 動作要件

- Windows 10 (1809+) / Windows 11 (x64)
- .NET 8 SDK
- kabu Station (au カブコム証券、API 接続可能プラン)
- Cloudflare アカウント (Tunnel 利用)
- TradingView Pro+ (Webhook 機能)
- **Claude Code Pro / Max (必須・本リポの中核)**
- Python 3.10+ (dashboard / analysis 用)
- TradingView MCP サーバー (analysis 用、別途案内)

---

## トラブル時

```
> /diagnose
```

を実行すれば、Claude Code が症状を自動診断します。

---

## バージョン

v0.0.1 (初期作成、配布前のドラフト状態)

---

## ライセンス

Proprietary — 購読者の個人利用のみ。再配布禁止。
