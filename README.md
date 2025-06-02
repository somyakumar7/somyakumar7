<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🧠 Philosopher's Wisdom Chatbot</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #8c82fc;
            --secondary: #ffb6b9;
            --accent: #41e0fd;
            --text-dark: #22223b;
            --text-light: #f8f9fa;
            --glass-bg: rgba(255,255,255,0.65);
            --glass-bg-dark: rgba(38,40,55,0.75);
            --shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.13);
            --blur: 18px;
            --gradient-header: linear-gradient(130deg, #8c82fc 10%, #ffb6b9 90%);
            --gradient-bot: linear-gradient(130deg, #41e0fd 10%, #8c82fc 90%);
            --gradient-user: linear-gradient(135deg, #ffb6b9 0%, #8c82fc 100%);
            --bubble-radius: 22px;
        }
        [data-theme="dark"] {
            --primary: #312e81;
            --secondary: #f472b6;
            --accent: #67e8f9;
            --text-dark: #f8f9fa;
            --text-light: #18181b;
            --glass-bg: rgba(38,40,55,0.85);
            --glass-bg-dark: rgba(18,20,30,0.95);
            --gradient-header: linear-gradient(130deg, #312e81 10%, #f472b6 90%);
            --gradient-bot: linear-gradient(130deg, #67e8f9 10%, #312e81 90%);
            --gradient-user: linear-gradient(135deg, #f472b6 0%, #312e81 100%);
        }

        html, body {
            min-height: 100vh;
            background: linear-gradient(135deg, #daf6ff 0%, #f5dff7 100%);
            transition: background 0.7s;
        }
        [data-theme="dark"] body {
            background: linear-gradient(135deg, #232946 0%, #353559 100%);
        }

        body {
            font-family: 'Inter', sans-serif;
            color: var(--text-dark);
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
        }

        .chat-container {
            width: 100%;
            max-width: 900px;
            height: 90vh;
            background: var(--glass-bg);
            border-radius: 32px;
            box-shadow: var(--shadow);
            backdrop-filter: blur(var(--blur));
            display: flex;
            flex-direction: column;
            overflow: hidden;
            position: relative;
            border: 1.5px solid rgba(140,130,252,0.15);
        }

        .theme-toggle {
            position: absolute;
            right: 30px;
            top: 25px;
            cursor: pointer;
            z-index: 10;
            border: none;
            background: none;
            font-size: 1.3rem;
            color: #fff;
            opacity: 0.7;
            transition: opacity 0.2s;
        }
        .theme-toggle:hover { opacity: 1; }

        .chat-header {
            background: var(--gradient-header);
            color: #fff;
            padding: 28px 24px 18px 24px;
            text-align: center;
            position: relative;
            z-index: 2;
            box-shadow: 0 2px 14px 0 rgba(140,130,252,0.08);
        }
        .chat-header h1 {
            font-size: 2.1rem;
            font-weight: 700;
            letter-spacing: -0.5px;
            margin-bottom: 7px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            animation: headerPulse 3s infinite;
        }
        @keyframes headerPulse {
            0%,100% { filter: brightness(1);}
            50% { filter: brightness(1.15);}
        }
        .chat-header p {
            opacity: 0.95;
            font-size: 1.05rem;
            font-weight: 400;
            max-width: 90%;
            margin: 0 auto;
        }

        .chat-messages {
            flex: 1;
            padding: 28px 28px 18px 28px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 24px;
            background: transparent;
            position: relative;
            z-index: 1;
        }

        .message {
            display: flex;
            align-items: flex-end;
            gap: 18px;
            opacity: 0;
            transform: translateY(20px);
            animation: slideIn 0.45s cubic-bezier(.4,1.7,.65,.82) forwards;
        }
        .message.user { flex-direction: row-reverse; }
        .message.user .message-avatar { background: var(--gradient-user); color: #fff;}
        .message.bot .message-avatar { background: var(--gradient-bot); color: #fff;}
        .message-avatar {
            width: 48px; height: 48px;
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            font-size: 1.7rem; font-weight: 600;
            box-shadow: 0 2px 10px 0 rgba(140,130,252,0.12);
            border: 2px solid #fff;
        }
        .message-content {
            max-width: 72vw;
            padding: 18px 25px;
            border-radius: var(--bubble-radius);
            background: var(--glass-bg);
            font-size: 1.08rem;
            color: var(--text-dark);
            box-shadow: 0 2px 10px 0 rgba(140,130,252,0.06);
            backdrop-filter: blur(3px);
            transition: box-shadow 0.2s, background 0.2s;
            word-break: break-word;
            border-bottom-right-radius: 8px;
        }
        .user .message-content {
            background: var(--gradient-user);
            color: #fff;
            border-bottom-right-radius: 6px;
        }
        .bot .message-content {
            background: var(--glass-bg-dark);
            color: var(--text-light);
            border-bottom-left-radius: 6px;
        }
        .philosopher-name {
            font-weight: 600;
            color: var(--accent);
            font-size: 0.92rem;
            margin-top: 11px;
            text-align: right;
            font-style: italic;
            opacity: 0.9;
        }
        .philosopher-name::before { content: "— "; }
        .quote-text {
            font-style: italic;
            position: relative;
            padding: 0 15px;
            font-weight: 500;
            line-height: 1.55;
            text-shadow: 0 1px 8px rgba(65,224,253,0.04);
        }
        .quote-text::before,
        .quote-text::after {
            color: var(--accent);
            font-family: Georgia, serif;
            opacity: 0.18;
            font-size: 3rem;
            position: absolute;
        }
        .quote-text::before {
            content: '"';
            left: -14px; top: -19px;
        }
        .quote-text::after {
            content: '"';
            right: -13px; bottom: -31px;
        }
        .chat-input-container {
            padding: 18px 24px;
            background: var(--glass-bg);
            border-top: 1.5px solid rgba(140,130,252,0.10);
            display: flex;
            gap: 16px;
            align-items: center;
            position: relative;
            z-index: 2;
            box-shadow: 0 -2px 14px 0 rgba(140,130,252,0.04);
        }
        .chat-input {
            flex: 1;
            padding: 16px 24px;
            border: 2px solid #e2e8f0;
            border-radius: 40px;
            font-size: 1.07rem;
            outline: none;
            background: var(--glass-bg);
            color: var(--text-dark);
            font-family: 'Inter', sans-serif;
            transition: border 0.3s, box-shadow 0.3s;
            box-shadow: 0 1px 3px 0 rgba(140,130,252,0.06);
        }
        .chat-input:focus {
            border-color: var(--accent);
            box-shadow: 0 0 0 4px rgba(65,224,253, 0.10);
        }
        .send-button {
            width: 56px;
            height: 56px;
            border: none;
            border-radius: 50%;
            background: var(--gradient-header);
            color: white;
            font-size: 1.4rem;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            display: flex; align-items: center; justify-content: center;
            box-shadow: 0 4px 12px -3px rgba(140,130,252,0.18);
            outline: none;
            border: 1.5px solid #fff;
        }
        .send-button:hover {
            transform: scale(1.05) translateY(-2px);
            box-shadow: 0 6px 18px 0 rgba(65,224,253,0.17);
            filter: brightness(1.08);
        }
        .typing-indicator {
            display: none;
            align-items: center;
            gap: 14px;
            margin: 7px 0;
            padding-left: 12px;
        }
        .typing-indicator.show { display: flex; }
        .typing-dots {
            display: flex;
            gap: 5px;
        }
        .typing-dot {
            width: 10px; height: 10px;
            border-radius: 50%;
            background: var(--accent);
            animation: typing 1.4s infinite cubic-bezier(.68,-0.55,.27,1.55);
            opacity: 0.7;
        }
        .typing-dot:nth-child(1) { animation-delay: -0.29s; }
        .typing-dot:nth-child(2) { animation-delay: -0.17s; }
        .welcome-message {
            text-align: center;
            color: var(--text-dark);
            padding: 45px 20px 38px 20px;
            font-style: italic;
            animation: fadeIn 1.1s ease-out;
        }
        .welcome-message h3 {
            color: var(--primary);
            margin-bottom: 14px;
            font-size: 1.6rem;
            font-weight: 700;
        }
        .welcome-message p {
            line-height: 1.7;
            max-width: 600px;
            margin: 0 auto;
        }
        .floating-philosophy {
            position: absolute;
            opacity: 0.03;
            font-size: 8rem;
            z-index: 0;
            pointer-events: none;
            animation: float 15s infinite ease-in-out;
            user-select: none;
        }
        .floating-1 { top: 21%; left: 9%; animation-delay: 0s; }
        .floating-2 { bottom: 15%; right: 10%; animation-delay: 2s; }
        @keyframes slideIn {
            to { opacity: 1; transform: translateY(0);}
        }
        @keyframes fadeIn {
            from { opacity: 0;}
            to { opacity: 1;}
        }
        @keyframes typing {
            0%, 80%, 100% { transform: scale(0.8); opacity: 0.5;}
            40% { transform: scale(1); opacity: 1;}
        }
        @keyframes float {
            0%, 100% { transform: translateY(0) rotate(0deg);}
            50% { transform: translateY(-24px) rotate(8deg);}
        }
        /* Scrollbar Styling */
        .chat-messages::-webkit-scrollbar {
            width: 8px;
        }
        .chat-messages::-webkit-scrollbar-thumb {
            background: #c3cfe2;
            border-radius: 4px;
        }
        .chat-messages::-webkit-scrollbar-thumb:hover {
            background: #a0aec0;
        }
        /* Responsive Design */
        @media (max-width: 768px) {
            .chat-container { height: 97vh; border-radius: 18px;}
            .chat-header h1 { font-size: 1.32rem;}
            .chat-header p { font-size: 0.94rem;}
            .chat-messages { padding: 15px 7px; gap: 18px;}
            .message-content { max-width: 95vw; padding: 13px 14px; font-size: 0.96rem;}
            .chat-input-container { padding: 12px;}
            .send-button { width: 48px; height: 48px; font-size: 1.17rem;}
            .message-avatar { width: 38px; height: 38px; font-size: 1.14rem;}
        }
        @media (max-width: 480px) {
            .chat-header { padding: 17px;}
            .chat-header h1 { font-size: 1rem;}
            .message-content { padding: 10px 9px;}
            .floating-philosophy { font-size: 5rem;}
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <button class="theme-toggle" id="themeToggle" title="Toggle dark/light mode">🌙</button>
        <div class="floating-philosophy floating-1">🤔</div>
        <div class="floating-philosophy floating-2">💭</div>
        <div class="chat-header">
            <h1><span class="emoji">🧠</span> Philosopher's Wisdom</h1>
            <p>Engage in thoughtful conversation and receive wisdom from history's greatest minds</p>
        </div>
        <div class="chat-messages" id="chatMessages">
            <div class="welcome-message">
                <h3>Welcome to Philosophical Dialogues</h3>
                <p>I am your guide to the wisdom of the ages. Ask me any question,<br> 
                and I shall respond with insights from history's most profound thinkers.</p>
            </div>
        </div>
        <div class="typing-indicator" id="typingIndicator">
            <div class="message-avatar">🤔</div>
            <div class="typing-dots">
                <div class="typing-dot"></div>
                <div class="typing-dot"></div>
                <div class="typing-dot"></div>
            </div>
            <span style="color: #718096; font-style: italic;">Philosopher is contemplating...</span>
        </div>
        <div class="chat-input-container">
            <input type="text" class="chat-input" id="chatInput" placeholder="Ask your philosophical question..." maxlength="500" autocomplete="off">
            <button class="send-button" id="sendButton" aria-label="Send">
                <svg xmlns="http://www.w3.org/2000/svg" width="25" height="25" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                    <line x1="22" y1="2" x2="11" y2="13"></line>
                    <polygon points="22 2 15 22 11 13 2 9 22 2"></polygon>
                </svg>
            </button>
        </div>
    </div>
    <script>
        // Theme toggler
        const themeToggle = document.getElementById('themeToggle');
        const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
        function setTheme(dark) {
            document.documentElement.setAttribute('data-theme', dark ? 'dark' : '');
            themeToggle.textContent = dark ? "☀️" : "🌙";
        }
        // Initial theme
        setTheme(localStorage.getItem('theme') === 'dark' || (!localStorage.getItem('theme') && prefersDark));
        themeToggle.onclick = () => {
            const isDark = document.documentElement.getAttribute('data-theme') === 'dark';
            setTheme(!isDark);
            localStorage.setItem('theme', !isDark ? 'dark' : 'light');
        };

        // Quotes data, same as before
        const philosopherQuotes = [/* ... (same as your list above) ... */];
        // For brevity, copy your existing philosopherQuotes array here

        // More expressive avatars map
        const avatarMap = {
            "Socrates": "🧓",
            "Aristotle": "🎓",
            "René Descartes": "🤔",
            "Jean-Paul Sartre": "🕶️",
            "Søren Kierkegaard": "📜",
            "Friedrich Nietzsche": "🦅",
            "Albert Camus": "🌄",
            "Buddha": "🧘",
            "Lao Tzu": "🌊",
            "Dalai Lama": "🙏",
            "Nelson Mandela": "🕊️",
            "Ralph Waldo Emerson": "🌲",
            "Martin Luther King Jr.": "✊",
            "Oscar Wilde": "🦚",
            "Brian Tracy": "💡",
            "Eleanor Roosevelt": "🌟",
            "Albert Einstein": "🧑‍🔬",
            "Winston Churchill": "🎩",
            "Theodore Roosevelt": "🐻",
            "Ludwig Wittgenstein": "📚",
            "Jean-Jacques Rousseau": "🌱",
            "Philosopher's Wisdom": "🧠"
        };

        const chatMessages = document.getElementById('chatMessages');
        const chatInput = document.getElementById('chatInput');
        const sendButton = document.getElementById('sendButton');
        const typingIndicator = document.getElementById('typingIndicator');

        function getRelevantQuote(userMessage) {
            const message = userMessage.toLowerCase();
            let matchedCategory = null;
            const categoryKeywords = {
                'knowledge': ['know', 'learn', 'wisdom', 'understand'],
                'happiness': ['happy', 'joy', 'content', 'fulfill'],
                'love': ['love', 'compassion', 'kindness', 'relationship'],
                'freedom': ['free', 'liberty', 'oppress', 'constrain'],
                'dreams': ['dream', 'aspire', 'goal', 'ambition'],
                'time': ['time', 'past', 'future', 'moment'],
                'self-knowledge': ['self', 'who am i', 'identity', 'know myself'],
                'hope': ['hope', 'light', 'dark', 'despair', 'optimism'],
                'perseverance': ['persevere', 'struggle', 'fall', 'fail', 'rise'],
                'authenticity': ['authentic', 'true', 'yourself', 'fake'],
                'mindset': ['mind', 'thought', 'think', 'become'],
                'resilience': ['resilience', 'strong', 'strength'],
                'excellence': ['excellence', 'quality', 'habit'],
                'opportunity': ['opportunity', 'difficulty', 'problem'],
                'communication': ['speak', 'talk', 'words', 'language', 'silence'],
                'innovation': ['innovation', 'trail', 'new', 'path'],
                'potential': ['potential', 'within', 'ability'],
                'beginnings': ['begin', 'start', 'step', 'journey'],
                'self-determination': ['destined', 'decide', 'choice'],
                'belief': ['believe', 'belief', 'confidence']
            };
            for (const [category, keywords] of Object.entries(categoryKeywords)) {
                if (keywords.some(keyword => message.includes(keyword))) {
                    matchedCategory = category; break;
                }
            }
            const filteredQuotes = matchedCategory ? philosopherQuotes.filter(q => q.category === matchedCategory) : philosopherQuotes;
            const randomIndex = Math.floor(Math.random() * filteredQuotes.length);
            return filteredQuotes[randomIndex];
        }

        function createMessage(content, isUser = false, quote = null) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${isUser ? 'user' : 'bot'}`;
            const avatar = document.createElement('div');
            avatar.className = 'message-avatar';
            if (isUser) {
                avatar.textContent = '👤';
            } else if (quote && avatarMap[quote.philosopher]) {
                avatar.textContent = avatarMap[quote.philosopher];
            } else {
                avatar.textContent = '🧠';
            }
            const messageContent = document.createElement('div');
            messageContent.className = 'message-content';
            if (isUser) {
                messageContent.textContent = content;
            } else {
                const quoteText = document.createElement('div');
                quoteText.className = 'quote-text';
                quoteText.textContent = quote.quote;
                const philosopherName = document.createElement('div');
                philosopherName.className = 'philosopher-name';
                philosopherName.textContent = quote.philosopher;
                messageContent.appendChild(quoteText);
                messageContent.appendChild(philosopherName);
            }
            messageDiv.appendChild(avatar);
            messageDiv.appendChild(messageContent);
            return messageDiv;
        }

        function addMessage(content, isUser = false, quote = null) {
            const message = createMessage(content, isUser, quote);
            chatMessages.appendChild(message);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }
        function showTypingIndicator() {
            typingIndicator.classList.add('show');
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }
        function hideTypingIndicator() {
            typingIndicator.classList.remove('show');
        }
        function sendMessage() {