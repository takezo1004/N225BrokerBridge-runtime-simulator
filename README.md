# N225BrokerBridge-runtime-simulator

N225BrokerBridge を **シミュレータモード (`--simulator`)** で動かすための装備一式。第 1 話・第 2 話の作業範囲をカバーする **Public** リポ。

> ⚠️ **このリポジトリ単体では動きません**
> [`N225BrokerBridge-public`](https://github.com/takezo1004/N225BrokerBridge-public) (Bridge コード本体) と組み合わせて使います。Bridge 本体の中に `runtime/simulator/` サブフォルダとして配置します。

---

## このリポジトリの位置づけ

```
[購読者の PC]
C:\Users\<username>\
└── N225BrokerBridge-public\          ← Public bridge を clone (1 回だけ)
    ├── bridge\                        ← Bridge コード本体
    └── runtime\
        └── simulator\                 ← このリポを clone してここに展開
            ├── CLAUDE.md
            ├── .claude\commands\
            ├── dashboard\
            └── webhook_test\
```

第 1 話 (本リポ・Public) はシミュレータでブリッジを動かす範囲。第 3 話で解放される `N225BrokerBridge-runtime-production` (Private) は本番接続 (kabu / TV / Cloudflare) 用です。

---

## このリポに含まれるもの

```
N225BrokerBridge-runtime-simulator/
├── CLAUDE.md              ← Claude Code が最初に読む基本ガイド (シミュレータ版)
├── README.md              ← このファイル
├── requirements.txt       ← Python 依存物 (dashboard 用)
├── .gitignore
├── .claude/
│   ├── commands/
│   │   ├── install.md     ← /install: 新規 PC 環境構築 (Phase 1〜6)
│   │   ├── setup.md       ← /setup: 全自動セットアップ
│   │   ├── verify.md      ← /verify: 動作確認
│   │   └── diagnose.md    ← /diagnose: トラブル時の自動診断
│   └── skills/
│       └── (シミュレータ範囲のスキル、本番接続スキルは production リポへ)
├── dashboard/                 ← (sync_to で配置) シミュレータテストダッシュボード
│   ├── n225_simulator_test_dashboard.py
│   ├── launch_simulator_test_dashboard.bat
│   └── webhook_test/          ← 7 種類の Webhook ペイロード
└── templates/                 ← 設定テンプレ (appsettings.Local.simulator.json.example 等)
```

---

## 使い方 (購読者向け)

### 1. 両方のリポを clone (両方 Public、認証不要)

```powershell
cd C:\Users\<your-name>\
git clone https://github.com/takezo1004/N225BrokerBridge-public.git
cd N225BrokerBridge-public\
mkdir runtime
git clone https://github.com/takezo1004/N225BrokerBridge-runtime-simulator.git runtime\simulator
```

### 2. Claude Code を起動して `/install` を実行

```powershell
cd C:\Users\<your-name>\N225BrokerBridge-public\
claude
```

Claude Code 起動後、本リポの `runtime/simulator/.claude/commands/install.md` に従って **Phase 1〜6** の環境構築が自動で進みます。

詳細は第 1 話 (note マガジン) を参照。

---

## 動作要件 (シミュレータモード)

- Windows 10 (1809+) / Windows 11 (x64)
- .NET 8 SDK + Desktop Runtime
- Python 3.10+
- Git
- **Claude Code Pro / Max (推奨、本リポの中核)** — 手動セットアップも可能、ただし著者へ要マニュアル送付依頼

本番運用 (kabu Station / TradingView Plus / Cloudflare Tunnel / OSE データ等) は第 3 話以降で扱います。

---

## バージョン

v0.2.0 (2026-05-27、3 リポ + 段階購入モデルへ再編、Public 化)

---

## ライセンス

Proprietary — 個人利用のみ。再配布禁止。
