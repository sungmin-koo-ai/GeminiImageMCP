# Gemini 画像生成器 MCP サーバー (修正版)

Claude DesktopでGoogle の Gemini AIを使用して高品質な画像を生成・編集できる MCP サーバーです。

<a href="https://glama.ai/mcp/servers/@sungmin-koo-ai/GeminiImageMCP">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/@sungmin-koo-ai/GeminiImageMCP/badge" alt="Gemini Image Generator MCP server" />
</a>

## 🚀 主な特徴

- **テキストから画像生成**: Gemini 2.0 Flash を使用したテキスト→画像変換
- **画像変換**: 既存の画像をテキストプロンプトで修正
- **多言語対応**: 日本語・韓国語・中国語プロンプトの自動英語翻訳・最適化
- **AI ファイル名生成**: プロンプト基準でファイル名を自動生成
- **ローカル保存**: 生成された画像を指定フォルダに自動保存
- **Claude チャット内表示**: 生成された画像をチャット画面で直接確認

## 🛠️ インストール要件

- **Python 3.11 以上**
- **Google Gemini API キー**
- **Claude Desktop** またはその他 MCP 互換クライアント

## 📋 ステップ1: Gemini API キー発行

1. [Google AI Studio API Keys ページ](https://aistudio.google.com/apikey) にアクセス
2. Google アカウントでログイン
3. **"Create API Key"** をクリック
4. 生成された API キーをコピー（後で使用）

## 💾 ステップ2: MCP サーバーインストール

### 自動インストール（推奨）
```bash
# リポジトリクローン
git clone https://github.com/sungmin-koo-ai/GeminiImageMCP.git
cd GeminiImageMCP

# 仮想環境作成・有効化
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# パッケージインストール
pip install -e .
```

### インストール確認
```bash
# サーバーが正常実行されるかテスト
python -m gemini_image_mcp.server
```
`Starting Gemini Image Generator MCP server...` メッセージが表示されれば成功！（Ctrl+CまたはCtrl+Z で終了）

## ⚙️ ステップ3: Claude Desktop 設定

### 設定ファイル場所
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

### 設定ファイル内容
```json
{
  "mcpServers": {
    "gemini-image-generator": {
      "command": "/Users/ユーザー名/GeminiImageMCP/venv/bin/python",
      "args": [
        "-m", "gemini_image_mcp.server"
      ],
      "env": {
        "GEMINI_API_KEY": "ここに実際のAPIキーを入力",
        "OUTPUT_IMAGE_PATH": "/Users/ユーザー名/Pictures/ai_generated"
      }
    }
  }
}
```

### 実際の設定例
```json
{
  "mcpServers": {
    "gemini-image-generator": {
      "command": "/Users/ユーザー名/GeminiImageMCP/venv/bin/python",
      "args": [
        "-m", "gemini_image_mcp.server"
      ],
      "env": {
        "GEMINI_API_KEY": "AIzaSy...(実際のAPIキー)",
        "OUTPUT_IMAGE_PATH": "/Users/ユーザー名/Pictures/ai_generated"
      }
    }
  }
}
```

### スクリプト命令を使用する場合（簡単設定）
```json
{
  "mcpServers": {
    "gemini-image-generator": {
      "command": "/Users/ユーザー名/GeminiImageMCP/venv/bin/gemini-image-mcp",
      "env": {
        "GEMINI_API_KEY": "AIzaSy...(実際のAPIキー)",
        "OUTPUT_IMAGE_PATH": "/Users/ユーザー名/Pictures/ai_generated"
      }
    }
  }
}
```
※ この方法は `args` 指定が不要でより簡潔です

### 🚨 重要事項
1. **絶対パス使用**: すべてのパスは完全パスで入力
2. **API キー置換**: `ここに実際のAPIキーを入力` 部分を発行した実際のキーに置換
3. **画像フォルダ**: `OUTPUT_IMAGE_PATH` に指定したフォルダが事前に作成されている必要があります

### 画像保存フォルダ作成
```bash
mkdir -p ~/Pictures/ai_generated
```

## 🎯 ステップ4: 実行・テスト

1. **Claude Desktop 再起動**: 設定後完全に終了して再起動
2. **接続確認**: Claude Desktop で MCP サーバーが接続されたか確認
3. **テスト**: 「猫の絵を描いて」とリクエストしてみる

## 📖 使用方法

### 画像生成
```
東京タワーの可愛いイラストを描いて
```
<img width="1024" height="1024" alt="image_Cute kawaii_54eac0c0" src="https://github.com/user-attachments/assets/2f508099-d365-4d80-a47c-a9ecf0d217ff" />

### 画像変換（ファイルパス）
```
/Users/username/image.jpg この画像に虹を追加して
```

### 画像変換（アップロード）
画像を Claude にアップロード後:
```
背景をレインボーブリッジの夜景にしてくれ
```
<img width="1024" height="1024" alt="image_Transform th_8231327e" src="https://github.com/user-attachments/assets/a5e36031-ac4f-4369-8e9e-8a0b175fb6e8" />


## 🔧 トラブルシューティング

### サーバー接続失敗
1. **ログ確認**: Claude Desktop のログフォルダで `gemini-image-generator.log` を確認
2. **パス確認**: `claude_desktop_config.json` の Python パスが正確か確認
3. **権限確認**: 画像保存フォルダに書き込み権限があるか確認

### API キーエラー
1. **キー有効性**: Google AI Studio で API キーが有効化されているか確認
2. **引用符確認**: 設定ファイルで API キーが引用符で囲まれているか確認

### 手動テスト
```bash
cd ~/GeminiImageMCP
source venv/bin/activate
export GEMINI_API_KEY="実際のAPIキー"
export OUTPUT_IMAGE_PATH="~/Pictures/ai_generated"
python -m gemini_image_mcp.server
```

## 📊 提供ツール

### 1. `generate_image_from_text`
- **機能**: テキストプロンプトで新しい画像生成
- **入力**: 画像説明テキスト
- **出力**: 生成された画像（Claude チャット内表示 + ローカル保存）

### 2. `transform_image_from_file`
- **機能**: ファイルパスの画像をテキストプロンプトで変換
- **入力**: 画像ファイルパス、変換プロンプト
- **出力**: 変換された画像（Claude チャット内表示 + ローカル保存）

### 3. `transform_image_from_encoded`
- **機能**: Base64 エンコードされた画像をテキストプロンプトで変換
- **入力**: Base64 画像データ、変換プロンプト
- **出力**: 変換された画像（Claude チャット内表示 + ローカル保存）

## 📝 オリジナルからの相違点

この修正版は元のリポジトリの以下の問題を解決しました:

- ❌ **元の問題**: JSON シリアル化エラー (`invalid utf-8 sequence`)
- ❌ **元の問題**: MCP ツールがバイナリデータ返却により実行失敗
- ✅ **修正事項**: ファイルパス返却で安定的な動作
- ✅ **修正事項**: Claude Desktop で完璧に動作
- ✅ **修正事項**: 生成された画像を Claude チャット内で直接確認可能

## 🤝 貢献・お問い合わせ

- **元のリポジトリ**: [qhdrl12/mcp-server-gemini-image-generator](https://github.com/qhdrl12/mcp-server-gemini-image-generator)
- **修正版**: [sungmin-koo-ai/GeminiImageMCP](https://github.com/sungmin-koo-ai/GeminiImageMCP)
- **問題報告**: GitHub Issues タブで問題を報告

## 📄 ライセンス

MIT License - 元のプロジェクトと同じ

---

**ヒント**: 初回設定時はステップごとに進め、問題が発生した場合はまずログファイルを確認してください！ 🚀