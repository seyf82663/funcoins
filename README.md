>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1.0" />
  <title>FunCoins Chat</title>
  <style>
    body { font-family: sans-serif; background: #f5f7fa; margin: 0; padding: 0; }
    header, footer { background: #6366F1; color: white; padding: 10px; text-align: center; }
    .container { max-width: 600px; margin: auto; padding: 10px; }
    .card { background: white; border-radius: 10px; padding: 15px; margin-bottom: 15px; }
    .btn { padding: 8px 12px; border: none; border-radius: 5px; cursor: pointer; }
    .btn-primary { background: #6366F1; color: white; }
    .btn-secondary { background: #ddd; }
    .chat-box { height: 200px; overflow-y: auto; border: 1px solid #ccc; padding: 5px; margin-bottom: 5px; }
    input { padding: 5px; border: 1px solid #ccc; border-radius: 5px; }
  </style>
</head>
<body>
  <header>
    <h1>💰 FunCoins Chat</h1>
    <small>نسخة تجريبية محلية</small>
  </header>

  <div class="container">
    <div class="card">
      <div>
        <strong>عملاتك:</strong> <span id="coins">0</span> 🪙
      </div>
      <button class="btn btn-secondary" onclick="login()">تسجيل دخول</button>
      <button class="btn btn-secondary" onclick="logout()">تسجيل خروج</button>
    </div>

    <div class="card">
      <h3>🎮 اللعبة: اضغط لتحصل على عملات</h3>
      <button class="btn btn-primary" onclick="playGame()">العب الآن</button>
      <div id="gameMsg"></div>
    </div>

    <div class="card">
      <h3>💬 غرفة الدردشة</h3>
      <div id="chat" class="chat-box"></div>
      <input type="text" id="msgInput" placeholder="اكتب رسالة..." />
      <button class="btn btn-primary" onclick="sendMsg()">إرسال</button>
    </div>
  </div>

  <footer>
    &copy; 2025 FunCoins Chat - نسخة تجريبية
  </footer>

  <script>
    let user = null;

    function login() {
      const name = prompt("اكتب اسمك:");
      if (name) {
        user = { name: name, coins: 100 };
        localStorage.setItem("fun_user", JSON.stringify(user));
        updateCoins();
      }
    }

    function logout() {
      user = null;
      localStorage.removeItem("fun_user");
      updateCoins();
    }

    function loadUser() {
      const saved = localStorage.getItem("fun_user");
      if (saved) user = JSON.parse(saved);
      updateCoins();
    }

    function updateCoins() {
      document.getElementById("coins").textContent = user ? user.coins : 0;
    }

    function playGame() {
      if (!user) return alert("سجّل دخول أولاً");
      const reward = Math.floor(Math.random() * 10) + 1;
      user.coins += reward;
      localStorage.setItem("fun_user", JSON.stringify(user));
      updateCoins();
      document.getElementById("gameMsg").textContent = `فزت بـ ${reward} عملة!`;
    }

    function sendMsg() {
      if (!user) return alert("سجّل دخول أولاً");
      const input = document.getElementById("msgInput");
      const txt = input.value.trim();
      if (!txt) return;
      const chat = JSON.parse(localStorage.getItem("fun_chat") || "[]");
      chat.push({ name: user.name, text: txt });
      localStorage.setItem("fun_chat", JSON.stringify(chat));
      input.value = "";
      renderChat();
    }

    function renderChat() {
      const chatBox = document.getElementById("chat");
      const chat = JSON.parse(localStorage.getItem("fun_chat") || "[]");
      chatBox.innerHTML = "";
      chat.forEach(m => {
        const div = document.createElement("div");
        div.textContent = `${m.name}: ${m.text}`;
        chatBox.appendChild(div);
      });
    }

    window.onload = () => {
      loadUser();
      renderChat();
    };

    window.addEventListener("storage", () => {
      renderChat();
      loadUser();
    });
  </script>
</body>
</html>
