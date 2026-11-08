<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI 圓桌會議</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div id="app">
    <!-- 設定 / 登入畫面 -->
    <div id="setup-screen" class="screen">
      <div class="card setup-card">
        <h1>AI 圓桌會議</h1>
        <p class="subtitle">讓多個 AI 模型針對同一個主題持續討論、互相交鋒與碰撞觀點。</p>
        
        <div id="password-group" class="form-group" style="display:none;">
          <label for="app-password">請輸入存取密碼：</label>
          <input type="password" id="app-password" placeholder="密碼">
        </div>

        <div class="form-group">
          <label for="topic-input">討論主題：</label>
          <textarea id="topic-input" rows="3" placeholder="例如：AI 是否會完全取代傳統軟體工程師？"></textarea>
        </div>

        <div class="form-group">
          <label>參與討論的 AI：</label>
          <div id="models-checklist" class="checkbox-grid"></div>
        </div>

        <button id="start-btn" class="primary-btn">開始討論</button>
      </div>
    </div>

    <!-- 討論進行中的主畫面 -->
    <div id="discussion-screen" class="screen" style="display:none;">
      <header class="top-bar">
        <div class="topic-info">
          <span class="badge" id="status-badge">進行中</span>
          <h2 id="current-topic-title">載入中...</h2>
        </div>
        <div class="controls">
          <button id="pause-resume-btn" class="secondary-btn">暫停</button>
          <button id="stop-btn" class="danger-btn">結束討論</button>
        </div>
      </header>

      <div class="main-layout">
        <!-- 左側：即時聊天記錄 -->
        <div class="chat-container">
          <div id="transcript" class="transcript"></div>
          <div class="input-bar">
            <input type="text" id="user-input" placeholder="輸入你想對 AI 說的話或插話（可用 @名字）...">
            <button id="send-btn" class="primary-btn">發送</button>
          </div>
        </div>

        <!-- 右側：側邊欄（成員與即時筆記） -->
        <div class="sidebar">
          <div class="panel">
            <h3>參與者</h3>
            <div id="participants-list" class="participants-list"></div>
          </div>
          <div class="panel notes-panel">
            <h3>即時討論筆記</h3>
            <div id="notes-content" class="notes-content">（討論進行中將自動整理共識與分歧...）</div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <script src="app.js"></script>
</body>
</html>

* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; background-color: #0d1117; color: #c9d1d9; height: 100vh; display: flex; flex-direction: column; }
.screen { width: 100%; height: 100%; display: flex; justify-content: center; align-items: center; }
.card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 32px; width: 100%; max-width: 600px; box-shadow: 0 4px 24px rgba(0,0,0,0.5); }
h1 { font-size: 24px; margin-bottom: 8px; color: #f0f6fc; }
.subtitle { font-size: 14px; color: #8b949e; margin-bottom: 24px; }
.form-group { margin-bottom: 20px; }
.form-group label { display: block; font-size: 14px; font-weight: 600; margin-bottom: 8px; color: #f0f6fc; }
input[type="text"], input[type="password"], textarea { width: 100%; padding: 10px 12px; background: #0d1117; border: 1px solid #30363d; border-radius: 6px; color: #c9d1d9; font-size: 14px; }
input:focus, textarea:focus { outline: none; border-color: #58a6ff; box-shadow: 0 0 0 3px rgba(88,166,255,0.3); }
.checkbox-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; }
.checkbox-item { display: flex; align-items: center; gap: 8px; background: #21262d; padding: 8px 12px; border-radius: 6px; border: 1px solid #30363d; cursor: pointer; }
.primary-btn { background: #238636; color: white; border: none; padding: 10px 16px; border-radius: 6px; font-weight: 600; cursor: pointer; width: 100%; }
.primary-btn:hover { background: #2ea043; }
.secondary-btn { background: #21262d; color: #c9d1d9; border: 1px solid #30363d; padding: 8px 12px; border-radius: 6px; cursor: pointer; }
.secondary-btn:hover { background: #30363d; }
.danger-btn { background: #da3633; color: white; border: none; padding: 8px 12px; border-radius: 6px; cursor: pointer; }
.danger-btn:hover { background: #f85149; }

/* 討論主畫面排版 */
#discussion-screen { flex-direction: column; align-items: stretch; padding: 16px; gap: 16px; }
.top-bar { display: flex; justify-content: space-between; align-items: center; background: #161b22; padding: 12px 20px; border: 1px solid #30363d; border-radius: 8px; }
.topic-info { display: flex; align-items: center; gap: 12px; }
.badge { background: #1f6feb; color: white; padding: 4px 8px; border-radius: 12px; font-size: 12px; font-weight: 600; }
.controls { display: flex; gap: 8px; }
.main-layout { display: flex; gap: 16px; flex: 1; height: calc(100vh - 90px); overflow: hidden; }
.chat-container { flex: 2; background: #161b22; border: 1px solid #30363d; border-radius: 8px; display: flex; flex-direction: column; overflow: hidden; }
.transcript { flex: 1; overflow-y: auto; padding: 16px; display: flex; flex-direction: column; gap: 16px; }
.message { padding: 12px 16px; border-radius: 8px; background: #21262d; border: 1px solid #30363d; max-width: 85%; }
.message.user { background: #1f6feb22; border-color: #1f6feb55; align-self: flex-end; }
.message-header { font-weight: 600; font-size: 13px; margin-bottom: 4px; color: #58a6ff; }
.message-body { font-size: 14px; line-height: 1.5; white-space: pre-wrap; word-break: break-word; }
.input-bar { display: flex; padding: 12px; background: #161b22; border-top: 1px solid #30363d; gap: 8px; }
.input-bar input { flex: 1; }
.input-bar button { width: auto; }
.sidebar { flex: 1; display: flex; flex-direction: column; gap: 16px; max-width: 350px; }
.panel { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 16px; }
.panel h3 { font-size: 14px; margin-bottom: 12px; color: #f0f6fc; border-bottom: 1px solid #30363d; padding-bottom: 6px; }
.participants-list { display: flex; flex-direction: column; gap: 8px; }
.participant-item { display: flex; align-items: center; justify-content: space-between; font-size: 13px; padding: 6px 8px; background: #21262d; border-radius: 4px; }
.notes-panel { flex: 1; overflow-y: auto; }
.notes-content { font-size: 13px; line-height: 1.6; color: #8b949e; white-space: pre-wrap; }
'use strict';
let config = null;
let currentSessionId = null;
let eventSource = null;

async function init() {
  const res = await fetch('/api/config');
  config = await res.json();
  if (config.requiresPassword && !config.authed) {
    document.getElementById('password-group').style.display = 'block';
  }
  const grid = document.getElementById('models-checklist');
  grid.innerHTML = config.models.map(m => `
    <label class="checkbox-item">
      <input type="checkbox" name="model" value="${m.id}" checked>
      <span style="color: ${m.color}">●</span> ${m.name}
    </label>
  `).join('');
}

document.getElementById('start-btn').addEventListener('click', async () => {
  const topic = document.getElementById('topic-input').value.trim();
  const password = document.getElementById('app-password')?.value || '';
  const checkedModels = [...document.querySelectorAll('input[name="model"]:checked')].map(cb => cb.value);

  if (!topic) { alert('請輸入討論主題'); return; }

  const headers = { 'content-type': 'application/json' };
  if (password) headers['x-app-password'] = password;

  const res = await fetch('/api/sessions', {
    method: 'POST',
    headers,
    body: JSON.stringify({ topic, participants: checkedModels })
  });
  const data = await res.json();
  if (!res.ok) { alert(data.error || '建立討論失敗'); return; }

  currentSessionId = data.id;
  document.getElementById('setup-screen').style.display = 'none';
  document.getElementById('discussion-screen').style.display = 'flex';
  document.getElementById('current-topic-title').textContent = topic;
  connectSSE();
});

function connectSSE() {
  const password = document.getElementById('app-password')?.value || '';
  // 注意：EventSource 不易自訂 Header，因此透過網址參數或直接連線
  eventSource = new EventSource(`/api/sessions/${currentSessionId}/events`);

  let activeMessageDiv = null;

  eventSource.addEventListener('init', (e) => {
    const data = JSON.parse(e.data);
    updateParticipants(data.participants);
    if (data.notes) document.getElementById('notes-content').textContent = data.notes;
    data.transcript.forEach(appendMessage);
  });

  eventSource.addEventListener('turn_start', (e) => {
    const data = JSON.parse(e.data);
    const transcript = document.getElementById('transcript');
    const div = document.createElement('div');
    div.className = 'message';
    div.id = `msg-${data.speaker}`;
    div.innerHTML = `<div class="message-header">${data.speaker} 發言中...</div><div class="message-body"></div>`;
    transcript.appendChild(div);
    transcript.scrollTop = transcript.scrollHeight;
    activeMessageDiv = div;
  });

  eventSource.addEventListener('delta', (e) => {
    const data = JSON.parse(e.data);
    if (activeMessageDiv) {
      const body = activeMessageDiv.querySelector('.message-body');
      body.textContent += data.text;
      const transcript = document.getElementById('transcript');
      transcript.scrollTop = transcript.scrollHeight;
    }
  });

  eventSource.addEventListener('turn_end', (e) => {
    const data = JSON.parse(e.data);
    if (activeMessageDiv) {
      activeMessageDiv.querySelector('.message-header').textContent = `${data.name} (第 ${data.turn} 輪)`;
      activeMessageDiv = null;
    }
  });

  eventSource.addEventListener('user_msg', (e) => {
    const data = JSON.parse(e.data);
    appendMessage(data);
  });

  eventSource.addEventListener('notes', (e) => {
    const data = JSON.parse(e.data);
    document.getElementById('notes-content').textContent = data.notes;
  });

  eventSource.addEventListener('status', (e) => {
    const data = JSON.parse(e.data);
    document.getElementById('status-badge').textContent = data.status === 'running' ? '進行中' : data.status === 'paused' ? '已暫停' : '已結束';
  });
}

function appendMessage(msg) {
  const transcript = document.getElementById('transcript');
  const div = document.createElement('div');
  div.className = `message ${msg.speaker === 'user' ? 'user' : ''}`;
  div.innerHTML = `
    <div class="message-header">${msg.name}</div>
    <div class="message-body">${escapeHtml(msg.text)}</div>
  `;
  transcript.appendChild(div);
  transcript.scrollTop = transcript.scrollHeight;
}

function escapeHtml(str) {
  return str.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
}

function updateParticipants(parts) {
  const list = document.getElementById('participants-list');
  list.innerHTML = parts.map(p => `
    <div class="participant-item">
      <span><strong style="color: ${p.color}">●</strong> ${p.name}</span>
      <span>發言 ${p.spoke} 次</span>
    </div>
  `).join('');
}

document.getElementById('send-btn').addEventListener('click', sendUserMessage);
document.getElementById('user-input').addEventListener('keypress', (e) => {
  if (e.key === 'Enter') sendUserMessage();
});

async function sendUserMessage() {
  const input = document.getElementById('user-input');
  const text = input.value.trim();
  if (!text || !currentSessionId) return;
  input.value = '';
  await fetch(`/api/sessions/${currentSessionId}/say`, {
    method: 'POST',
    headers: { 'content-type': 'application/json' },
    body: JSON.stringify({ text })
  });
}

document.getElementById('pause-resume-btn').addEventListener('click', async () => {
  const btn = document.getElementById('pause-resume-btn');
  const action = btn.textContent === '暫停' ? 'pause' : 'resume';
  await controlSession(action);
  btn.textContent = action === 'pause' ? '繼續' : '暫停';
});

document.getElementById('stop-btn').addEventListener('click', async () => {
  if (confirm('確定要結束這場討論嗎？')) {
    await controlSession('stop');
    if (eventSource) eventSource.close();
    alert('討論已結束');
    location.reload();
  }
});

async function controlSession(action) {
  if (!currentSessionId) return;
  await fetch(`/api/sessions/${currentSessionId}/control`, {
    method: 'POST',
    headers: { 'content-type': 'application/json' },
    body: JSON.stringify({ action })
  });
}

init();
# 伺服器設定
PORT=3000
HOST=127.0.0.1

# 啟用 DEMO 模式（無 API 金鑰也能直接測試執行）
DEMO_MODE=true

# 各家 AI API 金鑰（正式使用時請填入）
XAI_API_KEY=
OPENAI_API_KEY=
ANTHROPIC_API_KEY=
GEMINI_API_KEY=
DEEPSEEK_API_KEY=

# 可選：設定網站存取密碼
APP_PASSWORD=
'use strict';
const assert = require('assert');
const { createServer } = require('../server');

async function runTests() {
  console.log('正在執行自動化測試...');
  const server = createServer();
  await new Promise((resolve) => server.listen(0, resolve));
  const port = server.address().port;
  const base = `http://127.0.0.1:${port}`;

  // 測試 1: 取得 Config
  const res1 = await fetch(`${base}/api/config`);
  const cfg1 = await res1.json();
  assert.strictEqual(res1.status, 200);
  assert.strictEqual(cfg1.demo, true);

  // 測試 2: 建立 Session (DEMO 模式)
  const res2 = await fetch(`${base}/api/sessions`, {
    method: 'POST',
    headers: { 'content-type': 'application/json' },
    body: JSON.stringify({ topic: '測試主題：未來科技發展' })
  });
  const sessionData = await res2.json();
  assert.strictEqual(res2.status, 201);
  assert.ok(sessionData.id);

  server.close();
  console.log('✅ 所有測試通過！');
}

runTests().catch((err) => {
  console.error('❌ 測試失敗：', err);
  process.exit(1);
});
├── server.js
├── discussion.js
├── providers.js
├── package.json
├── README.md
├── .env.example
├── test/
│   └─ test.js
└── public/
    ├── index.html
    ├── styles.css
    └─ app.js
# 1. 複製環境變數設定檔
cp .env.example .env

# 2. 啟動 DEMO 模式進行測試（不需要花費 API 費用）
npm run demo
