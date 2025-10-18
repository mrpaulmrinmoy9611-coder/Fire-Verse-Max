<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
  <title>FireVerse Max</title>
  <link rel="manifest" href="manifest.json" />
  <meta name="theme-color" content="#111111" />
  <style>
    html, body {
      margin: 0; padding: 0; background: #0b0d12; color: #e9edf5; font-family: system-ui, Arial, sans-serif;
      height: 100%; overflow: hidden;
    }
    #ui {
      position: fixed; top: 16px; left: 16px; right: 16px; display: flex; align-items: center; justify-content: space-between;
      z-index: 10;
    }
    .pill { background:#151922; border:1px solid #2a2f3b; border-radius: 12px; padding:8px 12px; font-size:14px; }
    #hud { display:flex; gap:8px; align-items:center; }
    #healthBar {
      width: 160px; height: 12px; background:#2a2f3b; border-radius:6px; overflow:hidden; border:1px solid #3a4150;
    }
    #healthFill { height:100%; width:100%; background: linear-gradient(90deg, #ff3e3e, #ff9248); }
    #touch {
      position: fixed; bottom: 16px; left: 16px; right: 16px; display:flex; justify-content: space-between; z-index: 10;
    }
    .btn {
      width: 84px; height: 84px; border-radius: 50%; background: #151922cc; border:1px solid #2a2f3bcc; color:#e9edf5;
      display:flex; align-items:center; justify-content:center; user-select:none;
    }
    .stick {
      width: 140px; height: 140px; border-radius: 50%; background: #151922cc; border:1px solid #2a2f3bcc; position:relative;
    }
    .knob {
      width: 68px; height: 68px; border-radius: 50%; background:#2a2f3b; border:1px solid #3a4150; position:absolute; left:36px; top:36px;
    }
    canvas { display:block; width:100vw; height:100vh; }
    #notice { position: fixed; bottom: 8px; left: 50%; transform: translateX(-50%); color:#8fa0bf; font-size:12px; }
  </style>
</head>
<body>
  <div id="ui">
    <div id="hud">
      <div class="pill">FireVerse Max</div>
      <div id="healthBar"><div id="healthFill"></div></div>
      <div class="pill">Score: <span id="score">0</span></div>
      <div class="pill">Enemies: <span id="enemyCount">0</span></div>
    </div>
    <div class="pill"><button id="installBtn" style="background:#1d2330;color:#e9edf5;border:1px solid #2a2f3b;border-radius:8px;padding:6px 10px;cursor:pointer">Install</button></div>
  </div>

  <canvas id="game"></canvas>

  <div id="touch">
    <div class="stick" id="moveStick"><div class="knob" id="moveKnob"></div></div>
    <div style="display:flex; gap:12px;">
      <div class="btn" id="shootBtn">Shoot</div>
      <div class="btn" id="dashBtn">Dash</div>
    </div>
  </div>

  <div id="notice">Tip: WASD to move, Mouse to aim, Left click to shoot, Shift to dash. On mobile, use on-screen controls.</div>

  <script>
    // PWA install prompt
    let deferredPrompt;
    const installBtn = document.getElementById('installBtn');
    window.addEventListener('beforeinstallprompt', (e) => {
      e.preventDefault();
      deferredPrompt = e;
      installBtn.style.display = 'inline-block';
    });
    installBtn.addEventListener('click', async () => {
      if (!deferredPrompt) return;
      deferredPrompt.prompt();
      await deferredPrompt.userChoice;
      deferredPrompt = null;
      installBtn.style.display = 'none';
    });

    // Register service worker
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('sw.js');
    }

    // Game setup
    const canvas = document.getElementById('game');
    const ctx = canvas.getContext('2d');
    function resize() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resize);
    resize();

    // State
    const state = {
      player: { x: canvas.width/2, y: canvas.height/2, r: 22, speed: 3.2, hp: 100, maxHp: 100, dash: 0, aimX: 0, aimY: 0 },
      bullets: [],
      enemies: [],
      score: 0,
      lastSpawn: 0,
      spawnRate: 1400,
      keys: {},
      mobile: ('ontouchstart' in window)
    };

    // Input (keyboard/mouse)
    window.addEventListener('keydown', (e) => state.keys[e.key.toLowerCase()] = true);
    window.addEventListener('keyup', (e) => state.keys[e.key.toLowerCase()] = false);
    canvas.addEventListener('mousemove', (e) => {
      const rect = canvas.getBoundingClientRect();
      state.player.aimX = e.clientX - rect.left;
      state.player.aimY = e.clientY - rect.top;
    });
    canvas.addEventListener('mousedown', () => shoot());

    // Mobile joystick
    const moveStick = document.getElementById('moveStick');
    const moveKnob = document.getElementById('moveKnob');
    let stickActive = false, stickStart = {x:0,y:0}, stickVec = {x:0,y:0};
    function stickPosToVec(clientX, clientY) {
      const rect = moveStick.getBoundingClientRect();
      const cx = rect.left + rect.width/2, cy = rect.top + rect.height/2;
      const dx = clientX - cx, dy = clientY - cy;
      const len = Math.hypot(dx, dy);
      const max = rect.width/2 - 34;
      const nx = len ? dx/len : 0, ny = len ? dy/len : 0;
      const clampLen = Math.min(len, max);
      moveKnob.style.left = (rect.width/2 - 34 + nx*clampLen) + 'px';
      moveKnob.style.top  = (rect.height/2 - 34 + ny*clampLen) + 'px';
      stickVec.x = (clampLen / max) * nx;
      stickVec.y = (clampLen / max) * ny;
    }
    function resetStick() {
      moveKnob.style.left = '36px'; moveKnob.style.top = '36px'; stickVec.x = 0; stickVec.y = 0;
    }
    moveStick.addEventListener('touchstart', (e) => { stickActive = true; stickPosToVec(e.touches[0].clientX, e.touches[0].clientY); });
    moveStick.addEventListener('touchmove',  (e) => { stickPosToVec(e.touches[0].clientX, e.touches[0].clientY); });
    moveStick.addEventListener('touchend',   () => { stickActive = false; resetStick(); });

    // Mobile buttons
    document.getElementById('shootBtn').addEventListener('touchstart', () => shoot());
    document.getElementById('dashBtn').addEventListener('touchstart', () => dash());

    // Core functions
    function shoot() {
      const p = state.player;
      const mx = state.mobile ? p.x + (p.r+60) * Math.cos(pAngle()) : p.aimX;
      const my = state.mobile ? p.y + (p.r+60) * Math.sin(pAngle()) : p.aimY;
      const ang = Math.atan2(my - p.y, mx - p.x);
      state.bullets.push({ x: p.x + Math.cos(ang)*p.r, y: p.y + Math.sin(ang)*p.r, vx: Math.cos(ang)*9, vy: Math.sin(ang)*9, life: 110 });
    }
    function dash() {
      const p = state.player;
      if (p.dash <= 0) { p.dash = 18; }
    }
    function pAngle() {
      // Mobile aim: use movement vector or last mouse
      const vecX = stickVec.x, vecY = stickVec.y;
      if (Math.abs(vecX) + Math.abs(vecY) > 0.1) return Math.atan2(vecY, vecX);
      const mx = state.player.aimX || state.player.x, my = state.player.aimY || state.player.y;
      return Math.atan2(my - state.player.y, mx - state.player.x);
    }

    // Enemy spawn
    function spawnEnemy() {
      const edge = Math.floor(Math.random()*4);
      const margin = 60;
      let x = 0, y = 0;
      if (edge === 0) { x = Math.random()*canvas.width; y = -margin; }
      if (edge === 1) { x = canvas.width + margin; y = Math.random()*canvas.height; }
      if (edge === 2) { x = Math.random()*canvas.width; y = canvas.height + margin; }
      if (edge === 3) { x = -margin; y = Math.random()*canvas.height; }
      const speed = 1.4 + Math.random()*1.6;
      const hp = 26 + Math.floor(Math.random()*30);
      state.enemies.push({ x, y, r: 18, speed, hp });
      document.getElementById('enemyCount').textContent = state.enemies.length;
    }

    // Update
    function update(dt) {
      const p = state.player;
      // Movement (keyboard)
      const k = state.keys;
      let mx = 0, my = 0;
      if (k['w'] || k['arrowup']) my -= 1;
      if (k['s'] || k['arrowdown']) my += 1;
      if (k['a'] || k['arrowleft']) mx -= 1;
      if (k['d'] || k['arrowright']) mx += 1;
      // Movement (mobile joystick)
      if (stickActive) { mx = stickVec.x; my = stickVec.y; }
      const len = Math.hypot(mx, my) || 1;
      const dashBoost = p.dash > 0 ? 3.6 : 1;
      p.x += (mx/len) * p.speed * dashBoost;
      p.y += (my/len) * p.speed * dashBoost;
      p.x = Math.max(p.r, Math.min(canvas.width - p.r, p.x));
      p.y = Math.max(p.r, Math.min(canvas.height - p.r, p.y));
      if (p.dash > 0) p.dash -= 1;

      // Bullets
      for (let i = state.bullets.length - 1; i >= 0; i--) {
        const b = state.bullets[i];
        b.x += b.vx; b.y += b.vy; b.life--;
        if (b.life <= 0 || b.x < -80 || b.y < -80 || b.x > canvas.width+80 || b.y > canvas.height+80) {
          state.bullets.splice(i, 1);
        }
      }

      // Enemies move towards player
      for (let i = state.enemies.length - 1; i >= 0; i--) {
        const e = state.enemies[i];
        const ang = Math.atan2(p.y - e.y, p.x - e.x);
        e.x += Math.cos(ang) * e.speed;
        e.y += Math.sin(ang) * e.speed;

        // Enemy hits player
        const hitP = Math.hypot(e.x - p.x, e.y - p.y) < (e.r + p.r);
        if (hitP) {
          p.hp = Math.max(0, p.hp - 8);
          e.hp = 0; // die on contact
          updateHealth();
          if (p.hp <= 0) gameOver();
        }

        // Bullet hits enemy
        for (let j = state.bullets.length - 1; j >= 0; j--) {
          const b = state.bullets[j];
          if (Math.hypot(e.x - b.x, e.y - b.y) < (e.r + 6)) {
            e.hp -= 26;
            state.bullets.splice(j, 1);
            if (e.hp <= 0) {
              state.enemies.splice(i, 1);
              state.score += 10;
              document.getElementById('score').textContent = state.score;
              document.getElementById('enemyCount').textContent = state.enemies.length;
              break;
            }
          }
        }
      }

      // Spawn pacing
      const now = performance.now();
      if (now - state.lastSpawn > state.spawnRate) {
        spawnEnemy();
        state.lastSpawn = now;
        // Gradually increase difficulty
        state.spawnRate = Math.max(400, state.spawnRate - 12);
      }
    }

    // Draw
    function draw() {
      // Background
      const grd = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
      grd.addColorStop(0, '#0b0d12');
      grd.addColorStop(1, '#111522');
      ctx.fillStyle = grd;
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      // Arena grid
      ctx.strokeStyle = '#1a2030';
      ctx.lineWidth = 1;
      for (let x = 0; x < canvas.width; x += 48) {
        ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
      }
      for (let y = 0; y < canvas.height; y += 48) {
        ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
      }

      // Player
      const p = state.player;
      ctx.save();
      ctx.translate(p.x, p.y);
      ctx.rotate(pAngle());
      ctx.fillStyle = '#ff6a3d';
      ctx.beginPath(); ctx.arc(0, 0, p.r, 0, Math.PI*2); ctx.fill();
      // Player blade
      ctx.fillStyle = '#ffd166';
      ctx.fillRect(p.r - 6, -4, 24, 8);
      ctx.restore();

      // Bullets
      ctx.fillStyle = '#f1faee';
      for (const b of state.bullets) {
        ctx.beginPath(); ctx.arc(b.x, b.y, 6, 0, Math.PI*2); ctx.fill();
      }

      // Enemies
      for (const e of state.enemies) {
        ctx.fillStyle = '#3a86ff';
        ctx.beginPath(); ctx.arc(e.x, e.y, e.r, 0, Math.PI*2); ctx.fill();
        // Glow ring
        ctx.strokeStyle = '#6cc1ff55'; ctx.lineWidth = 6;
        ctx.beginPath(); ctx.arc(e.x, e.y, e.r+8, 0, Math.PI*2); ctx.stroke();
      }
    }

    // HUD
    function updateHealth() {
      const pct = Math.max(0, state.player.hp) / state.player.maxHp;
      document.getElementById('healthFill').style.width = (pct*100).toFixed(0) + '%';
    }

    // Game over
    function gameOver() {
      // Freeze spawns and show overlay
      state.spawnRate = 999999;
      ctx.fillStyle = '#000000aa';
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = '#e9edf5';
      ctx.textAlign = 'center';
      ctx.font = 'bold 42px system-ui, Arial';
      ctx.fillText('Game Over', canvas.width/2, canvas.height/2 - 20);
      ctx.font = '16px system-ui, Arial';
      ctx.fillText('Tap or Press R to restart', canvas.width/2, canvas.height/2 + 24);

      function restart() {
        // Reset everything
        state.player.x = canvas.width/2; state.player.y = canvas.height/2;
        state.player.hp = state.player.maxHp; updateHealth();
        state.bullets.length = 0; state.enemies.length = 0;
        state.score = 0; document.getElementById('score').textContent = 0;
        document.getElementById('enemyCount').textContent = 0;
        state.spawnRate = 1400; state.lastSpawn = performance.now();
        window.removeEventListener('keydown', restartKey);
        canvas.removeEventListener('mousedown', restartClick);
        loop(performance.now());
      }
      function restartKey(e){ if (e.key.toLowerCase() === 'r') restart(); }
      function restartClick(){ restart(); }
      window.addEventListener('keydown', restartKey, { once: true });
      canvas.addEventListener('mousedown', restartClick, { once: true });
    }

    // Main loop
    let last = performance.now();
    function loop(t) {
      const dt = t - last; last = t;
      update(dt);
      draw();
      requestAnimationFrame(loop);
    }
    // Start
    updateHealth();
    state.lastSpawn = performance.now();
    loop(performance.now());

    // Extra keyboard actions
    window.addEventListener('keydown', (e) => {
      if (e.key.toLowerCase() === 'shift') dash();
      if (e.key.toLowerCase() === ' ') shoot();
    });
  </script>
</body>
</html>