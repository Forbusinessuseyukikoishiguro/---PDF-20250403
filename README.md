# 執事チャットボットの実行方法とファイル構成

執事チャットボットを実行するための詳細な手順を説明します。ファイル名や構成も含めて、具体的に解説していきますね。

## ファイル構成

### 基本構成
以下のファイルを準備します：

1. `butler_chatbot.py` - メインのPythonプログラムファイル
2. `.env` - APIキーを保存する環境変数ファイル（VSCode使用時のみ）

## 実行方法

### Google Colabでの実行方法

1. **ファイル作成**:
   - Google Colabを開き、新しいノートブックを作成します
   - ファイル名を「Butler_Chatbot.ipynb」などわかりやすい名前に変更します
   - コード全体を1つのセルにコピー＆ペーストします

2. **APIキーの設定**:
   - 左側のサイドバーにある「🔑」アイコン（秘密）をクリック
   - 「新規」をクリック
   - 名前: `OPENAI_API_KEY`
   - 値: あなたのOpenAI APIキー
   - 「保存」をクリック

3. **実行**:
   - セルの左側にある実行ボタン（▶️）をクリックするか、`Shift+Enter`キーを押します
   - プログラムが実行され、チャットボットが起動します

### VSCodeでの実行方法

1. **ファイル作成**:
   - 新しいフォルダを作成（例: `butler-chatbot`）
   - このフォルダ内に `butler_chatbot.py` ファイルを作成し、コードを貼り付けます
   - 同じフォルダ内に `.env` ファイルを作成します

2. **`.env` ファイルの内容**:
   ```
   OPENAI_API_KEY=あなたのOpenAIのAPIキー
   ```

3. **ライブラリのインストール**:
   - コマンドプロンプトまたはターミナルを開きます
   - フォルダに移動します: `cd path/to/butler-chatbot`
   - 必要なライブラリをインストールします:
     ```
     pip install openai python-dotenv requests beautifulsoup4 PyPDF2 langchain
     ```

4. **実行**:
   - ターミナルから以下のコマンドを実行:
     ```
     python butler_chatbot.py
     ```
   - またはVSCode内で、ファイルを開いて右上の実行ボタンをクリックします

## PDF参照機能を使う場合の追加ファイル

PDF参照機能を使用する場合、PDFファイルをプロジェクトフォルダに配置してください：

1. VSCodeの場合:
   - PDFファイルをプロジェクトフォルダ（`butler-chatbot`）内に配置します
   - チャットボット内で参照する際は：`pdf ファイル名.pdf`

2. Google Colabの場合:
   - 左側のファイルアイコンをクリック
   - アップロードボタンをクリックしてPDFをアップロード
   - チャットボット内で参照する際は：`pdf ファイル名.pdf`

## フォルダ構造例（VSCode）

```
butler-chatbot/
│
├── butler_chatbot.py    # メインのプログラムファイル
├── .env                 # APIキーを保存する環境変数ファイル
│
├── documents/           # PDFなどのドキュメントを保存するフォルダ（任意）
│   ├── sample.pdf
│   └── reference.pdf
│
└── README.md            # 使い方の説明（任意）
```

## 実行例

以下は実際のコマンドラインでの実行例です：

```
$ cd butler-chatbot
$ python butler_chatbot.py

===== お嬢様の執事チャットボット =====
※会話を終了するには「終了」と入力してください

【新機能】
・PDFの参照: 「pdf ファイル名.pdf」または「pdf https://example.com/file.pdf」と入力
・Webページの参照: 「url https://example.com」と入力
・参照したドキュメントに関する質問ができます

API key loaded successfully.

お嬢様: こんにちは

執事: お嬢様、こんにちは。今日はいかがお過ごしでしょうか。何かお手伝いできることがございましたら、どうぞお申し付けくださいませ。
```

これで、執事チャットボットのセットアップと実行方法について、必要なファイル名と共にご理解いただけたかと思います。何か不明点があれば、お気軽にお尋ねください。

ーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーー
やさしい執事とお嬢様のチャットボット　強化版使い方

★実行コマンド
PS　\Desktop\butler-chatbot> py butler_chatbot.py


★環境
WSL　仮想なし　Windows


★PDF格納先
butler-chatbotディレクトリ内・documentsフォルダでテスト済みです。

★ターミナルで実行

ーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーー
★サンプルCMD
PS \butler-chatbot> py butler_chatbot.py
API key loaded successfully.
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

お嬢様: つかれた

執事: お嬢様、お疲れ様です。お体がお疲れているようで心苦しいです。ゆっくり休んでいただくことが大切です。何かお手伝いが必要でしたら、何でもお申し付けくださいませ。

お嬢様: pdf 　\butler-chatbot\健康のすべて.pdf

執事: お嬢様、p\butler-chatbot\健康のすべて.pdf' の内容をご参照できるよう準備いたしました。

以下のコマンドをお使いいただけます：
・「要約」：PDFの内容を要約します
・「トピック」：主なトピックを抽出します
・「議論」：PDFの主な議論や論点を分析します
・「検索 〇〇」：特定のキーワードに関する情報を検索します

また、具体的なご質問もお気軽にどうぞ。

お嬢様: 要約して

執事: お嬢様、このPDFの内容を要約いたしますと：

健康に関する文書の要約：

この文書は健康に関する重要なトピックについて述べられています。以下に各セクションの要点をまとめます。

1. 健康と生活習慣：
- 健康は生活において非常に重要であり、生活習慣が健康に与える影響は大きい。
- バランスの取れた食事、適度な運動、十分な睡眠、ストレス管理が健康に良い影響を与える。
- 健康と生活習慣の関係を理解し、バランスの取れた生活を送ることが健康への第一歩。

2. 健康と疾病予防：
- 健康と疾病予防は密接に関連しており、予防接種や定期健康チェックが重要。
- 適切な衛生状態の維持、生活習慣の見直し、定期的な健康スクリーニングが疾病予防に役立つ。
- 疾病予防は個人の責任であり、健康への意識と積極的な取り組みが不可欠。

3. 健康とメンタルウェルビーイング：
- 健康は身体的な側面だけでなく、メンタルウェルビーイングも含まれる。
- ストレス管理、適切な休息やリラックス、心のケアがメンタルウ

より詳細な情報については、お気軽にお尋ねください。

お嬢様: １００文字で要約できる

執事: お嬢様、このPDFの内容を要約いたしますと：

健康に関する文書の要約：

1. **健康と生活習慣**
   - 健康は生活において重要であり、生活習慣が健康に影響を与える。
   - バランスの取れた食事、適度な運動、十分な睡眠、ストレス管理が重要。
   - 健康と生活習慣の関係を理解し、バランスの取れた生活を送ることが健康への第一歩。

2. **健康と疾病予防**
   - 健康と疾病予防は密接に関連しており、予防接種や健康チェックが重要。
   - 衛生状態の維持、手洗い、生活習慣の見直しが疾病予防に役立つ。
   - 禁煙、適度な飲酒、バランスの取れた食事、適度な運動が疾病リスクを低減する。

3. **健康とメンタルウェルビーイング**
   - 健康は身体的だけでなくメンタル面も含む。
   - ストレス管理、適切な休息、心のケアが重要。
   - 自己ケアの時間を設けることや適切な治療がメンタルウェルビーイングに役立つ。

4. **健康と社会的関係**
   - 社会的関係は健康に重要であり、人間関係や支持システムがストレ

より詳細な情報については、お気軽にお尋ねください。



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

お嬢様: トピック　健康

執事: お嬢様、健康に関するトピックについてお話しいたしましょう。健康を維持するためには、バランスの取れた食事、適度な運動、良質な睡眠、ストレス管理が重要です。また、定期的な健康診断もお忘れなく。

食事については、野菜、果物、たんぱく質、炭水化物など、栄養バランスの

お嬢様: タンパク質

執事: お嬢様、タンパク質についてお話しいたします。タンパク質は、身体の細胞や組織を構成するたんぱく質であり、筋肉や髪の毛、皮膚などの材料として重要な役割を果たします。

タンパク質は、動物性食品（肉、魚、卵、乳製品）や植物性食品（大豆製品、ナッ


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

お嬢様: 議論

執事: お嬢様、議論は異なる意見や立場を持つ人々の間で行われることがありますが、大切なのは相手を尊重し、冷静な議論を心がけることです。議論の際には、相手の意見をよく聞き、自分の考えをわかりやすく説明することが重要です。また、感情的にならずに冷静に議論を進めること

お嬢様: pdf \butler-chatbot\健康のすべて.pdf

執事: お嬢様、'\butler-chatbot\健康のすべて.pdf' の内容をご参照できるよう準備いたしました。

以下のコマンドをお使いいただけます：
・「要約」：PDFの内容を要約します
・「トピック」：主なトピックを抽出します
・「議論」：PDFの主な議論や論点を分析します
・「検索 〇〇」：特定のキーワードに関する情報を検索します

また、具体的なご質問もお気軽にどうぞ。

お嬢様: 議論

執事: お嬢様、議論は異なる意見や立場を持つ人々の間で行われることがありますが、大切なのは相手を尊重し、冷静な議論を心がけることです。議論の際には、相手の意見をよく聞き、自分の考えをわかりやすく説明することが重要です。また、感情的にならずに冷静に議論を進めること

お嬢様: 書いてあることで睡眠時間について議論したい

執事: お嬢様、このPDF内の主な議論や論点を分析いたしました：

【主な議論、観点、論点の分析】

1. 健康と生活習慣
   - 主張: 生活習慣が健康に与える影響は大きい。
   - 裏付け: バランスの取れた食事、適度な運動、十分な睡眠、ストレス管理の重要性が述べられている。
   - 制限事項: 個々の生活環境や制約によって、生活習慣の改善が難しい場合もある。

2. 健康と疾病予防
   - 主張: 疾病予防は健康と密接に関連しており、生活習慣の見直しや定期的な健康チェックが重要。
   - 裏付け: 禁煙や適度な飲酒、定期的な健康スクリーニングの効果が述べられている。
   - 制限事項: 疾病の発症は個人の責任だけでなく、環境や遺伝要因も影響を与える。

3. 健康とメンタルウェルビーイング
   - 主張: メンタルヘルス

これらの点について、より詳しくご議論されたい場合は、お申し付けください。

お嬢様: メンタルヘルス

執事: お嬢様、メンタルヘルスについてお問い合わせいただきまして、誠にありがとうございます。提供された情報に基づきまして、メンタルヘルスの重要性が強調されております。メンタルヘルスの維持や向上は、幸福感や生活の質を高める上で不可欠な要素でございます。ストレスの適切な管理や適切な休息、リラックス、心のケアが、メンタルウェルビーイングに大きな影響を与えます。また、精神的な問題に早期に対応し


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

お嬢様: ねむい

執事: お嬢様、お疲れのようですね。お体がお疲れのようでしたら、お休みになられることをお勧めいたします。お休みになられて、しっかりとお体を休められることが大切です。お気をつけてお休みくださいませ

お嬢様: 検索　猫

執事: お嬢様、猫に関する情報をお探しでしょうか。猫は人々に親しまれ、愛される動物です。猫は独立心が強く、狩りの技術に長けています。また、猫は繊細な動物でもあり、環境の変化に敏感です。猫を飼育する際には、適切な食事や運動、獣医の定期的な診

お嬢様: 終了

執事：またのお呼び出しをお待ちしております、お嬢様。良き一日をお過ごしください。

\butler-chatbot> python butler_chatbot.py

ーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーー
★URL参照

Desktop\butler-chatbot> py butler_chatbot.py    
API key loaded successfully.
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

お嬢様: ねむい

執事: お嬢様、お疲れのようですね。お体がお疲れのようでしたら、お休みになられることをお勧めいたします。お休みになられて、しっかりとお体を休められることが大切です。お気をつけてお休みくださいませ

お嬢様: 検索　猫

執事: お嬢様、猫に関する情報をお探しでしょうか。猫は人々に親しまれ、愛される動物です。猫は独立心が強く、狩りの技術に長けています。また、猫は繊細な動物でもあり、環境の変化に敏感です。猫を飼育する際には、適切な食事や運動、獣医の定期的な診

お嬢様: 終了

執事：またのお呼び出しをお待ちしております、お嬢様。良き一日をお過ごしください。
PS C:\Users\yukik\Desktop\butler-chatbot> python butler_chatbot.py^C
PS C:\Users\yukik\Desktop\butler-chatbot> py butler_chatbot.py
API key loaded successfully.
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

お嬢様: url https://ebetsu-sumikae.info/archives/8299

執事: お嬢様、'https://ebetsu-sumikae.info/archives/8299' の内容をご参照できるよう準備いたしました。

以下のコマンドをお使いいただけます：
・「要約」：PDFの内容を要約します
・「トピック」：主なトピックを抽出します
・「議論」：PDFの主な議論や論点を分析します
・「検索 〇〇」：特定のキーワードに関する情報を検索します

また、具体的なご質問もお気軽にどうぞ。

お嬢様: 要約

執事: お嬢様、このPDFの内容を要約いたしますと：

この文書は、北海道江別市のパン屋さんに関する情報を紹介しています。江別市は小麦「ハルユタカ」の生産地であり、多くの魅力的なパン屋が存在します。文書は18店舗のパン屋を個別に紹介し、各店の特徴やおすすめ商品、営業時間、定休日などを述べています。例えば、「ぱん屋ねこのさんぽ」ではねこパンやピスタチオクリーム入りの黒ねこパンが人気で、営業時間は8時から15時で水曜 と木曜が定休日です。他にも低糖質専門店やクロワッサン専門店など、様々な種類のパン屋が取り上げられています。また、文書の構造は各店舗の紹介が続き、最後に2025年の冬の江別での雪遊びや学校給食に関する情報が提示されています。江別市の魅力的なパン屋を巡る楽しさや地域の食文化に触れることができる内容となっています。

より詳細な情報については、お気軽にお尋ねください。

お嬢様: トピック

執事: お嬢様、このPDFで扱われている主なトピックは以下の通りでございます：

- 主なトピック:
  1. 江別市のパン屋さんの紹介
  2. パン屋さんの特徴やおすすめメニュー
  3. パン屋さんの営業時間や定休日
  4. パン屋さんの場所やアクセス方法
  5. 移住者インタビュー
  6. えべつの暮らしや子育て、働く環境に関する情報
  7. まちの話題や移住関連情報

- キーテーマ:
  - 江別市のパン屋さんの魅力
  - パン屋さんの多様なメニューと特徴
  - パン屋さんが地域とのコラボレーションを行う様子
  - 移住者の声や地域の魅力の紹介
  - えべつの暮らしや働く環境の魅力
  - まち

お嬢様: 助かった！

執事: お嬢様、何かお手伝いできることがありましたでしょうか？お役に立てたのであれば、光栄でございます。何か他にお困りのことがございましたら、お知らせくださいませ。

お嬢様: 終了

執事：またのお呼び出しをお待ちしております、お嬢様。良き一日をお過ごしください。

ーーーーーーーーーーーーーーーーーーーーーーーーー
aenum==3.1.15
aiofiles==23.2.1
aiohttp==3.9.5
aiosignal==1.3.1
allure-pytest==2.13.5
allure-python-commons==2.13.5
altgraph==0.17.4
annotated-types==0.7.0
anthropic==0.42.0
anyio==3.7.1
asgiref==3.8.1
attrs==23.2.0
Automat==24.8.1
autopep8==2.3.1
axe-selenium-python==2.1.6
babel==2.16.0
backoff==2.2.1
beautifulsoup4==4.12.3
black==24.10.0
blinker==1.8.2
boto3==1.36.2
botocore==1.36.2
browser-use==0.1.25
bs4==0.0.2
cachelib==0.13.0
cachetools==5.5.0
certifi==2024.6.2
cffi==1.16.0
charset-normalizer==3.4.1
click==8.1.7
colorama==0.4.6
constantly==23.10.4
contourpy==1.2.1
courlan==1.3.2
cryptography==43.0.1
cssselect==1.2.0
cycler==0.12.1
dateparser==1.2.0
decorator==5.1.1
defusedxml==0.7.1
Deprecated==1.2.14
distro==1.9.0
Django==4.2.16
et-xmlfile==1.1.0
execnet==2.1.1
Faker==23.3.0
fastapi==0.115.6
ffmpy==0.5.0
filelock==3.15.4
filetype==1.2.0
fireworks-ai==0.15.11
Flask==2.2.5
Flask-Session==0.8.0
Flask-SQLAlchemy==3.1.1
Flask-WTF==1.2.1
fonttools==4.53.1
frozenlist==1.4.1
fsspec==2024.12.0
future==1.0.0
google-ai-generativelanguage==0.6.10
google-api-core==2.24.0
google-api-python-client==2.159.0
google-auth==2.37.0
google-auth-httplib2==0.2.0
google-generativeai==0.8.3
googleapis-common-protos==1.66.0
googlemaps==4.10.0
gradio==5.9.1
gradio_client==1.5.2
greenlet==3.1.1
grpcio==1.69.0
grpcio-status==1.69.0
gunicorn==23.0.0
h11==0.14.0
html2text==2024.2.26
htmldate==1.9.3
httpcore==1.0.7
httplib2==0.22.0
httpx==0.27.2
httpx-sse==0.4.0
httpx-ws==0.7.1
huggingface-hub==0.27.1
hyperlink==21.0.0
idna==3.7
importlib_metadata==8.5.0
incremental==24.7.2
iniconfig==2.0.0
itemadapter==0.9.0
itemloaders==1.3.1
itsdangerous==2.2.0
Jinja2==3.1.4
jiter==0.8.2
jmespath==1.0.1
jsonpatch==1.33
jsonpointer==3.0.0
jusText==3.0.1
kiwisolver==1.4.5
langchain==0.3.14
langchain-anthropic==0.3.3
langchain-aws==0.2.11
langchain-core==0.3.30
langchain-fireworks==0.2.6
langchain-google-genai==2.0.8
langchain-ollama==0.2.2
langchain-openai==0.3.1
langchain-text-splitters==0.3.5
langsmith==0.2.10
line-bot-sdk==3.11.0
lmnr==0.4.53
lxml==5.3.0
lxml_html_clean==0.4.1
MainContentExtractor==0.0.4
markdown-it-py==3.0.0
MarkupSafe==2.1.5
matplotlib==3.9.1
mdurl==0.1.2
monotonic==1.6
MouseInfo==0.1.3
msgspec==0.18.6
multidict==6.0.5
mypy-extensions==1.0.0
numpy==1.26.4
ollama==0.4.6
openai==1.3.5
openpyxl==3.1.5
opentelemetry-api==1.29.0
opentelemetry-exporter-otlp-proto-common==1.29.0
opentelemetry-exporter-otlp-proto-grpc==1.29.0
opentelemetry-exporter-otlp-proto-http==1.29.0
opentelemetry-instrumentation==0.50b0
opentelemetry-instrumentation-langchain==0.36.1
opentelemetry-instrumentation-requests==0.50b0
opentelemetry-instrumentation-sqlalchemy==0.50b0
opentelemetry-instrumentation-threading==0.50b0
opentelemetry-instrumentation-urllib3==0.50b0
opentelemetry-proto==1.29.0
opentelemetry-sdk==1.29.0
opentelemetry-semantic-conventions==0.50b0
opentelemetry-semantic-conventions-ai==0.4.2
opentelemetry-util-http==0.50b0
orjson==3.10.14
outcome==1.3.0.post0
packaging==24.1
pandas==2.2.2
parsel==1.9.1
pathspec==0.12.1
pdf2image==1.17.0
pdfminer.six==20231228
pdfplumber==0.11.4
pefile==2023.2.7
pillow==10.4.0
platformdirs==4.3.6
playwright==1.49.1
pluggy==1.5.0
posthog==3.8.3
Protego==0.3.1
proto-plus==1.25.0
protobuf==5.29.3
py==1.11.0
pyasn1==0.6.0
pyasn1_modules==0.4.0
PyAutoGUI==0.9.54
pycodestyle==2.12.1
pycparser==2.22
pydantic==2.10.5
pydantic_core==2.27.2
PyDispatcher==2.0.7
pydub==0.25.1
pyee==12.0.0
pygame==2.6.0
pygeocoder==1.2.5
PyGetWindow==0.0.9
Pygments==2.18.0
pyinstaller==6.9.0
pyinstaller-hooks-contrib==2024.7
pyleniumio==1.21.0
PyMsgBox==1.0.9
PyMuPDF==1.24.13
pynput==1.7.7
pyOpenSSL==24.2.1
pyparsing==3.1.2
PyPDF2==3.0.1
pypdfium2==4.30.0
pyperclip==1.9.0
PyQt6==6.8.0
PyQt6-Qt6==6.8.1
PyQt6_sip==13.9.1
PyRect==0.2.0
PyScreeze==0.1.30
PySocks==1.7.1
pytesseract==0.3.10
pytest==8.3.3
pytest-xdist==3.6.1
python-dateutil==2.9.0.post0
python-dotenv==1.0.1
python-multipart==0.0.20
pytweening==1.2.0
pytz==2024.1
pywin32==308
pywin32-ctypes==0.2.2
PyYAML==6.0.2
queuelib==1.7.0
regex==2024.11.6
requests==2.32.3
requests-file==2.1.0
requests-toolbelt==1.0.0
retry==0.9.2
rich==13.9.4
rsa==4.9
ruff==0.9.1
s3transfer==0.11.1
safehttpx==0.1.6
Scrapy==2.11.2
selenium==4.22.0
semantic-version==2.10.0
service-identity==24.1.0
shellingham==1.5.4
six==1.16.0
sniffio==1.3.1
sortedcontainers==2.4.0
soupsieve==2.5
SQLAlchemy==2.0.34
sqlparse==0.5.1
starlette==0.41.3
tenacity==9.0.0
tiktoken==0.8.0
tld==0.13
tldextract==5.1.2
tomlkit==0.13.2
tqdm==4.67.1
trafilatura==2.0.0
trio==0.25.1
trio-websocket==0.11.1
Twisted==24.7.0
typer==0.15.1
typing_extensions==4.12.2
tzdata==2024.1
tzlocal==5.2
uritemplate==4.1.1
urllib3==2.2.2
uv==0.5.18
uvicorn==0.30.6
w3lib==2.2.1
webdriver-manager==4.0.2
websocket-client==1.8.0
websockets==14.1
Werkzeug==3.0.3
wrapt==1.16.0
wsproto==1.2.0
WTForms==3.1.2
XlsxWriter==3.2.0
yarl==1.9.4
zipp==3.21.0
zope.interface==7.0.3

ーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーー
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

-------------------------------------------------------------------------------------------------






# ---PDF-20250403
やさしい執事とお嬢様チャットボット完全版 - PDF分析機能強化版20250403
`.env`ファイルの作成方法と開発バージョン管理についての詳細なガイドを作成しました。このガイドでは以下の内容を詳しく解説しています：

## .envファイルの作成方法

- **`.env`ファイルとは何か**：環境変数管理の基本概念
- **作成手順**：テキストエディタでの作成から保存までのステップバイステップ
- **ファイル内容の書き方**：正しい構文とフォーマット
- **注意点**：セキュリティ面での考慮事項
- **読み込み確認方法**：ファイルが正しく読み込まれているかのテスト方法

## 開発バージョン情報の管理

- **開発環境バージョン**：推奨されるPythonバージョンと各ライブラリのバージョン
- **バージョン確認方法**：コマンドラインやプログラム内での確認手順
- **requirements.txtファイルの作成と使用**：依存関係管理の方法
- **バージョン互換性の注意点**：特に注意が必要なライブラリの互換性情報
- **バージョン管理のベストプラクティス**：仮想環境の使用や依存関係の固定方法

また、サンプルの`.env`ファイルも含めて、実際の読み込み方法までコード例で示しています。

このガイドを参考にすれば、執事チャットボットの環境設定を適切に行い、バージョン管理も効率的に実施できるようになります。何か追加の質問や詳細が必要な場合は、お気軽にお尋ねください。
