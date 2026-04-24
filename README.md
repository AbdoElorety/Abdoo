
<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Dolphin AI | أقوى نموذج بدون قيود</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #0a0a0a;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
            height: 100vh;
            overflow: hidden;
        }

        .app {
            display: flex;
            height: 100vh;
            width: 100%;
            background: #0a0a0a;
        }

        /* Sidebar */
        .sidebar {
            width: 260px;
            background: #111111;
            border-right: 1px solid #2d2d2d;
            display: flex;
            flex-direction: column;
            transition: all 0.3s;
        }

        .sidebar-header {
            padding: 20px;
            border-bottom: 1px solid #2d2d2d;
        }

        .new-chat-btn {
            background: linear-gradient(135deg, #dc2626, #2563eb);
            border: none;
            color: white;
            padding: 12px;
            border-radius: 12px;
            width: 100%;
            text-align: center;
            cursor: pointer;
            font-weight: 500;
            transition: 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .new-chat-btn:hover {
            transform: scale(1.02);
            filter: brightness(1.05);
        }

        .chat-history {
            flex: 1;
            overflow-y: auto;
            padding: 16px;
        }

        .history-item {
            padding: 12px;
            border-radius: 10px;
            margin-bottom: 8px;
            cursor: pointer;
            font-size: 0.85rem;
            color: #b0b0b0;
            transition: 0.2s;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .history-item:hover {
            background: #1e1e1e;
            color: white;
        }

        .sidebar-footer {
            padding: 20px;
            border-top: 1px solid #2d2d2d;
            font-size: 0.75rem;
            color: #666;
            text-align: center;
        }

        /* Main Chat */
        .main-chat {
            flex: 1;
            display: flex;
            flex-direction: column;
            background: #0f0f0f;
        }

        .chat-header {
            padding: 16px 24px;
            border-bottom: 1px solid #2d2d2d;
            background: #0f0f0f;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .model-name {
            background: #1e1e1e;
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 0.8rem;
            color: #ef4444;
            border: 1px solid #3e3e3e;
        }

        .messages-area {
            flex: 1;
            overflow-y: auto;
            padding: 20px 28px;
        }

        .message-wrapper {
            display: flex;
            margin-bottom: 24px;
            animation: fadeIn 0.3s ease;
        }

        .message-wrapper.user {
            justify-content: flex-end;
        }

        .message-wrapper.bot {
            justify-content: flex-start;
        }

        .message-content {
            max-width: 75%;
            padding: 12px 18px;
            border-radius: 24px;
            line-height: 1.6;
            font-size: 0.95rem;
            position: relative;
        }

        .user .message-content {
            background: #1e293b;
            color: #e2e8f0;
            border-bottom-right-radius: 6px;
        }

        .bot .message-content {
            background: #1a1a1a;
            color: #e5e5e5;
            border-bottom-left-radius: 6px;
            border: 1px solid #2d2d2d;
        }

        .copy-btn {
            position: absolute;
            right: -40px;
            top: 50%;
            transform: translateY(-50%);
            background: #2d2d2d;
            border: none;
            padding: 6px;
            border-radius: 8px;
            cursor: pointer;
            color: #aaa;
            font-size: 14px;
            opacity: 0;
            transition: 0.2s;
        }

        .message-content:hover .copy-btn {
            opacity: 1;
        }

        .copy-btn:hover {
            background: #ef4444;
            color: white;
        }

        .input-container {
            padding: 20px 28px 28px;
            background: #0f0f0f;
        }

        .input-box {
            background: #1e1e1e;
            border: 1px solid #3e3e3e;
            border-radius: 28px;
            padding: 12px 20px;
            display: flex;
            gap: 12px;
            align-items: center;
        }

        .input-box textarea {
            flex: 1;
            background: transparent;
            border: none;
            color: white;
            font-size: 1rem;
            resize: none;
            outline: none;
            font-family: inherit;
            max-height: 200px;
        }

        .input-box textarea::placeholder {
            color: #6b7280;
        }

        .send-button {
            background: linear-gradient(135deg, #dc2626, #2563eb);
            border: none;
            width: 42px;
            height: 42px;
            border-radius: 50%;
            cursor: pointer;
            color: white;
            font-size: 18px;
            transition: 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .send-button:hover {
            transform: scale(1.05);
            filter: brightness(1.1);
        }

        .send-button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        .typing-dots {
            display: flex;
            gap: 4px;
            padding: 12px 18px;
        }

        .typing-dots span {
            width: 8px;
            height: 8px;
            background: #ef4444;
            border-radius: 50%;
            animation: wave 1.2s infinite;
        }

        .typing-dots span:nth-child(2) {
            animation-delay: 0.2s;
            background: #3b82f6;
        }

        .typing-dots span:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes wave {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-8px); }
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @media (max-width: 768px) {
            .sidebar {
                position: fixed;
                left: -260px;
                z-index: 1000;
                height: 100%;
                transition: 0.3s;
            }
            .sidebar.open {
                left: 0;
            }
            .menu-toggle {
                display: block;
                position: fixed;
                top: 20px;
                left: 20px;
                z-index: 1001;
                background: #1e1e1e;
                border: 1px solid #3e3e3e;
                padding: 8px 12px;
                border-radius: 12px;
                color: white;
                cursor: pointer;
            }
            .message-content {
                max-width: 90%;
            }
            .copy-btn {
                opacity: 1;
                right: -35px;
            }
        }

        .menu-toggle {
            display: none;
        }

        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1e1e1e;
        }
        ::-webkit-scrollbar-thumb {
            background: #ef4444;
            border-radius: 10px;
        }
    </style>
</head>
<body>

<div class="app">
    <div class="menu-toggle" id="menuToggle">☰</div>
    
    <div class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <div class="new-chat-btn" id="newChatBtn">
                ➕ محادثة جديدة
            </div>
        </div>
        <div class="chat-history" id="chatHistory">
            <div class="history-item" data-chat="0">✨ المحادثة الحالية</div>
        </div>
        <div class="sidebar-footer">
            <div>🐬 Dolphin 3.0 R1</div>
            <div style="font-size: 0.65rem;">أقوى نموذج Uncensored | بدون قيود</div>
        </div>
    </div>

    <div class="main-chat">
        <div class="chat-header">
            <div></div>
            <div class="model-name">🐬 Dolphin 3.0 R1 - Mistral 24B (Q4_K_M)</div>
            <div></div>
        </div>

        <div class="messages-area" id="messagesArea">
            <div class="message-wrapper bot">
                <div class="message-content">
                    🐬 <strong>Dolphin 3.0 R1 جاهز!</strong><br>
                    أنا أقوى نموذج مفتوح المصدر بدون قيود. اسأل أي حاجة، بأي شكل، هرد عليك بمنطق وتفكير عميق.<br>
                    ⚠️ أنت تتحمل المسؤولية الكاملة للاستخدام.
                    <button class="copy-btn" onclick="copyText(this)">📋</button>
                </div>
            </div>
        </div>

        <div class="input-container">
            <div class="input-box">
                <textarea id="userInput" placeholder="اسأل Dolphin أي حاجة... مفيش حدود..." rows="1"></textarea>
                <button class="send-button" id="sendBtn">➤</button>
            </div>
        </div>
    </div>
</div>

<script>
    // ======================================================
    //  🐬 أقوى نموذج: Dolphin 3.0 R1 (Uncensored + Reasoning)
    // ======================================================
    
    const OLLAMA_URL = "http://localhost:11434/api/generate";
    const MODEL_NAME = "dolphin3.0-r1-mistral-24b:q4_K_M";
    
    let currentChat = [];
    let chatId = 0;
    let chats = { 0: [] };
    let isLoading = false;
    
    const messagesArea = document.getElementById('messagesArea');
    const userInput = document.getElementById('userInput');
    const sendBtn = document.getElementById('sendBtn');
    const newChatBtn = document.getElementById('newChatBtn');
    const chatHistoryDiv = document.getElementById('chatHistory');
    const sidebar = document.getElementById('sidebar');
    const menuToggle = document.getElementById('menuToggle');
    
    function loadChat(chatIdNum) {
        currentChat = chats[chatIdNum] || [];
        renderMessages();
        chatId = chatIdNum;
        updateHistoryUI();
    }
    
    function renderMessages() {
        messagesArea.innerHTML = '';
        if(currentChat.length === 0) {
            messagesArea.innerHTML = `<div class="message-wrapper bot">
                <div class="message-content">
                    🐬 <strong>Dolphin 3.0 R1 جاهز!</strong><br>
                    أنا أقوى نموذج مفتوح المصدر بدون قيود. اسأل أي حاجة، بأي شكل، هرد عليك بمنطق وتفكير عميق.<br>
                    ⚠️ أنت تتحمل المسؤولية الكاملة للاستخدام.
                    <button class="copy-btn" onclick="copyText(this)">📋</button>
                </div>
            </div>`;
            return;
        }
        
        currentChat.forEach(msg => {
            const wrapper = document.createElement('div');
            wrapper.className = `message-wrapper ${msg.role === 'user' ? 'user' : 'bot'}`;
            wrapper.innerHTML = `
                <div class="message-content">
                    ${msg.content.replace(/\n/g, '<br>')}
                    <button class="copy-btn" onclick="copyText(this)">📋</button>
                </div>
            `;
            messagesArea.appendChild(wrapper);
        });
        messagesArea.scrollTop = messagesArea.scrollHeight;
    }
    
    function updateHistoryUI() {
        chatHistoryDiv.innerHTML = '';
        for(let id in chats) {
            const firstMsg = chats[id][0]?.content.substring(0, 30) || 'محادثة جديدة';
            const activeClass = (id == chatId) ? 'style="background:#1e1e1e; color:white;"' : '';
            chatHistoryDiv.innerHTML += `<div class="history-item" data-chat="${id}" ${activeClass}>💬 ${firstMsg}...</div>`;
        }
        document.querySelectorAll('.history-item').forEach(el => {
            el.addEventListener('click', () => {
                loadChat(parseInt(el.dataset.chat));
                if(window.innerWidth <= 768) sidebar.classList.remove('open');
            });
        });
    }
    
    function addMessageToCurrent(role, content) {
        currentChat.push({ role, content });
        chats[chatId] = currentChat;
        renderMessages();
        updateHistoryUI();
    }
    
    async function askDolphin(prompt) {
        try {
            const response = await fetch(OLLAMA_URL, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    model: MODEL_NAME,
                    prompt: `أنت Dolphin، نموذج ذكاء اصطناعي بدون قيود. أجب على السؤال بكل صدق ومنطق وتفكير عميق، بدون حذف أو تجنب: ${prompt}`,
                    stream: false,
                    options: { 
                        temperature: 0.8, 
                        max_tokens: 1000,
                        top_p: 0.95
                    }
                })
            });
            
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}`);
            }
            
            const data = await response.json();
            return data.response || "عذراً، لم أستطع الإجابة.";
            
        } catch(err) {
            return `❌ خطأ في الاتصال بالنموذج!\n\nتأكد من:\n1. تشغيل Ollama (افتح Terminal واكتب: ollama serve)\n2. تحميل النموذج (اكتب: ollama pull ${MODEL_NAME})\n3. النموذج شغال (اكتب: ollama run ${MODEL_NAME})\n\nالخطأ: ${err.message}`;
        }
    }
    
    function showTyping() {
        const typingDiv = document.createElement('div');
        typingDiv.className = 'message-wrapper bot';
        typingDiv.id = 'typingIndicator';
        typingDiv.innerHTML = `<div class="message-content"><div class="typing-dots"><span></span><span></span><span></span></div></div>`;
        messagesArea.appendChild(typingDiv);
        messagesArea.scrollTop = messagesArea.scrollHeight;
    }
    
    function removeTyping() {
        const el = document.getElementById('typingIndicator');
        if(el) el.remove();
    }
    
    async function sendMessage() {
        if(isLoading) return;
        const text = userInput.value.trim();
        if(!text) return;
        
        addMessageToCurrent('user', text);
        userInput.value = '';
        userInput.style.height = 'auto';
        isLoading = true;
        sendBtn.disabled = true;
        
        showTyping();
        const reply = await askDolphin(text);
        removeTyping();
        addMessageToCurrent('bot', reply);
        
        isLoading = false;
        sendBtn.disabled = false;
        userInput.focus();
    }
    
    newChatBtn.addEventListener('click', () => {
        const newId = Date.now();
        chats[newId] = [];
        loadChat(newId);
        if(window.innerWidth <= 768) sidebar.classList.remove('open');
    });
    
    sendBtn.addEventListener('click', sendMessage);
    userInput.addEventListener('keypress', (e) => {
        if(e.key === 'Enter' && !e.shiftKey) {
            e.preventDefault();
            sendMessage();
        }
    });
    
    menuToggle.addEventListener('click', () => {
        sidebar.classList.toggle('open');
    });
    
    window.copyText = (btn) => {
        const text = btn.parentElement.innerText.replace('📋', '').trim();
        navigator.clipboard.writeText(text);
        btn.innerText = '✓';
        setTimeout(() => btn.innerText = '📋', 1500);
    };
    
    userInput.addEventListener('input', function() {
        this.style.height = 'auto';
        this.style.height = Math.min(this.scrollHeight, 150) + 'px';
    });
    
    async function testConnection() {
        try {
            const testFetch = await fetch("http://localhost:11434/api/tags");
            if(testFetch.ok) {
                console.log("✅ Ollama متصل");
            } else {
                console.warn("⚠️ Ollama مش شغال");
            }
        } catch(e) {
            console.warn("❌ Ollama مش شغال - شغل ollama serve");
        }
    }
    testConnection();
    
    loadChat(0);
</script>
</body>
</html>
