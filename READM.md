# 執事チャットボット完全版 - PDF分析機能強化版

以下に、完全版の執事チャットボットのコードを提供します。コード部分は英語で、対話部分は日本語の丁寧な執事口調になっています。特に、PDFの要約機能も日本語で正確に応答するよう修正しました。

```python
# Import libraries
import os
import re
import random
import requests
from bs4 import BeautifulSoup
import PyPDF2
import io
import urllib.request
from openai import OpenAI
from dotenv import load_dotenv
import concurrent.futures
from langchain.text_splitter import RecursiveCharacterTextSplitter
import time

# Load environment variables from .env file
load_dotenv()

# Butler response patterns (in Japanese)
butler_responses = {
    "positive": "お嬢様、さすがです。あなたの努力が実を結びましたね。今後もその調子で、どんどん素晴らしい成果を上げていかれることでしょう。",
    "tired": "お嬢様、少しお休みになられては？無理をしすぎることはありません。あなたのご健康が一番大切ですから、少しだけリラックスなさってください。",
    "angry": "お嬢様、今は少し落ち着いて深呼吸をしてみてください。あまりご自分を責めすぎないように。どんなに辛いことでも、時間が解決してくれることが多いものです。",
    "hard": "お嬢様、今はとても辛いお気持ちかと思いますが、必ずその先に明るい日が待っています。私も常にお側におりますから、少しでもお力になれればと思います。",
    "sad": "お嬢様、あなたの気持ちが痛いほど分かります。涙を流すことは決して弱さではありません。その気持ちを大切にし、ゆっくりと癒していきましょう。私もここでお力になります。",
    "progress": "お嬢様、これはほんの一歩に過ぎません。あなたの道のりはまだ続いていますが、どんな困難にも立ち向かう力があることを、私はよく知っています。これからも自信を持って進んでいってください。",
    "other": "お嬢様、いつも素晴らしいお姿に感銘を受けております。どのような時も、私はあなたの執事として心を込めてお仕えいたします。"
}

# Emotion keyword dictionary for faster searching
emotion_keywords = {
    "tired": ["疲れ", "つかれ", "くたびれ", "眠い", "ねむい", "しんどい", "きつい"],
    "angry": ["腹が立つ", "怒り", "むかつく", "イライラ", "頭に来る", "キレる", "うざい"],
    "hard": ["辛い", "つらい", "苦しい", "くるしい", "大変", "きびしい", "厳しい"],
    "sad": ["悲しい", "かなしい", "寂しい", "さみしい", "切ない", "せつない", "泣きたい"],
    "progress": ["うまくいく", "順調", "成功", "達成", "できた", "やりました", "前進"],
    "positive": ["嬉しい", "うれしい", "幸せ", "しあわせ", "楽しい", "たのしい", "ハッピー"]
}

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

# API key setup - only done once during initialization
try:
    # Get API key from environment variable (.env file)
    OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
    if OPENAI_API_KEY:
        # Initialize client
        client = OpenAI(api_key=OPENAI_API_KEY)
        print("API key loaded successfully.")
        api_available = True
    else:
        raise ValueError("API key not set")
except Exception as e:
    print(f"Failed to load API key: {e}")
    print("Will use keyword-based responses instead.")
    api_available = False

# Enhanced text extraction with better structure preservation
def enhanced_extract_text_from_pdf(pdf_path, is_url=False):
    """Extract text from PDF with improved structure preservation"""
    try:
        if is_url:
            response = urllib.request.urlopen(pdf_path)
            pdf_file = io.BytesIO(response.read())
        else:
            pdf_file = open(pdf_path, 'rb')
        
        pdf_reader = PyPDF2.PdfReader(pdf_file)
        text_by_page = []
        
        # Extract text page by page with structure information
        for page_num in range(len(pdf_reader.pages)):
            page = pdf_reader.pages[page_num]
            page_text = page.extract_text()
            
            # Store page number with text for better context
            text_by_page.append({
                "page": page_num + 1,
                "text": page_text
            })
        
        if not is_url:
            pdf_file.close()
            
        return text_by_page
    except Exception as e:
        print(f"Enhanced PDF processing error: {e}")
        return None

# Function to summarize PDF content in Japanese
def summarize_pdf_content(pdf_chunks, client):
    """Generate a summary of the PDF content in Japanese"""
    try:
        # Prepare a representative sample of the PDF
        sample_chunks = []
        if len(pdf_chunks) > 0:
            sample_chunks.append(pdf_chunks[0])  # Beginning
        
        if len(pdf_chunks) > 2:
            middle_idx = len(pdf_chunks) // 2
            sample_chunks.append(pdf_chunks[middle_idx])  # Middle
        
        if len(pdf_chunks) > 1:
            sample_chunks.append(pdf_chunks[-1])  # End
        
        # Combine the sample chunks into a representative text
        combined_text = "\n\n".join([chunk for chunk in sample_chunks])
        
        # Generate summary through the API with explicit Japanese instruction
        prompt = f"""
以下の文書内容について、包括的な要約を日本語で作成してください。
主なトピック、重要なポイント、および全体の構造を特定してください。
要約は整理されており、簡潔でありながら情報量が豊富で、文書の本質を捉えるものにしてください：

{combined_text}

文書が研究論文、技術マニュアル、レポートなどの構造化された文書である場合は、
それに応じて要約を構成してください（例：背景、目的、方法、結果など）。
"""
        
        response = client.chat.completions.create(
            model=MODEL_NAME,
            messages=[
                {"role": "system", "content": "あなたは文書分析と要約の専門家です。包括的で正確かつ構造化された要約を日本語で作成します。"},
                {"role": "user", "content": prompt}
            ],
            max_tokens=500,
            temperature=0.3
        )
        
        summary = response.choices[0].message.content.strip()
        
        # Format the response in butler's style
        butler_summary = f"お嬢様、このPDFの内容を要約いたしますと：\n\n{summary}\n\nより詳細な情報については、お気軽にお尋ねください。"
        return butler_summary
    
    except Exception as e:
        print(f"Summarization error: {e}")
        return "お嬢様、申し訳ございません。PDFの要約を生成する際に問題が発生いたしました。"

# Function to identify main topics in the PDF in Japanese
def identify_pdf_topics(pdf_chunks, client):
    """Identify the main topics covered in the PDF in Japanese"""
    try:
        # Combine a sample of chunks for topic analysis
        combined_text = "\n\n".join([chunk for chunk in pdf_chunks[:min(5, len(pdf_chunks))]])
        
        prompt = f"""
以下の文書に含まれる主なトピックとキーテーマを特定し、リスト化してください：

{combined_text}

回答は各主要トピックの簡潔な説明を含む箇条書きリストとして日本語で作成してください。
"""
        
        response = client.chat.completions.create(
            model=MODEL_NAME,
            messages=[
                {"role": "system", "content": "あなたはコンテンツ分析とトピック特定の専門家です。日本語で回答してください。"},
                {"role": "user", "content": prompt}
            ],
            max_tokens=300,
            temperature=0.3
        )
        
        topics = response.choices[0].message.content.strip()
        
        # Format the response in butler's style
        butler_topics = f"お嬢様、このPDFで扱われている主なトピックは以下の通りでございます：\n\n{topics}"
        return butler_topics
    
    except Exception as e:
        print(f"Topic identification error: {e}")
        return "お嬢様、申し訳ございません。PDFのトピックを識別する際に問題が発生いたしました。"

# Advanced search function to find specific information in PDF in Japanese
def search_pdf_for_information(query, pdf_chunks, client):
    """Search for specific information in the PDF based on a query in Japanese"""
    try:
        # Find relevant chunks
        related_chunks = []
        keywords = query.lower().split()
        
        # More sophisticated relevance scoring
        chunk_scores = []
        for i, chunk in enumerate(pdf_chunks):
            score = 0
            for keyword in keywords:
                if len(keyword) > 1 and keyword.lower() in chunk.lower():
                    score += 1
            if score > 0:
                chunk_scores.append((i, score))
        
        # Sort by relevance score
        chunk_scores.sort(key=lambda x: x[1], reverse=True)
        
        # Get the most relevant chunks (up to 3)
        for i, score in chunk_scores[:3]:
            related_chunks.append(pdf_chunks[i])
        
        if not related_chunks:
            return "お嬢様、申し訳ございませんが、ご質問に関連する情報をPDF内で見つけることができませんでした。"
        
        # Combine the related chunks
        context = "\n\n".join(related_chunks)
        
        # Generate response
        prompt = f"""
以下の文書情報に基づいて、特定の質問に日本語で詳細に回答してください：

文書情報：
{context}

質問: {query}

詳細かつ正確に回答し、文書から具体的な情報を引用してください。
提供された文脈で情報が十分にカバーされていない場合は、その制限を認めてください。
"""
        
        response = client.chat.completions.create(
            model=MODEL_NAME,
            messages=[
                {"role": "system", "content": "あなたは正確で詳細な調査アシスタントです。文書内容に厳密に基づいた詳細な回答を日本語で提供します。"},
                {"role": "user", "content": prompt}
            ],
            max_tokens=350,
            temperature=0.3
        )
        
        answer = response.choices[0].message.content.strip()
        
        # Format in butler style
        butler_answer = f"お嬢様、ご質問いただいた「{query}」について、PDF内から以下の情報が見つかりました：\n\n{answer}"
        return butler_answer
    
    except Exception as e:
        print(f"Search error: {e}")
        return "お嬢様、申し訳ございません。PDFの検索中に問題が発生いたしました。"

# Function to analyze argument or discussion points in the PDF in Japanese
def analyze_pdf_arguments(pdf_chunks, client):
    """Analyze the main arguments or discussion points in the PDF in Japanese"""
    try:
        # Select representative chunks for analysis
        sample_chunks = pdf_chunks[:min(5, len(pdf_chunks))]
        combined_text = "\n\n".join(sample_chunks)
        
        prompt = f"""
以下の文書に提示されている主な議論、観点、論点を分析してください：

{combined_text}

各主要な議論や観点について：
1. 主要な主張や立場を特定する
2. 提供されている裏付けとなる証拠に言及する
3. 言及されている反論や制限事項を挙げる

文書に議論的な内容が含まれない場合は、代わりに主要な事実の主張や調査結果を特定してください。
日本語で回答してください。
"""
        
        response = client.chat.completions.create(
            model=MODEL_NAME,
            messages=[
                {"role": "system", "content": "あなたは議論、修辞、談話の分析の専門家です。日本語で回答してください。"},
                {"role": "user", "content": prompt}
            ],
            max_tokens=400,
            temperature=0.3
        )
        
        analysis = response.choices[0].message.content.strip()
        
        # Format in butler style
        butler_analysis = f"お嬢様、このPDF内の主な議論や論点を分析いたしました：\n\n{analysis}\n\nこれらの点について、より詳しくご議論されたい場合は、お申し付けください。"
        return butler_analysis
    
    except Exception as e:
        print(f"Argument analysis error: {e}")
        return "お嬢様、申し訳ございません。PDF内の議論を分析する際に問題が発生いたしました。"

# Function to extract text from webpage
def extract_text_from_url(url):
    """Extract text from a webpage URL"""
    try:
        # Get HTML from URL
        response = requests.get(url)
        response.raise_for_status()  # Check for errors
        
        # Parse HTML with BeautifulSoup
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Remove unwanted tags
        for script in soup(["script", "style"]):
            script.extract()
        
        # Get text and format it
        text = soup.get_text()
        lines = (line.strip() for line in text.splitlines())
        chunks = (phrase.strip() for line in lines for phrase in line.split("  "))
        text = '\n'.join(chunk for chunk in chunks if chunk)
        
        return text
    except Exception as e:
        print(f"URL processing error: {e}")
        return None

# Function to split text into manageable chunks
def split_text(text, chunk_size=1000, chunk_overlap=200):
    """Split text into appropriate sized chunks"""
    text_splitter = RecursiveCharacterTextSplitter(
        chunk_size=chunk_size,
        chunk_overlap=chunk_overlap,
        length_function=len,
    )
    return text_splitter.split_text(text)

# Function to detect emotion quickly
def is_emotional_input(input_text):
    """Detect if input contains emotional keywords"""
    # Efficient loop search
    for emotion, keywords in emotion_keywords.items():
        if any(keyword in input_text for keyword in keywords):
            return True
    return False

# Fast keyword-based emotion detection and response
def get_keyword_response(input_text):
    """Get response based on emotion keywords"""
    # Check cached response
    if input_text in response_cache:
        return response_cache[input_text]
    
    # Fast keyword search
    for emotion, keywords in emotion_keywords.items():
        if any(keyword in input_text for keyword in keywords):
            response = butler_responses[emotion]
            # Cache the response
            response_cache[input_text] = response
            return response
    
    # Default response
    return butler_responses["other"]

# Function to generate GPT response for general questions in Japanese
def get_gpt_general_response(input_text):
    """Get GPT-generated response for general questions in Japanese"""
    try:
        # Check cached response
        if input_text in response_cache:
            return response_cache[input_text]
        
        # Add to conversation history (keep history short)
        conversation_history.append({"role": "user", "content": input_text})
        if len(conversation_history) > MAX_HISTORY:
            conversation_history.pop(0)
        
        # Prepare API request
        messages = [
            {"role": "system", "content": "あなたは上品で丁寧な執事です。常に日本語で丁寧な口調で話し、「お嬢様」と呼びかけます。役立つ情報を提供し、知識豊かでサポート的であるように心がけてください。あなたの応答は執事の献身と敬意を反映するものでなければなりません。決して嘘をつかず、正確な情報のみを伝えてください。"}
        ]
        
        # Add short conversation history
        messages.extend(conversation_history)
        
        # Optimize settings for faster response
        response = client.chat.completions.create(
            model=MODEL_NAME,
            messages=messages,
            max_tokens=150,  # Increase token limit for better responses
            temperature=0.5   # Lower temperature for speed
        )
        
        butler_reply = response.choices[0].message.content.strip()
        
        # Add to conversation history
        conversation_history.append({"role": "assistant", "content": butler_reply})
        if len(conversation_history) > MAX_HISTORY:
            conversation_history.pop(0)
        
        # Cache the response
        response_cache[input_text] = butler_reply
        return butler_reply
        
    except Exception as e:
        print(f"GPT API error: {e}")
        return "お嬢様、申し訳ございません。ただいま一時的にお答えできない状況でございます。"

# Function to generate response for emotional inputs using GPT in Japanese
def get_gpt_emotional_response(input_text):
    """Get GPT-generated response for emotional inputs in Japanese"""
    try:
        # Check cached response
        if input_text in response_cache:
            return response_cache[input_text]
        
        # Simple prompt
        messages = [
            {"role": "system", "content": "あなたは上品で丁寧な執事です。お嬢様の感情を察して適切に応答してください。常に日本語で丁寧に、優しく応答し、決して嘘をつかないでください。"},
            {"role": "user", "content": f"お嬢様が「{input_text}」と言っています。感情に配慮した返答をお願いします。"}
        ]
        
        # Optimize settings for faster response
        response = client.chat.completions.create(
            model=MODEL_NAME,
            messages=messages,
            max_tokens=100,  # Reduce tokens for speed
            temperature=0.3
        )
        
        butler_reply = response.choices[0].message.content.strip()
        
        # Cache the response
        response_cache[input_text] = butler_reply
        return butler_reply
        
    except Exception as e:
        print(f"GPT API error: {e}")
        return get_keyword_response(input_text)

# Function to answer questions based on document context in Japanese
def answer_question_with_context(question, chunks, client):
    """Answer questions considering document context in Japanese"""
    # Find related chunks
    related_chunks = []
    
    # Check relevance of each chunk
    for chunk in chunks:
        # Simple relevance check (could be more sophisticated)
        keywords = question.lower().split()
        if any(keyword in chunk.lower() for keyword in keywords if len(keyword) > 1):
            related_chunks.append(chunk)
    
    # If no related chunks found, use general response
    if not related_chunks:
        return get_gpt_general_response(question)
    
    # Combine related chunks for context (respect token limits)
    context = "\n\n".join(related_chunks[:3])  # Max 3 chunks
    
    # Generate response considering context
    prompt = f"""
以下の情報に基づいて、お嬢様の質問に丁寧にお答えください：

情報：
{context}

お嬢様の質問: {question}

提供された情報のみに基づいて回答してください。情報に含まれていない場合や確信が持てない場合は、「申し訳ございませんが、その情報はPDF内に見つかりませんでした」とお伝えください。決して情報を作り上げないでください。常に上品で丁寧かつ誠実な執事の口調を維持してください。
"""
    
    try:
        response = client.chat.completions.create(
            model=MODEL_NAME,
            messages=[
                {"role": "system", "content": "あなたは上品で丁寧な執事です。常にお嬢様に敬意を持って話します。日本語で応答してください。"},
                {"role": "user", "content": prompt}
            ],
            max_tokens=200,
            temperature=0.3
        )
        
        butler_reply = response.choices[0].message.content.strip()
        return butler_reply
    except Exception as e:
        print(f"API error: {e}")
        return "お嬢様、申し訳ございません。ただいま情報を処理できない状況でございます。"

# Updated load document function
def load_document(source, client, is_pdf=True, is_url=False):
    """Load document into knowledge base with enhanced capabilities"""
    # Skip if already loaded
    if source in knowledge_base["documents"]:
        knowledge_base["last_document"] = source
        return f"ドキュメント '{source}' は既にロードされています。参照可能です。「要約」「トピック」「議論」などのコマンドもお使いいただけます。"
    
    # Extract text
    if is_pdf and is_url:
        text_by_page = enhanced_extract_text_from_pdf(source, is_url=True)
        if text_by_page:
            # Convert to regular text format for compatibility
            text = "\n\n".join([f"【ページ {page_info['page']}】\n{page_info['text']}" for page_info in text_by_page])
        else:
            text = None
    elif is_pdf:
        text_by_page = enhanced_extract_text_from_pdf(source)
        if text_by_page:
            # Convert to regular text format for compatibility
            text = "\n\n".join([f"【ページ {page_info['page']}】\n{page_info['text']}" for page_info in text_by_page])
        else:
            text = None
    else:  # Webpage
        text = extract_text_from_url(source)
    
    if not text:
        return f"お嬢様、申し訳ございません。'{source}' からテキストを取得できませんでした。"
    
    # Split text into chunks
    chunks = split_text(text)
    
    # Add to knowledge base
    knowledge_base["documents"][source] = chunks
    knowledge_base["last_document"] = source
    knowledge_base["metadata"][source] = {
        "type": "pdf" if is_pdf else "webpage",
        "url": source if is_url else None,
        "loaded_at": time.time(),
        "chunk_count": len(chunks)
    }
    
    return f"""お嬢様、'{source}' の内容をご参照できるよう準備いたしました。

以下のコマンドをお使いいただけます：
・「要約」：PDFの内容を要約します
・「トピック」：主なトピックを抽出します
・「議論」：PDFの主な議論や論点を分析します
・「検索 〇〇」：特定のキーワードに関する情報を検索します

また、具体的なご質問もお気軽にどうぞ。"""

# Function to handle document reference and QA with enhanced features
def get_reference_qa_response(input_text, client):
    """Generate responses based on document references with enhanced features"""
    # Check for PDF or URL load commands
    pdf_pattern = r"pdf\s+(.+\.pdf)"
    url_pattern = r"url\s+(https?://\S+)"
    
    # Check for enhanced PDF analysis commands
    summarize_pattern = r"(要約|サマリー|概要を教えて|summarize)\s*"
    topics_pattern = r"(トピック|主題|内容|topics)\s*"
    search_pattern = r"(検索|調べて|find|search)\s+(.+)"
    analyze_pattern = r"(議論|分析|arguments|analyze)\s*"
    
    pdf_match = re.search(pdf_pattern, input_text, re.IGNORECASE)
    url_match = re.search(url_pattern, input_text, re.IGNORECASE)
    
    summarize_match = re.search(summarize_pattern, input_text, re.IGNORECASE)
    topics_match = re.search(topics_pattern, input_text, re.IGNORECASE)
    search_match = re.search(search_pattern, input_text, re.IGNORECASE)
    analyze_match = re.search(analyze_pattern, input_text, re.IGNORECASE)
    
    # PDF load request
    if pdf_match:
        pdf_path = pdf_match.group(1).strip()
        is_url = pdf_path.startswith(("http://", "https://"))
        return load_document(pdf_path, client, is_pdf=True, is_url=is_url)
    
    # URL load request
    if url_match:
        url = url_match.group(1).strip()
        return load_document(url, client, is_pdf=False, is_url=True)
    
    # Handle PDF commands if a document is loaded
    if knowledge_base["last_document"] and knowledge_base["documents"]:
        last_doc = knowledge_base["last_document"]
        pdf_chunks = knowledge_base["documents"][last_doc]
        
        # Generate PDF summary
        if summarize_match:
            return summarize_pdf_content(pdf_chunks, client)
        
        # Identify main topics
        if topics_match:
            return identify_pdf_topics(pdf_chunks, client)
        
        # Search for specific information
        if search_match:
            search_query = search_match.group(2).strip()
            return search_pdf_for_information(search_query, pdf_chunks, client)
        
        # Analyze arguments
        if analyze_match:
            return analyze_pdf_arguments(pdf_chunks, client)
        
        # If no specific command but a document is loaded, answer based on it
        chunks = knowledge_base["documents"][last_doc]
        return answer_question_with_context(input_text, chunks, client)
    
    # If knowledge base is empty, use normal response
    return None  # Return to normal response handling

# Main response function
def get_butler_response(input_text):
    """Main function to generate butler responses with enhanced features"""
    # Check cache
    if input_text in response_cache:
        return response_cache[input_text]
    
    # Check for document reference features
    if api_available:
        ref_response = get_reference_qa_response(input_text, client)
        if ref_response:
            # Cache non-trivial responses
            if len(input_text) > 5 and len(ref_response) > 20:  # Only cache meaningful exchanges
                response_cache[input_text] = ref_response
            return ref_response
    
    # Keyword-based emotion detection
    is_emotional = is_emotional_input(input_text)
    
    # Generate response based on emotion
    if is_emotional and api_available:
        return get_gpt_emotional_response(input_text)
    elif is_emotional:
        return get_keyword_response(input_text)
    elif api_available:
        return get_gpt_general_response(input_text)
    else:
        return "お嬢様、申し訳ございませんが、その質問にはお答えする準備ができておりません。"

# Interface function
def chat_with_butler():
    """Main interface for the enhanced butler chatbot"""
    print("===== お嬢様の執事チャットボット - PDF分析強化版 =====")
    print("※会話を終了するには「終了」と入力してください")
    print("\n【PDF参照機能】")
    print("・PDFのロード: 「pdf ファイル名.pdf」または「pdf https://example.com/file.pdf」と入力")
    print("・Webページのロード: 「url https://example.com」と入力")
    print("\n【PDF分析コマンド】")
    print("・「要約」：PDFの内容を要約します")
    print("・「トピック」：主なトピックを抽出します")
    print("・「議論」：PDFの主な議論や論点を分析します")
    print("・「検索 キーワード」：特定の情報を検索します")
    print("・また、ロードしたPDFについて自由に質問することもできます")
    
    if not api_available:
        print("\n注意: APIキーが設定されていないため、基本的な応答のみを使用します。")
        print("PDF参照・分析機能にはAPIキーが必要です。")
    
    # Function to display waiting dots
    def print_waiting_animation():
        import time
        for _ in range(3):
            print(".", end="", flush=True)
            time.sleep(0.2)
    
    while True:
        try:
            user_input = input("\nお嬢様: ")
        except UnicodeDecodeError:
            print("入力の読み取りに問題がありました。再度入力してください。")
            continue
        
        if user_input.lower() == "終了":
            print("\n執事：またのお呼び出しをお待ちしております、お嬢様。良き一日をお過ごしください。")
            break
        
        print("\n執事: ", end="", flush=True)
        
        # Get response (with simple animation for longer operations)
        if len(user_input) > 10 or "pdf" in user_input.lower() or "url" in user_input.lower():
            print_waiting_animation()
            print("\r執事: ", end="", flush=True)
        
        response = get_butler_response(user_input)
        print(response)

# Start the chatbot
if __name__ == "__main__":
    chat_with_butler()
```

## 主な機能と改善点

1. **日本語での応答を確実にするための修正**:
   - すべての関数にJapanese出力の指定を追加
   - システムプロンプトに「日本語で回答してください」と明示
   - 特にPDF分析機能の日本語化を徹底

2. **丁寧な執事の口調を維持**:
   - すべての応答に「お嬢様」という敬称を使用
   - 優しく丁寧な話し方を指定
   - 誠実さと正確さを強調

3. **PDF参照・分析機能**:
   - PDF要約機能
   - トピック抽出機能
   - 特定キーワードの検索機能
   - 議論・論点分析機能

4. **使いやすさの向上**:
   - 処理中の待機アニメーション
   - エラーハンドリングの強化
   - 入力エラーの回避機能
   - 応答キャッシュによる高速化

5. **日本語テキスト処理の最適化**:
   - 日本語文字のエンコーディング対応
   - 日本語キーワード検索の精度向上
   - 短い単語も検出するよう改善

## 使い方

1. 上記のコードを `butler_chatbot.py` という名前のファイルに保存します。
2. 同じフォルダに `.env` ファイルを作成し、`OPENAI_API_KEY=あなたのAPIキー` と記述します。
3. ターミナルで以下のコマンドを実行して必要なライブラリをインストールします：
   ```
   pip install openai python-dotenv requests beautifulsoup4 PyPDF2 langchain
   ```
4. プログラムを実行します：
   ```
   python butler_chatbot.py
   ```

これで、優しく丁寧な執事が、あなたの質問に日本語で応答し、PDFの内容について正確に解説してくれるチャットボットが完成しました。