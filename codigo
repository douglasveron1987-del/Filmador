<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-fullscreen">
<meta name="mobile-web-app-capable" content="yes">
<title>Filmador</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

  html, body {
    width: 100%; height: 100%;
    background: #000;
    overflow: hidden;
    font-family: 'Helvetica Neue', Arial, sans-serif;
    -webkit-user-select: none; user-select: none;
  }

  /* ── TELA DE INÍCIO ── */
  #setupScreen {
    position: fixed; inset: 0;
    background: #0c0c0e;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 100;
    padding: 40px 32px;
    gap: 0;
  }

  .setup-logo {
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.3em;
    color: rgba(255,255,255,0.18);
    text-transform: uppercase;
    margin-bottom: 56px;
  }

  /* Nome do dispositivo detectado */
  #deviceNameDisplay {
    font-size: 28px;
    font-weight: 600;
    color: #fff;
    text-align: center;
    letter-spacing: -0.01em;
    margin-bottom: 8px;
    line-height: 1.2;
  }

  .setup-subtitle {
    font-size: 14px;
    color: rgba(255,255,255,0.3);
    margin-bottom: 48px;
    text-align: center;
    letter-spacing: 0.01em;
  }

  /* Campo servidor — colapsável, discreto */
  .server-toggle {
    font-size: 12px;
    color: rgba(255,255,255,0.22);
    cursor: pointer;
    margin-bottom: 12px;
    letter-spacing: 0.06em;
    text-decoration: underline;
    text-underline-offset: 3px;
    text-decoration-color: rgba(255,255,255,0.12);
  }

  #serverWrap {
    display: none;
    width: 100%;
    max-width: 300px;
    margin-bottom: 24px;
  }
  #serverWrap.open { display: block; }

  .setup-server-input {
    background: rgba(255,255,255,0.05);
    border: 1.5px solid rgba(255,255,255,0.1);
    border-radius: 10px;
    color: #fff;
    font-size: 14px;
    padding: 12px 16px;
    outline: none;
    width: 100%;
    text-align: center;
    font-family: 'SF Mono', 'Courier New', monospace;
    -webkit-appearance: none;
    transition: border-color 0.2s;
  }
  .setup-server-input:focus { border-color: rgba(255,255,255,0.3); }
  .setup-server-input::placeholder { color: rgba(255,255,255,0.18); }

  .start-btn {
    width: 100%;
    max-width: 300px;
    padding: 18px;
    border-radius: 16px;
    border: none;
    background: #fff;
    color: #000;
    font-size: 17px;
    font-weight: 700;
    cursor: pointer;
    letter-spacing: 0.01em;
    transition: opacity 0.15s, transform 0.1s;
    margin-top: 8px;
  }
  .start-btn:active { opacity: 0.75; transform: scale(0.98); }

  /* ── TELA DA CÂMERA ── */
  #camScreen {
    position: fixed; inset: 0;
    display: none;
  }

  video {
    position: absolute; inset: 0;
    width: 100%; height: 100%;
    object-fit: cover;
  }

  /* ── BORDA TALLY ── */
  #tallyBorder {
    position: absolute; inset: 0;
    border: 5px solid transparent;
    pointer-events: none;
    z-index: 10;
    transition: border-color 0.3s ease, box-shadow 0.3s ease;
  }
  /* VERDE = Prévia */
  #tallyBorder.preview {
    border-color: #22c55e;
    box-shadow: inset 0 0 32px rgba(34,197,94,0.2);
  }
  /* VERMELHO = Ao Vivo */
  #tallyBorder.program {
    border-color: #e53935;
    box-shadow: inset 0 0 36px rgba(229,57,53,0.25);
    animation: tallyPulse 1.4s ease-in-out infinite;
  }
  @keyframes tallyPulse {
    0%, 100% { box-shadow: inset 0 0 36px rgba(229,57,53,0.25); }
    50%       { box-shadow: inset 0 0 56px rgba(229,57,53,0.42); }
  }

  /* ── HUD ── */
  #hud {
    position: absolute; inset: 0;
    pointer-events: none;
    z-index: 20;
  }

  /* Nome do celular — canto superior esquerdo */
  #camNameBadge {
    position: absolute;
    top: calc(env(safe-area-inset-top, 0px) + 14px);
    left: 16px;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.16em;
    color: rgba(255,255,255,0.45);
    text-transform: uppercase;
    max-width: 50vw;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  /* Tally pill — canto superior direito */
  #tallyPill {
    position: absolute;
    top: calc(env(safe-area-inset-top, 0px) + 10px);
    right: 14px;
    display: flex;
    align-items: center;
    gap: 5px;
    padding: 5px 11px;
    border-radius: 100px;
    background: rgba(0,0,0,0.4);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    transition: all 0.3s;
  }
  .tally-dot-small {
    width: 7px; height: 7px;
    border-radius: 50%;
    background: rgba(255,255,255,0.2);
    transition: all 0.3s;
  }
  /* verde preview */
  #tallyPill.preview .tally-dot-small {
    background: #22c55e;
    box-shadow: 0 0 6px #22c55e;
  }
  /* vermelho program */
  #tallyPill.program .tally-dot-small {
    background: #e53935;
    box-shadow: 0 0 6px #e53935;
    animation: dotPulse 1.4s infinite;
  }
  @keyframes dotPulse { 0%,100%{opacity:1} 50%{opacity:0.25} }

  #tallyLabel {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.12em;
    color: rgba(255,255,255,0.35);
    transition: color 0.3s;
  }
  #tallyPill.preview #tallyLabel { color: #22c55e; }
  #tallyPill.program #tallyLabel { color: #e53935; }

  /* Conexão — canto inferior esquerdo */
  #connStatus {
    position: absolute;
    bottom: calc(env(safe-area-inset-bottom, 0px) + 16px);
    left: 16px;
    display: flex;
    align-items: center;
    gap: 5px;
    pointer-events: auto;
  }
  .conn-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: rgba(255,255,255,0.15);
    transition: background 0.3s;
  }
  .conn-dot.on  { background: #22c55e; }
  .conn-dot.err { background: #e53935; }
  #connLabel {
    font-size: 10px;
    color: rgba(255,255,255,0.22);
    letter-spacing: 0.08em;
    font-weight: 600;
  }

  /* Sair — canto inferior direito */
  #exitBtn {
    position: absolute;
    bottom: calc(env(safe-area-inset-bottom, 0px) + 12px);
    right: 14px;
    padding: 6px 14px;
    border-radius: 100px;
    border: 1px solid rgba(255,255,255,0.1);
    background: rgba(0,0,0,0.3);
    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);
    color: rgba(255,255,255,0.25);
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.08em;
    cursor: pointer;
    pointer-events: auto;
  }

  /* ── COMANDO OVERLAY ──
     Fica fixo no topo até a câmera entrar ao vivo.
     Some automaticamente quando tally = program. */
  #cmdOverlay {
    position: absolute;
    top: calc(env(safe-area-inset-top, 0px) + 48px);
    left: 14px; right: 14px;
    z-index: 30;
    pointer-events: none;
    opacity: 0;
    transform: translateY(-8px);
    transition: opacity 0.28s ease, transform 0.32s cubic-bezier(0.34,1.4,0.64,1);
  }
  #cmdOverlay.show {
    opacity: 1;
    transform: translateY(0);
  }

  .cmd-bubble {
    background: rgba(8,8,14,0.82);
    backdrop-filter: blur(18px) saturate(200%);
    -webkit-backdrop-filter: blur(18px) saturate(200%);
    border-radius: 13px;
    padding: 13px 15px;
    display: flex;
    align-items: center;
    gap: 12px;
    border: 1px solid rgba(255,255,255,0.09);
  }

  .cmd-icon-wrap {
    width: 38px; height: 38px;
    border-radius: 9px;
    background: rgba(255,255,255,0.07);
    display: flex; align-items: center; justify-content: center;
    font-size: 20px;
    flex-shrink: 0;
  }

  .cmd-text-wrap { flex: 1; min-width: 0; }
  .cmd-from {
    font-size: 9px;
    font-weight: 700;
    letter-spacing: 0.16em;
    color: rgba(255,255,255,0.3);
    text-transform: uppercase;
    margin-bottom: 3px;
  }
  #cmdText {
    font-size: 18px;
    font-weight: 600;
    color: #fff;
    line-height: 1.25;
    white-space: normal;
  }

  /* Flash */
  #flashOverlay {
    position: absolute; inset: 0;
    background: rgba(255,255,255,0);
    pointer-events: none;
    z-index: 50;
    transition: background 0.07s;
  }
  #flashOverlay.flash { background: rgba(255,255,255,0.14); }
</style>
</head>
<body>

<!-- TELA DE INÍCIO -->
<div id="setupScreen">
  <div class="setup-logo">CamCmd · Filmador</div>

  <div id="deviceNameDisplay">Carregando…</div>
  <div class="setup-subtitle">Este nome aparecerá no painel do operador</div>

  <div class="server-toggle" onclick="toggleServer()">▸ Configurar servidor</div>
  <div id="serverWrap">
    <input class="setup-server-input" id="serverInput" type="text"
      value="ws://192.168.1.100:8080"
      placeholder="ws://IP:8080"
      autocomplete="off" autocorrect="off" spellcheck="false">
  </div>

  <button class="start-btn" onclick="startCamera()">Iniciar Câmera</button>
</div>

<!-- TELA DA CÂMERA -->
<div id="camScreen">
  <video id="video" autoplay muted playsinline webkit-playsinline></video>

  <div id="tallyBorder"></div>
  <div id="flashOverlay"></div>

  <div id="hud">
    <div id="camNameBadge"></div>
    <div id="tallyPill">
      <div class="tally-dot-small"></div>
      <span id="tallyLabel">OFF</span>
    </div>
    <div id="connStatus">
      <div class="conn-dot" id="connDot"></div>
      <span id="connLabel">DESCONECTADO</span>
    </div>
    <button id="exitBtn" onclick="exitCamera()">SAIR</button>
  </div>

  <!-- OVERLAY DO COMANDO — sem timer, some só quando vai ao vivo -->
  <div id="cmdOverlay">
    <div class="cmd-bubble">
      <div class="cmd-icon-wrap" id="cmdIcon">📢</div>
      <div class="cmd-text-wrap">
        <div class="cmd-from">Diretor</div>
        <div id="cmdText"></div>
      </div>
    </div>
  </div>
</div>

<script>
  // ── DETECTAR NOME DO DISPOSITIVO ──
  function getDeviceName() {
    const ua = navigator.userAgent;
    // iOS: tenta pegar modelo
    if (/iPhone/.test(ua)) {
      // Não temos acesso ao nome real do iPhone via browser por segurança,
      // mas usamos um nome editável que o usuário pode customizar
      return 'iPhone';
    }
    if (/iPad/.test(ua)) return 'iPad';
    // Android: tenta extrair o modelo
    const android = ua.match(/Android[^;]*;\s*([^)]+)\)/);
    if (android) return android[1].trim().split(' ').slice(0,3).join(' ');
    // Desktop (para testes)
    const hostname = location.hostname || 'Câmera';
    return hostname !== 'localhost' && hostname !== '127.0.0.1' ? hostname : 'Câmera Desktop';
  }

  let deviceName = getDeviceName();

  // Torna o nome editável ao clicar
  const nameEl = document.getElementById('deviceNameDisplay');
  nameEl.textContent = deviceName;
  nameEl.title = 'Toque para editar';
  nameEl.style.cursor = 'pointer';
  nameEl.addEventListener('click', () => {
    const novo = prompt('Nome desta câmera:', deviceName);
    if (novo && novo.trim()) {
      deviceName = novo.trim();
      nameEl.textContent = deviceName;
    }
  });

  // ── SERVIDOR TOGGLE ──
  function toggleServer() {
    const wrap = document.getElementById('serverWrap');
    wrap.classList.toggle('open');
  }

  // ── STATE ──
  let ws = null;
  let currentTally = 'none';
  let pendingCmd = null; // guarda comando se não houver um ativo

  // ── INICIAR CÂMERA ──
  async function startCamera() {
    const serverUrl = document.getElementById('serverInput').value.trim();

    let stream;
    try {
      stream = await navigator.mediaDevices.getUserMedia({
        video: { facingMode: { ideal: 'environment' }, width: { ideal: 1920 }, height: { ideal: 1080 } },
        audio: false
      });
    } catch(e) {
      alert('Permissão de câmera necessária. Habilite nas configurações do navegador.');
      return;
    }

    document.getElementById('setupScreen').style.display = 'none';
    document.getElementById('camScreen').style.display = 'block';
    document.getElementById('video').srcObject = stream;
    document.getElementById('camNameBadge').textContent = deviceName;

    try { await screen.orientation.lock('landscape'); } catch(e) {}
    try { document.documentElement.requestFullscreen?.(); } catch(e) {}

    keepAwake();
    connectWS(serverUrl);
  }

  // ── WEBSOCKET ──
  function connectWS(url) {
    setConn('connecting');
    try { ws = new WebSocket(url); } catch(e) { retry(url); return; }

    ws.onopen = () => {
      setConn('connected');
      // Registra com o nome do dispositivo como ID
      ws.send(JSON.stringify({ type: 'register', id: deviceName, name: deviceName }));
    };

    ws.onmessage = (evt) => {
      try {
        const data = JSON.parse(evt.data);
        if (data.type === 'command' && matchesMe(data.target)) {
          receiveCommand(data.msg);
        }
        if (data.type === 'tally' && matchesMe(data.target)) {
          setTally(data.state);
        }
      } catch(e) {}
    };

    ws.onclose = () => { setConn('error'); retry(url); };
    ws.onerror = () => { setConn('error'); };
  }

  function retry(url) { setTimeout(() => connectWS(url), 4000); }

  function matchesMe(target) {
    if (!target) return false;
    const t = target.toLowerCase();
    return t === deviceName.toLowerCase() || t === 'all';
  }

  // ── TALLY ──
  function setTally(state) {
    const prev = currentTally;
    currentTally = state;

    const border = document.getElementById('tallyBorder');
    const pill   = document.getElementById('tallyPill');
    const label  = document.getElementById('tallyLabel');

    border.className = state;
    pill.className   = state;
    label.textContent = state === 'program' ? 'AO VIVO' : state === 'preview' ? 'PRÉVIA' : 'OFF';

    // Câmera foi ao vivo → esconde o comando automaticamente
    if (state === 'program') {
      hideCommand();
    }

    // Saiu do ar → se tinha comando pendente, mostra de volta
    if (prev === 'program' && state !== 'program' && pendingCmd) {
      showCommand(pendingCmd);
    }
  }

  // ── COMANDO ──
  const cmdIcons = {
    'ação': '🎬', 'corta': '✋', 'aproxima': '🔍', 'zoom in': '🔍',
    'afasta': '🔭', 'zoom out': '🔭', 'plano': '📐', 'close': '👤',
    'stand by': '⏳', 'standby': '⏳', 'atenção': '⚠️', 'move': '↔️',
    'pan': '↔️', 'tilt': '↕️', 'foco': '🎯', 'focus': '🎯',
  };

  function guessIcon(msg) {
    const l = msg.toLowerCase();
    for (const [k, v] of Object.entries(cmdIcons)) if (l.includes(k)) return v;
    return '📢';
  }

  function receiveCommand(msg) {
    pendingCmd = msg; // sempre guarda o último

    // Só exibe se não estiver ao vivo
    if (currentTally !== 'program') {
      showCommand(msg);
    }
    // se estiver ao vivo, fica guardado e aparece quando sair do ar
  }

  function showCommand(msg) {
    document.getElementById('cmdText').textContent  = msg;
    document.getElementById('cmdIcon').textContent  = guessIcon(msg);
    document.getElementById('cmdOverlay').classList.add('show');
    flashScreen();
    vibrate();
  }

  function hideCommand() {
    document.getElementById('cmdOverlay').classList.remove('show');
  }

  function flashScreen() {
    const f = document.getElementById('flashOverlay');
    f.classList.add('flash');
    setTimeout(() => f.classList.remove('flash'), 110);
  }

  function vibrate() {
    try { navigator.vibrate?.([35, 25, 35]); } catch(e) {}
  }

  // ── CONEXÃO ──
  function setConn(state) {
    const dot   = document.getElementById('connDot');
    const label = document.getElementById('connLabel');
    if (state === 'connected')  { dot.className = 'conn-dot on';  label.textContent = 'CONECTADO'; }
    else if (state === 'error') { dot.className = 'conn-dot err'; label.textContent = 'RECONECTANDO…'; }
    else                        { dot.className = 'conn-dot';     label.textContent = 'CONECTANDO…'; }
  }

  // ── SAIR ──
  function exitCamera() {
    if (confirm('Sair da câmera?')) {
      if (ws) ws.close();
      const v = document.getElementById('video');
      if (v.srcObject) v.srcObject.getTracks().forEach(t => t.stop());
      document.getElementById('camScreen').style.display = 'none';
      document.getElementById('setupScreen').style.display = 'flex';
      try { document.exitFullscreen?.(); } catch(e) {}
    }
  }

  // ── WAKE LOCK ──
  async function keepAwake() {
    try { if ('wakeLock' in navigator) await navigator.wakeLock.request('screen'); } catch(e) {}
    document.addEventListener('visibilitychange', async () => {
      if (document.visibilityState === 'visible') keepAwake();
    });
  }

  // ── DEMO MODE ── (quando servidor é o padrão)
  function startDemo() {
    setConn('connected');
    document.getElementById('connLabel').textContent = 'MODO DEMO';
    let s = 0;
    const steps = [
      () => setTally('preview'),
      () => receiveCommand('🔍 Aproxima no apresentador'),
      () => receiveCommand('📐 Plano americano'),
      () => setTally('program'),           // ao vivo: comando some
      () => setTally('preview'),           // saiu: comando volta
      () => receiveCommand('✋ CORTA — ótimo!'),
      () => setTally('none'),
    ];
    const run = () => { if (s < steps.length) { steps[s++](); setTimeout(run, 3500); } };
    setTimeout(run, 1500);
  }

  // Auto-demo se IP ainda for o padrão
  const _origStart = window.startCamera;
  window.startCamera = async function() {
    await _origStart?.();
    const url = document.getElementById('serverInput').value;
    if (url.includes('192.168.1.100')) setTimeout(startDemo, 800);
  };
</script>
</body>
</html>