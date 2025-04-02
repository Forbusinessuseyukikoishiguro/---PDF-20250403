# 執事チャットボット 技術解説書

## 目次
1. [システム概要](#システム概要)
2. [初期設定方法](#初期設定方法)
3. [実行方法](#実行方法)
4. [システムコード解説](#システムコード解説)
5. [トラブルシューティング](#トラブルシューティング)

## システム概要

このシステムは、優しく丁寧な執事として応対するチャットボットであり、PDFドキュメントの参照・分析機能を備えています。テキスト感情分析と大規模言語モデル（LLM）を組み合わせ、ユーザーの感情や質問内容に応じた適切な応答を生成します。特にPDF文書の解析、要約、質問応答機能に優れています。

### 主な機能
- 感情に応じた丁寧な応答
- PDFファイルの読み込みと参照
- ウェブページの参照
- PDF内容の要約・トピック抽出
- 特定キーワードの検索
- PDF内の議論・論点分析
- 自然な会話とコンテキスト維持

## 初期設定方法

### 環境要件
- Python 3.7以上
- OpenAI API アクセス権（API キー）

### ファイル構成
```
butler-chatbot/
│
├── butler_chatbot.py   # メインプログラムファイル
├── .env                # API キー設定ファイル
│
└── documents/          # （任意）PDFファイル格納フォルダ
    └── sample.pdf
```

### インストール手順

1. **リポジトリの準備**
   ```bash
   mkdir butler-chatbot
   cd butler-chatbot
   ```

2. **ソースコードの保存**  
   提供されたコードを `butler_chatbot.py` という名前で保存します。

3. **必要なライブラリのインストール**
   ```bash
   pip install openai python-dotenv requests beautifulsoup4 PyPDF2 langchain
   ```
   
   インストールされるライブラリ：
   - `openai`: OpenAI API クライアント
   - `python-dotenv`: 環境変数管理
   - `requests`: HTTP リクエスト
   - `beautifulsoup4`: HTML パース
   - `PyPDF2`: PDF 処理
   - `langchain`: テキスト分割・処理

4. **API キーの設定**

   a. OpenAI API キーを取得:
      - [OpenAI Platform](https://platform.openai.com/) にアクセス
      - アカウント作成・ログイン
      - API キーセクションで新しいシークレットキーを生成

   b. `.env` ファイルの作成:
      ```
      OPENAI_API_KEY=sk-あなたのAPIキーをここに記述
      ```
      
   > **注意**: API キーは秘密情報です。共有したりリポジトリに公開したりしないでください。

## 実行方法

### 基本的な実行

コマンドラインからプログラムを実行します：
```bash
python butler_chatbot.py
```

実行すると、チャットボットの起動メッセージと利用可能なコマンドが表示されます：
```
===== お嬢様の執事チャットボット - PDF分析強化版 =====
※会話を終了するには「終了」と入力してください

【PDF参照機能】
・PDFのロード: 「pdf ファイル名.pdf」または「pdf https://example.com/file.pdf」と入力
・Webページのロード: 「url https://example.com」と入力

【PDF分析コマンド】
・「要約」：PDFの内容を要約します
・「トピック」：主なトピックを抽出します
・「議論」：PDFの主な議論や論点を分析します
・「検索 キーワード」：特定の情報を検索します
・また、ロードしたPDFについて自由に質問することもできます
```

### 会話の始め方

プログラム起動後、「お嬢様: 」というプロンプトが表示されます。ここに質問や指示を入力して Enter キーを押します。

### 主要コマンド一覧

1. **PDF操作コマンド**:
   - `pdf ファイル名.pdf` - ローカルPDFファイルを読み込み
   - `pdf https://example.com/file.pdf` - オンラインPDFを読み込み
   - `url https://example.com` - ウェブページを読み込み

2. **PDF分析コマンド**:
   - `要約` - PDFの内容を要約
   - `トピック` - 主なトピックを抽出
   - `議論` - PDF内の議論や論点を分析
   - `検索 キーワード` - 特定キーワードに関する情報を検索

3. **一般的な会話**:
   - PDF読み込み後は通常の質問も可能（例：「このPDFの第3章について教えて」）
   - 感情表現を含む会話も可能（例：「今日はとても疲れたわ」）

4. **システム操作**:
   - `終了` - チャットを終了

### 使用例

```
お嬢様: こんにちは

執事: お嬢様、こんにちは。今日はいかがお過ごしでしょうか。何かお手伝いできることがございましたら、どうぞお申し付けくださいませ。

お嬢様: pdf 健康のすべて.pdf

執事: お嬢様、'健康のすべて.pdf' の内容をご参照できるよう準備いたしました。

以下のコマンドをお使いいただけます：
・「要約」：PDFの内容を要約します
・「トピック」：主なトピックを抽出します
・「議論」：PDFの主な議論や論点を分析します
・「検索 〇〇」：特定のキーワードに関する情報を検索します

また、具体的なご質問もお気軽にどうぞ。

お嬢様: 要約

執事: お嬢様、このPDFの内容を要約いたしますと：

このドキュメントは健康と幸福に関する様々な側面について論じており、生活習慣、疾病予防、精神的健康、社会的関係、充実した人生の追求の重要性を強調しています。主な内容は以下の通りです：

1. 健康的な生活習慣：バランスの取れた食事、定期的な運動、十分な睡眠、ストレス管理が健康維持に不可欠であることを説明しています。リラクゼーション技術やマインドフルネス瞑想を日常に取り入れることを推奨しています。

2. 疾病予防：予防接種、定期健康診断、衛生管理、手洗い、感染対策などの予防措置の重要性を強調しています。また、禁煙、適度な飲酒、バランスの取れた食事、定期的な運動などの生活習慣の改善が疾病リスクを減らすことを説明しています。

3. 精神的健康：ストレス管理、十分な休息、リラクゼーション、心のケアが精神的健康の向上に寄与することを述べています。精神的な問題に対する早期介入と適切な治療の重要性も強調しています。

4. 社会的関係：強い対人関係とサポートシステムがストレスを軽減し、感情的な安定をもたらすことを説明しています。コミュニティ活動やサポートグループへの積極的な参加、コミュニケーション能力の向上が社会的サポートネットワークを構築するために重要であることを述べています。

5. 充実した人生の追求：目標設定、継続的な自己改善、喜びや挑戦をもたらす活動への参加が健康に重要であることを強調しています。個人的な興味、趣味、学習機会、成長への取り組みが充実した人生につながるとしています。

全体として、このドキュメントは健康に対する全体的なアプローチを強調し、バランスの取れた充実した人生を送るためには身体的、精神的、社会的、個人的側面を包括的に考慮する必要があることを示しています。

より詳細な情報については、お気軽にお尋ねください。
```

## システムコード解説

システムは以下の主要コンポーネントから構成されています：

### 1. 初期化と設定

```python
# Load environment variables from .env file
load_dotenv()

# Butler response patterns (in Japanese)
butler_responses = { ... }

# Emotion keyword dictionary for faster searching
emotion_keywords = { ... }

# Response cache dictionary
response_cache = {}

# Conversation history (limited size)
conversation_history = []
MAX_HISTORY = 6  # Keep history limited for better performance

# Global model name setting
MODEL_NAME = "gpt-3.5-turbo"  # Use faster model

# Knowledge base to store document references
knowledge_base = {
    "documents": {},  # key: doc name/url, value: chunk list
    "last_document": None,  # last referenced document
    "metadata": {}  # additional information about documents
}
```

この部分は、システムの初期化と設定を行います：
- `.env` ファイルから環境変数を読み込み
- 定型応答パターンの定義
- 感情キーワード辞書の定義
- レスポンスキャッシュの初期化
- 会話履歴の管理設定
- LLMモデルの指定
- ドキュメント参照のための知識ベース初期化

### 2. PDF処理モジュール

```python
def enhanced_extract_text_from_pdf(pdf_path, is_url=False):
    """Extract text from PDF with improved structure preservation"""
    # ...

def split_text(text, chunk_size=1000, chunk_overlap=200):
    """Split text into appropriate sized chunks"""
    # ...
```

このモジュールは、PDFファイルのテキスト抽出とチャンク分割を担当します：
- PDFファイルからのテキスト抽出（ページ構造を保持）
- テキストを処理可能なサイズのチャンクに分割
- ローカルとURL両方のPDFソース対応
- PyPDF2ライブラリとLangchainのテキスト分割機能を活用

### 3. PDF分析モジュール

```python
def summarize_pdf_content(pdf_chunks, client):
    """Generate a summary of the PDF content in Japanese"""
    # ...

def identify_pdf_topics(pdf_chunks, client):
    """Identify the main topics covered in the PDF in Japanese"""
    # ...

def search_pdf_for_information(query, pdf_chunks, client):
    """Search for specific information in the PDF based on a query in Japanese"""
    # ...

def analyze_pdf_arguments(pdf_chunks, client):
    """Analyze the main arguments or discussion points in the PDF in Japanese"""
    # ...
```

このモジュールは、PDFの高度な分析機能を提供します：
- PDFコンテンツの要約生成
- PDFの主要トピック抽出
- キーワードベースの情報検索
- 論点・議論の分析
- すべての出力を日本語・執事口調で整形

### 4. 感情分析モジュール

```python
def is_emotional_input(input_text):
    """Detect if input contains emotional keywords"""
    # ...

def get_keyword_response(input_text):
    """Get response based on emotion keywords"""
    # ...

def get_gpt_emotional_response(input_text):
    """Get GPT-generated response for emotional inputs in Japanese"""
    # ...
```

このモジュールは、テキスト入力から感情を検出し適切な応答を生成します：
- キーワードベースの基本感情検出
- 定型感情応答の選択
- GPTを用いた高度な感情応答生成
- 応答キャッシュの活用

### 5. 質問応答モジュール

```python
def get_gpt_general_response(input_text):
    """Get GPT-generated response for general questions in Japanese"""
    # ...

def answer_question_with_context(question, chunks, client):
    """Answer questions considering document context in Japanese"""
    # ...
```

このモジュールは、一般的な会話と文脈を考慮した質問応答を処理します：
- 一般的な質問への応答生成
- ドキュメント文脈を考慮した質問応答
- 関連チャンクの検索と利用
- 会話履歴の管理

### 6. コマンド解釈とメイン処理

```python
def load_document(source, client, is_pdf=True, is_url=False):
    """Load document into knowledge base with enhanced capabilities"""
    # ...

def get_reference_qa_response(input_text, client):
    """Generate responses based on document references with enhanced features"""
    # ...

def get_butler_response(input_text):
    """Main function to generate butler responses with enhanced features"""
    # ...
```

このモジュールは、ユーザー入力を解釈しシステム全体を制御します：
- ドキュメントのロードと処理
- コマンドパターンの検出と処理
- 応答生成の統合制御
- 適切な処理モジュールへの振り分け

### 7. ユーザーインターフェース

```python
def chat_with_butler():
    """Main interface for the enhanced butler chatbot"""
    # ...

# Start the chatbot
if __name__ == "__main__":
    chat_with_butler()
```

このモジュールは、ユーザーとの対話インターフェースを提供します：
- 起動メッセージと説明の表示
- ユーザー入力の受け取り
- 処理状況のアニメーション表示
- エラー処理と会話の終了制御

## トラブルシューティング

### 一般的な問題と解決方法

1. **APIキー関連のエラー**
   - エラーメッセージ: `Failed to load API key`
   - 解決方法:
     - `.env` ファイルが正しい場所にあるか確認
     - APIキーが正しく設定されているか確認
     - APIキーの先頭に `sk-` が含まれているか確認

2. **ライブラリのインストール問題**
   - エラーメッセージ: `ModuleNotFoundError: No module named '...'`
   - 解決方法:
     - 必要なライブラリを再インストール: `pip install [ライブラリ名]`
     - 仮想環境が有効かを確認

3. **PDF読み込みエラー**
   - エラーメッセージ: `PDF processing error` または `PDF text extraction failed`
   - 解決方法:
     - PDFパスが正しいか確認 (相対パス vs 絶対パス)
     - PDFがパスワード保護されていないか確認
     - PDFがテキスト抽出可能なフォーマットか確認

4. **日本語文字化けの問題**
   - 症状: 日本語が文字化けして表示される
   - 解決方法:
     - ターミナル/コマンドプロンプトがUTF-8エンコーディングを使用しているか確認
     - `sys.stdout.reconfigure(encoding='utf-8')` を追加

5. **APIレート制限エラー**
   - エラーメッセージ: `RateLimitError`
   - 解決方法:
     - リクエスト頻度を下げる
     - API使用量上限を確認
     - 別のAPIキーを試す

6. **メモリ使用量の問題**
   - 症状: 大きなPDFを処理する際にプログラムが遅くなる/クラッシュする
   - 解決方法:
     - `chunk_size` パラメータを小さくする
     - 大きなPDFをより小さなセクションに分割する

### 高度なカスタマイズ

1. **GPTモデルの変更**
   - `MODEL_NAME = "gpt-3.5-turbo"` を変更
   - 例: `MODEL_NAME = "gpt-4"` // より高度な処理が可能になりますが、コストが増加します

2. **テキスト分割パラメータの調整**
   - `split_text` 関数のパラメータを調整
   - 例: `chunk_size=500, chunk_overlap=100` // より細かいチャンクになりますが、文脈理解が低下する可能性があります

3. **感情キーワードの追加**
   - `emotion_keywords` 辞書に新しいキーワードを追加
   - 例: `"happy": ["嬉しい", "幸せ", "ハッピー", "楽しい", "うれしい"]`

4. **執事の応答パターンのカスタマイズ**
   - `butler_responses` 辞書の値を変更
   - 例: より特定のキャラクター性を持たせる、よりフォーマルな口調にするなど

   