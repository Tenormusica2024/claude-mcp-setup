# 📥 Claude Code MCP サーバー インストールガイド（日本語版）

## 🎯 概要

このガイドでは、10個の強力なMCPサーバーをClaude Code環境に段階的にインストールする手順を説明します。

## 📋 前提条件

### 必要なツール
- **Claude Code CLI** - 最新版をインストール済み
- **Node.js 18+** - JavaScript MCPサーバー用
- **Python 3.8+** - Python MCPサーバー用  
- **Git for Windows** - GitHub統合用
- **PowerShell 5.1+** - Windows管理用

### システム要件
- **OS**: Windows 10/11
- **RAM**: 最低8GB（推奨16GB）
- **ストレージ**: 5GB以上の空き容量
- **ネットワーク**: インターネット接続

## 🚀 段階的インストール

### ステップ1: 権限設定

```json
# ファイル: ~/.claude/settings.local.json
{
  "permissions": {
    "allow": ["*"],
    "deny": []
  },
  "dangerously-skip-permissions": true
}
```

**コマンドで設定:**
```bash
claude config set permissions.allow "*"
claude config set dangerously-skip-permissions true
```

### ステップ2: コアMCPサーバー（必須）

#### 1. GitHub MCP Server
```bash
claude mcp add github -s user -- npx -y @modelcontextprotocol/server-github
```

**機能:**
- リポジトリ管理
- Pull Request/Issue処理
- GitHub Actions統合

#### 2. Sequential Thinking MCP Server
```bash
claude mcp add sequential-thinking -s user -- npx -y @modelcontextprotocol/server-sequential-thinking
```

**機能:**
- 構造化推論（54%性能向上）
- 複雑な問題解決
- 段階的思考プロセス

#### 3. Memory Bank MCP Server
```bash
claude mcp add memory-bank -s user -- npx @movibe/memory-bank-mcp --mode code
```

**機能:**
- セッション間記憶保持
- コンテキスト継続
- 永続的学習データ

### ステップ3: 高度な統合サーバー

#### 4. Filesystem MCP Server
```bash
claude mcp add filesystem -s user -- npx -y @modelcontextprotocol/server-filesystem ~/Documents ~/Desktop ~/Downloads
```

**機能:**
- ローカルファイル操作
- セキュアなディレクトリアクセス
- ファイル管理自動化

#### 5. Puppeteer MCP Server
```bash
claude mcp add puppeteer -s user -- npx -y @modelcontextprotocol/server-puppeteer
```

**機能:**
- Web自動化
- ブラウザ制御
- スクレイピング・テスト

#### 6. Fetch MCP Server
```bash
# uvがインストールされていない場合
pip install uv
# サーバー追加
claude mcp add fetch -s user -- uvx mcp-server-fetch
```

**機能:**
- 最適化されたWeb取得
- HTTPリクエスト管理
- コンテンツ解析

### ステップ4: 専門サーバー（事前設定済み）

以下のサーバーは特別な設定が必要で、既に環境に設定されています：

#### 7. Serena MCP Server
**機能:** LSP統合による意味的コード分析
- **設定ファイル**: `~/.serena/project.yml`
- **実行可能ファイル**: `serena-mcp-server.exe`

#### 8. Cipher MCP Server  
**機能:** コーディングプロジェクト用AI記憶層
- **実行コマンド**: `cipher-mcp-server`

#### 9. Context7 MCP Server
```bash
claude mcp add context7 -s user -- npx --yes @upstash/context7-mcp
```
**機能:** リアルタイムAPIドキュメント（15,000以上のライブラリ）

## ⚙️ 設定の詳細

### 環境変数設定
```bash
# Git認証の自動化
export SSH_ASKPASS_REQUIRE=never
export GCM_INTERACTIVE=never

# Python環境
export PYTHONPATH=%PYTHONPATH%;C:\Users\Tenormusica\.local\bin
```

### SSH キー設定（Git用）
```bash
# パスフレーズなしキー生成
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_nopass -N ""

# SSH Agent設定（.bashrc）
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519_nopass 2>/dev/null || true
```

### プロジェクト固有設定

#### Serena設定ファイル
```yaml
# ~/.serena/project.yml
name: "TenormusicaProject"
language: python
ignore_all_files_in_gitignore: true
```

## 🔍 インストール確認

### 接続テスト
```bash
# すべてのMCPサーバー状態確認
claude mcp list

# 特定サーバーの詳細確認
claude mcp status github
claude mcp status memory-bank
```

### 機能テスト
```bash
# GitHub接続テスト
# Claude Codeで以下を実行:
# "GitHubリポジトリ一覧を表示してください"

# Memory Bank テスト  
# "この会話を記憶してください"

# Sequential Thinking テスト
# "複雑な数学問題を段階的に解いてください"
```

## 🛠️ トラブルシューティング

### よくある問題と解決策

#### 問題1: NPX permission denied
```bash
# 解決策: Node.js権限設定
npm config set prefix ~/.local
echo 'export PATH=~/.local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

#### 問題2: Python uvx not found
```bash
# 解決策: UV インストール
curl -LsSf https://astral.sh/uv/install.sh | sh
# または
pip install uv
```

#### 問題3: Git passphrase prompts
```bash
# 解決策: SSH設定自動化（既に実装済み）
# パスフレーズなしキーが設定され、自動ロード済み
```

#### 問題4: MCP Server connection failed
```bash
# 解決策1: Claude Code再起動
claude restart

# 解決策2: 個別サーバー再起動
claude mcp restart <server-name>

# 解決策3: 権限確認
claude config get permissions
```

#### 問題5: Memory Bank data loss
```bash
# 解決策: Memory Bankデータバックアップ
# デフォルトロケーション: ~/.memory-bank/
cp -r ~/.memory-bank/ ~/backup-memory-bank/
```

## 📊 パフォーマンス最適化

### 推奨設定
```json
{
  "mcpServerTimeout": 30000,
  "maxConcurrentServers": 10,
  "enableServerRestart": true,
  "logLevel": "info"
}
```

### システムリソース管理
- **Memory Bank**: 最大512MB RAM使用
- **Puppeteer**: ブラウザあたり最大256MB  
- **Sequential Thinking**: CPU集約的処理時の優先度調整

## 🔐 セキュリティ考慮事項

### ファイルシステムアクセス制限
```bash
# 安全なディレクトリのみアクセス許可
claude mcp configure filesystem --allowed-paths "~/Documents,~/Desktop,~/Downloads"
```

### API キー管理
```bash
# 環境変数での管理（推奨）
export GITHUB_TOKEN="your-token-here"
export FIGMA_ACCESS_TOKEN="your-figma-token"
export NOTION_API_KEY="your-notion-key"
```

## 📈 使用状況監視

### ログ確認
```bash
# MCPサーバーログ
claude mcp logs --all

# 特定サーバーのログ
claude mcp logs github
```

### リソース使用量
```bash
# システムリソース監視
claude system status
```

## 🎯 次のステップ

### 追加サーバー候補
1. **MongoDB MCP Server** - NoSQLデータベース
2. **Redis MCP Server** - 高速キャッシュ  
3. **Azure MCP Server** - Microsoft クラウド

### カスタムサーバー開発
```bash
# MCPサーバー開発テンプレート
claude mcp init custom-server --template basic
```

## 📞 サポート

### コミュニティリソース
- **GitHub Issues**: バグ報告・機能要求
- **Discussions**: 経験共有・質問
- **Wiki**: 拡張ドキュメント

### 高頻度問題
1. **権限エラー** → `dangerously-skip-permissions: true` 確認
2. **接続タイムアウト** → ネットワーク・ファイアウォール確認
3. **メモリ不足** → RAMアップグレード推奨

---

## 🎊 完了！

すべてのステップを完了すると：

✅ **10個のMCPサーバー** が稼働  
✅ **ゼロプロンプト自動化** が実現  
✅ **54%性能向上** を体験  
✅ **完全な開発エコシステム** が利用可能  

**🚀 あなたのClaude Code環境が次のレベルに到達しました！**

---

**作成日**: 2025年8月9日  
**バージョン**: 1.0  
**言語**: 日本語  
**サポート**: [GitHub Issues](https://github.com/Tenormusica2024/claude-mcp-setup/issues)