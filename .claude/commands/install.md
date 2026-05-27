# /install — 環境構築 (Bridge 本体のビルドまで)

`/setup` の中で呼ばれる「ソフトウェアインストール + Bridge ビルド」のサブコマンド。
単独で実行することもできる (例: 認証情報設定は後でやりたい時)。

> ⚠️ ドラフト (v0.1.0)。テスター環境で動作確認が必要。

## 目的

利用者の PC に Bridge を動かすのに必要なソフトウェア (.NET 8 SDK / Python 3.10+ / Git / cloudflared) を揃え、`N225BrokerBridge-public` をビルドして実行可能な状態にする。

**kabu Station / TradingView Pro+ / Cloudflare ドメインの設定は本コマンドの対象外**。これらは `/setup` の Step 6〜7 で対話的にカバーする。

## 前提

- `N225BrokerBridge-public` と `N225BrokerBridge-runtime` が同じ親フォルダ配下に clone 済 (例: `C:\Users\<name>\N225BrokerBridge-public\` と `C:\Users\<name>\N225BrokerBridge-runtime\`)
- 本コマンドは `runtime` リポを cwd にした状態で `claude` を起動した前提で動く

## 実行フロー

### Step 1: 隣接する public リポを発見

```
Claude Code 発話:
「public リポ (コード本体) の場所を確認します。
このフォルダの親に N225BrokerBridge-public がありますか?」
```

確認方法:
```powershell
Test-Path (Join-Path (Split-Path -Parent (Get-Location)) 'N225BrokerBridge-public\bridge\N225BrokerBridge.sln')
```

→ false なら「両リポを同じ親フォルダに置いてください」と案内して中断。

### Step 2: 必須ソフトウェア確認

| 項目 | 確認 | 不足時の案内 |
|---|---|---|
| **.NET 8 SDK** | `dotnet --list-sdks` で 8.x.x | https://dotnet.microsoft.com/download/dotnet/8.0 |
| **.NET 8 Desktop Runtime** | `dotnet --list-runtimes` で `Microsoft.WindowsDesktop.App 8.x` | 同上 |
| **Python 3.10+** | `python --version` で 3.10〜3.12 | https://www.python.org/downloads/ |
| **Git** | `git --version` | https://git-scm.com/downloads |
| **cloudflared** | `where.exe cloudflared` | https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/ |

不足項目があれば利用者に確認した上で順次インストール案内 (`winget install` コマンドを提示)。

### Step 3: Bridge 本体ビルド

```powershell
cd ..\N225BrokerBridge-public\bridge
dotnet build N225BrokerBridge.sln --configuration Debug
```

ビルド失敗時は `dotnet --list-sdks` と `dotnet --list-runtimes` を再確認、必要なら .NET SDK の再インストールを案内。

### Step 4: Python venv 作成 (dashboard / analysis 用)

```powershell
cd ..\..\N225BrokerBridge-public
python -m venv .venv
.venv\Scripts\activate
pip install -r ..\N225BrokerBridge-runtime\requirements.txt
```

### Step 5: 動作確認 — シミュレータ起動

```powershell
cd ..\N225BrokerBridge-public\bridge\src\N225BrokerBridge.UI\bin\Debug\net8.0-windows
.\N225BrokerBridge.UI.exe --simulator
```

期待動作:
- 起動ログに「シミュレータモードで起動します」が出る
- MainWindow が開いて左上に **SIMULATOR バッジ** (黄色) が表示される
- 現在値が 55,600 円付近で 1 秒ごとに揺らぐ
- 手動発注ボタンを押すと確認ダイアログ「Mock ブローカーへ発注します」が出る → OK で建玉一覧に追加される

シミュレータが動けば、kabu / TV / Cloudflare をまだ設定していなくても **Bridge コア部分は OK**。

### Step 6: 完了案内

```
Claude Code 発話:
「Bridge のビルド + シミュレータ起動まで成功しました。
次のステップは利用者の選択次第です:

  A. 本番環境 (kabu Station + TV Pro+ + Cloudflare) もセットアップ
     → /setup の Step 6〜7 を続けて実行 (kabu / TV / Cloudflare 設定)
  B. シミュレータで遊んでから決める
     → 何もせず exit、後で /setup を再実行

どちらにしますか?」
```

---

## 関連

- 全体フロー: [`/setup`](setup.md)
- 動作確認のみ: [`/verify`](verify.md)
- トラブル時: [`/diagnose`](diagnose.md)
- シミュレータ仕様詳細: `N225BrokerBridge-public/bridge/docs/simulator-mode.md`
- 設定テンプレ: [`../../templates/appsettings.Local.json.example`](../../templates/appsettings.Local.json.example)
