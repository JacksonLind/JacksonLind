<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JacksonLind // SOC Analyst</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  :root {
    --blue: #00b4ff;
    --blue-dim: #0077aa;
    --blue-glow: rgba(0, 180, 255, 0.4);
    --blue-faint: rgba(0, 180, 255, 0.07);
    --bg: #020c14;
    --bg2: #040f1a;
    --panel: rgba(0, 20, 40, 0.85);
    --text: #c8e8f8;
    --text-dim: #4a7a9b;
    --green: #00ff88;
    --red: #ff3b5c;
    --warn: #ffaa00;
    --border: rgba(0, 180, 255, 0.2);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Share Tech Mono', monospace;
    min-height: 100vh;
    overflow-x: hidden;
    cursor: crosshair;
  }

  /* ── HEX GRID CANVAS ── */
  #hexCanvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    opacity: 0.35;
  }

  /* ── SCANLINES ── */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 9999;
  }

  /* ── CORNER BRACKETS ── */
  .corner-bracket {
    position: fixed;
    width: 40px; height: 40px;
    z-index: 100;
    opacity: 0.6;
  }
  .corner-bracket.tl { top:12px; left:12px; border-top: 2px solid var(--blue); border-left: 2px solid var(--blue); }
  .corner-bracket.tr { top:12px; right:12px; border-top: 2px solid var(--blue); border-right: 2px solid var(--blue); }
  .corner-bracket.bl { bottom:12px; left:12px; border-bottom: 2px solid var(--blue); border-left: 2px solid var(--blue); }
  .corner-bracket.br { bottom:12px; right:12px; border-bottom: 2px solid var(--blue); border-right: 2px solid var(--blue); }

  /* ── LAYOUT ── */
  .container {
    position: relative;
    z-index: 10;
    max-width: 900px;
    margin: 0 auto;
    padding: 40px 20px 80px;
  }

  /* ── HEADER ── */
  .header {
    text-align: center;
    padding: 60px 0 40px;
    position: relative;
  }

  .status-bar {
    display: flex;
    justify-content: center;
    gap: 24px;
    font-size: 10px;
    letter-spacing: 3px;
    color: var(--text-dim);
    margin-bottom: 32px;
    text-transform: uppercase;
  }
  .status-bar span { display: flex; align-items: center; gap: 6px; }
  .dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 8px var(--green);
    animation: pulse 2s ease-in-out infinite;
  }
  .dot.warn { background: var(--warn); box-shadow: 0 0 8px var(--warn); }

  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.3} }

  .name-display {
    font-family: 'Orbitron', monospace;
    font-size: clamp(36px, 8vw, 72px);
    font-weight: 900;
    letter-spacing: 6px;
    color: #fff;
    text-shadow: 0 0 40px var(--blue), 0 0 80px rgba(0,180,255,0.3);
    position: relative;
    display: inline-block;
  }

  .name-display::before {
    content: attr(data-text);
    position: absolute;
    top: 0; left: 0;
    color: var(--blue);
    clip-path: polygon(0 0, 100% 0, 100% 35%, 0 35%);
    opacity: 0.5;
    transform: translateX(-2px);
    animation: glitch1 4s infinite;
  }
  .name-display::after {
    content: attr(data-text);
    position: absolute;
    top: 0; left: 0;
    color: var(--red);
    clip-path: polygon(0 65%, 100% 65%, 100% 100%, 0 100%);
    opacity: 0.3;
    transform: translateX(2px);
    animation: glitch2 4s infinite;
  }

  @keyframes glitch1 {
    0%,90%,100% { transform: translateX(-2px); opacity:0.5; }
    92% { transform: translateX(4px) skewX(5deg); opacity:0.8; }
    94% { transform: translateX(-4px); opacity:0.2; }
    96% { transform: translateX(2px) skewX(-3deg); opacity:0.6; }
  }
  @keyframes glitch2 {
    0%,90%,100% { transform: translateX(2px); opacity:0.3; }
    93% { transform: translateX(-4px) skewX(-5deg); opacity:0.7; }
    95% { transform: translateX(4px); opacity:0.1; }
    97% { transform: translateX(-2px) skewX(2deg); opacity:0.5; }
  }

  .tagline-wrapper {
    margin-top: 16px;
    height: 28px;
    overflow: hidden;
  }
  .tagline {
    font-family: 'Rajdhani', sans-serif;
    font-size: 18px;
    font-weight: 300;
    letter-spacing: 4px;
    color: var(--blue);
    text-transform: uppercase;
  }
  .tagline .cursor {
    display: inline-block;
    width: 2px; height: 18px;
    background: var(--blue);
    margin-left: 2px;
    vertical-align: middle;
    animation: blink 0.8s step-end infinite;
  }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

  .divider {
    display: flex;
    align-items: center;
    gap: 16px;
    margin: 36px 0;
    opacity: 0.5;
  }
  .divider::before, .divider::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--blue), transparent);
  }
  .divider-icon { color: var(--blue); font-size: 12px; letter-spacing: 2px; }

  /* ── PANELS ── */
  .panel {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 2px;
    position: relative;
    overflow: hidden;
    backdrop-filter: blur(8px);
    transition: border-color 0.3s, box-shadow 0.3s;
  }
  .panel:hover {
    border-color: rgba(0,180,255,0.5);
    box-shadow: 0 0 30px rgba(0,180,255,0.1), inset 0 0 30px rgba(0,180,255,0.03);
  }
  .panel::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--blue), transparent);
  }

  .panel-header {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 14px 20px;
    border-bottom: 1px solid var(--border);
    font-size: 11px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--blue);
  }
  .panel-header .icon { opacity: 0.7; font-size: 14px; }
  .panel-id {
    margin-left: auto;
    font-size: 9px;
    color: var(--text-dim);
    letter-spacing: 1px;
  }

  .panel-body { padding: 24px; }

  /* ── GRID ── */
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
  @media(max-width:600px) { .grid-2,.grid-3 { grid-template-columns:1fr; } }

  /* ── TERMINAL ── */
  .terminal {
    background: #010a12;
    border: 1px solid var(--border);
    border-radius: 2px;
    overflow: hidden;
    font-size: 13px;
    line-height: 1.8;
  }
  .terminal-bar {
    background: #0a1a28;
    padding: 8px 16px;
    display: flex;
    align-items: center;
    gap: 8px;
    border-bottom: 1px solid var(--border);
  }
  .t-btn { width:10px;height:10px;border-radius:50%; }
  .t-btn.r{background:#ff3b5c;} .t-btn.y{background:#ffaa00;} .t-btn.g{background:#00ff88;}
  .terminal-title { margin-left: auto; font-size:10px; color:var(--text-dim); letter-spacing:2px; }
  .terminal-body { padding: 20px; }
  .t-line { display: flex; gap: 8px; margin-bottom: 2px; }
  .t-prompt { color: var(--green); flex-shrink: 0; }
  .t-cmd { color: var(--blue); }
  .t-out { color: var(--text); padding-left: 16px; }
  .t-key { color: var(--text-dim); }
  .t-val { color: #fff; }
  .t-bar-wrap { display:flex; align-items:center; gap:12px; padding-left:16px; margin: 4px 0; }
  .t-bar-label { color:var(--text-dim); font-size:11px; min-width:90px; }
  .t-bar-track { flex:1; height:4px; background:rgba(0,180,255,0.1); border-radius:2px; overflow:hidden; }
  .t-bar-fill { height:100%; background: linear-gradient(90deg, var(--blue-dim), var(--blue)); border-radius:2px; transition: width 1.5s ease; }
  .t-bar-pct { color:var(--blue); font-size:10px; min-width:36px; text-align:right; }

  /* ── SKILLS ── */
  .skill-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; }
  @media(max-width:500px){.skill-grid{grid-template-columns:1fr;}}

  .skill-item {
    padding: 14px 16px;
    border: 1px solid var(--border);
    border-radius: 2px;
    background: rgba(0,180,255,0.03);
    position: relative;
    overflow: hidden;
    transition: all 0.3s;
    cursor: default;
  }
  .skill-item:hover {
    background: rgba(0,180,255,0.08);
    border-color: var(--blue);
    transform: translateY(-2px);
    box-shadow: 0 4px 20px rgba(0,180,255,0.15);
  }
  .skill-item::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0;
    height: 2px;
    width: var(--lvl);
    background: linear-gradient(90deg, var(--blue-dim), var(--blue));
    transition: width 1s ease 0.5s;
  }
  .skill-name { font-size: 12px; letter-spacing: 2px; color: var(--text); text-transform: uppercase; }
  .skill-cat { font-size: 9px; color: var(--text-dim); letter-spacing: 1px; margin-top: 2px; }
  .skill-level { font-size: 10px; color: var(--blue); margin-top: 8px; }

  /* ── STAT CARDS ── */
  .stat-card {
    padding: 20px;
    border: 1px solid var(--border);
    border-radius: 2px;
    background: rgba(0,180,255,0.03);
    text-align: center;
    transition: all 0.3s;
  }
  .stat-card:hover {
    background: rgba(0,180,255,0.08);
    border-color: var(--blue);
    box-shadow: 0 0 20px rgba(0,180,255,0.1);
  }
  .stat-value {
    font-family: 'Orbitron', monospace;
    font-size: 32px;
    font-weight: 700;
    color: var(--blue);
    text-shadow: 0 0 20px var(--blue-glow);
  }
  .stat-label { font-size: 9px; letter-spacing: 3px; color: var(--text-dim); text-transform: uppercase; margin-top: 6px; }

  /* ── ALERT FEED ── */
  .alert-feed { display: flex; flex-direction: column; gap: 8px; }
  .alert-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 14px;
    border: 1px solid var(--border);
    border-radius: 2px;
    font-size: 11px;
    letter-spacing: 1px;
    background: rgba(0,180,255,0.02);
    animation: slideIn 0.5s ease backwards;
  }
  .alert-item.high { border-left: 2px solid var(--green); }
  .alert-item.med  { border-left: 2px solid var(--warn); }
  .alert-item.low  { border-left: 2px solid var(--blue); }
  @keyframes slideIn { from{opacity:0;transform:translateX(-10px)} to{opacity:1;transform:none} }

  .alert-badge {
    font-size: 8px; letter-spacing: 2px;
    padding: 2px 6px;
    border-radius: 1px;
    flex-shrink: 0;
  }
  .badge-high { background: rgba(0,255,136,0.15); color: var(--green); border: 1px solid var(--green); }
  .badge-med  { background: rgba(255,170,0,0.15); color: var(--warn); border: 1px solid var(--warn); }
  .badge-low  { background: rgba(0,180,255,0.15); color: var(--blue); border: 1px solid var(--blue); }

  .alert-text { flex: 1; color: var(--text); }
  .alert-time { color: var(--text-dim); font-size: 9px; flex-shrink: 0; }

  /* ── PROJECT PLACEHOLDER ── */
  .project-placeholder {
    border: 1px dashed rgba(0,180,255,0.2);
    border-radius: 2px;
    padding: 32px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .project-placeholder::before {
    content: '';
    position: absolute;
    inset: 0;
    background: repeating-linear-gradient(
      45deg,
      transparent,
      transparent 10px,
      rgba(0,180,255,0.02) 10px,
      rgba(0,180,255,0.02) 20px
    );
  }
  .project-placeholder-icon { font-size: 36px; margin-bottom: 12px; }
  .project-placeholder-title {
    font-family: 'Orbitron', monospace;
    font-size: 14px;
    color: var(--blue);
    letter-spacing: 3px;
    margin-bottom: 8px;
  }
  .project-placeholder-text { font-size: 11px; color: var(--text-dim); letter-spacing: 1px; line-height: 1.8; }

  /* ── CONTACT ── */
  .contact-link {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 16px 20px;
    border: 1px solid var(--border);
    border-radius: 2px;
    text-decoration: none;
    color: var(--text);
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
    background: rgba(0,180,255,0.02);
  }
  .contact-link:hover {
    border-color: var(--blue);
    background: rgba(0,180,255,0.08);
    transform: translateX(4px);
    box-shadow: -4px 0 0 var(--blue);
  }
  .contact-link::after {
    content: '→';
    margin-left: auto;
    color: var(--blue);
    opacity: 0;
    transition: opacity 0.3s;
  }
  .contact-link:hover::after { opacity: 1; }
  .contact-icon { font-size: 20px; }
  .contact-info { display: flex; flex-direction: column; }
  .contact-platform { font-size: 10px; letter-spacing: 3px; color: var(--text-dim); text-transform: uppercase; }
  .contact-handle { font-size: 14px; color: var(--blue); letter-spacing: 1px; margin-top: 2px; }

  /* ── FOOTER ── */
  .footer {
    text-align: center;
    margin-top: 60px;
    padding-top: 24px;
    border-top: 1px solid var(--border);
    font-size: 10px;
    letter-spacing: 3px;
    color: var(--text-dim);
    text-transform: uppercase;
  }

  /* ── SECTION SPACING ── */
  section { margin-bottom: 20px; }

  /* ── LIVE CLOCK ── */
  .clock {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    color: var(--blue);
    letter-spacing: 2px;
  }

  /* ── PROGRESS RING ── */
  .ring-container { display: flex; justify-content: center; align-items: center; gap: 40px; flex-wrap: wrap; padding: 8px 0; }
  .ring-item { text-align: center; }
  svg.ring { transform: rotate(-90deg); }
  .ring-track { fill: none; stroke: rgba(0,180,255,0.1); stroke-width: 4; }
  .ring-fill { fill: none; stroke: var(--blue); stroke-width: 4; stroke-linecap: round;
    stroke-dasharray: 100; stroke-dashoffset: 100;
    filter: drop-shadow(0 0 6px var(--blue));
    transition: stroke-dashoffset 2s ease 0.5s; }
  .ring-label { font-size: 9px; letter-spacing: 2px; color: var(--text-dim); margin-top: 8px; text-transform: uppercase; }
  .ring-pct { font-family: 'Orbitron', monospace; font-size: 14px; color: var(--blue); margin-top: 4px; }

  /* fade-in on load */
  .fade-in { opacity: 0; transform: translateY(20px); animation: fadeUp 0.7s ease forwards; }
  @keyframes fadeUp { to{opacity:1;transform:none} }
  .delay-1{animation-delay:0.1s} .delay-2{animation-delay:0.25s} .delay-3{animation-delay:0.4s}
  .delay-4{animation-delay:0.55s} .delay-5{animation-delay:0.7s} .delay-6{animation-delay:0.85s}
</style>
</head>
<body>

<canvas id="hexCanvas"></canvas>
<div class="corner-bracket tl"></div>
<div class="corner-bracket tr"></div>
<div class="corner-bracket bl"></div>
<div class="corner-bracket br"></div>

<div class="container">

  <!-- ── HEADER ── -->
  <div class="header fade-in">
    <div class="status-bar">
      <span><span class="dot"></span>SYSTEM ONLINE</span>
      <span><span class="dot warn"></span>THREAT LEVEL: LOW</span>
      <span class="clock" id="clock">00:00:00 UTC</span>
    </div>

    <div class="name-display" data-text="JACKSONLIND">JACKSONLIND</div>
    <div class="tagline-wrapper">
      <div class="tagline"><span id="tagline-text"></span><span class="cursor"></span></div>
    </div>

    <div class="divider"><div class="divider-icon">◈ INITIALIZING PROFILE ◈</div></div>
  </div>

  <!-- ── WHOAMI TERMINAL ── -->
  <section class="fade-in delay-1">
    <div class="terminal">
      <div class="terminal-bar">
        <div class="t-btn r"></div><div class="t-btn y"></div><div class="t-btn g"></div>
        <div class="terminal-title">bash — jackson@blueteam: ~</div>
      </div>
      <div class="terminal-body">
        <div class="t-line"><span class="t-prompt">jackson@blueteam:~$</span><span class="t-cmd"> cat /etc/operator.conf</span></div>
        <br>
        <div class="t-line"><span class="t-out"><span class="t-key">HANDLE    </span> <span class="t-val">JacksonLind</span></span></div>
        <div class="t-line"><span class="t-out"><span class="t-key">ROLE      </span> <span class="t-val">SOC Analyst [Entry Level] — Blue Team Operations</span></span></div>
        <div class="t-line"><span class="t-out"><span class="t-key">HOBBY     </span> <span class="t-val">Full Time CTF Player</span></span></div>
        <div class="t-line"><span class="t-out"><span class="t-key">FOCUS     </span> <span class="t-val">Threat Detection | Incident Response | Log Analysis</span></span></div>
        <div class="t-line"><span class="t-out"><span class="t-key">STATUS    </span> <span style="color:var(--green)">● ACTIVELY LEARNING</span></span></div>
        <br>
        <div class="t-line"><span class="t-prompt">jackson@blueteam:~$</span><span class="t-cmd"> ./skill_assessment.sh</span></div>
        <br>
        <div class="t-bar-wrap">
          <span class="t-bar-label">Wireshark</span>
          <div class="t-bar-track"><div class="t-bar-fill" style="width:0%" data-width="78%"></div></div>
          <span class="t-bar-pct">78%</span>
        </div>
        <div class="t-bar-wrap">
          <span class="t-bar-label">Python</span>
          <div class="t-bar-track"><div class="t-bar-fill" style="width:0%" data-width="65%"></div></div>
          <span class="t-bar-pct">65%</span>
        </div>
        <div class="t-bar-wrap">
          <span class="t-bar-label">SIEM</span>
          <div class="t-bar-track"><div class="t-bar-fill" style="width:0%" data-width="55%"></div></div>
          <span class="t-bar-pct">55%</span>
        </div>
        <div class="t-bar-wrap">
          <span class="t-bar-label">Java</span>
          <div class="t-bar-track"><div class="t-bar-fill" style="width:0%" data-width="60%"></div></div>
          <span class="t-bar-pct">60%</span>
        </div>
        <br>
        <div class="t-line"><span style="color:var(--green)">jackson@blueteam:~$</span><span class="t-cmd"> <span id="blink-cursor" style="display:inline-block;width:8px;height:13px;background:var(--blue);vertical-align:middle;animation:blink 0.8s step-end infinite;margin-left:4px;"></span></span></div>
      </div>
    </div>
  </section>

  <!-- ── THREAT STATS ── -->
  <section class="fade-in delay-2">
    <div class="panel">
      <div class="panel-header"><span class="icon">◉</span> OPERATOR METRICS <span class="panel-id">SYS::METRICS_01</span></div>
      <div class="panel-body">
        <div class="grid-3">
          <div class="stat-card">
            <div class="stat-value" data-target="47">0</div>
            <div class="stat-label">CTFs Played</div>
          </div>
          <div class="stat-card">
            <div class="stat-value" data-target="312">0</div>
            <div class="stat-label">Alerts Triaged</div>
          </div>
          <div class="stat-card">
            <div class="stat-value" data-target="4">0</div>
            <div class="stat-label">Tools Mastered</div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ── SKILL RINGS + CARDS ── -->
  <section class="fade-in delay-3">
    <div class="panel">
      <div class="panel-header"><span class="icon">◈</span> CAPABILITY MATRIX <span class="panel-id">SYS::SKILLS_02</span></div>
      <div class="panel-body">
        <div class="ring-container" id="rings">
          <div class="ring-item">
            <svg class="ring" width="80" height="80" viewBox="0 0 36 36">
              <circle class="ring-track" cx="18" cy="18" r="15.9"/>
              <circle class="ring-fill" cx="18" cy="18" r="15.9" style="stroke-dasharray:78 100" data-pct="78"/>
            </svg>
            <div class="ring-label">Wireshark</div>
            <div class="ring-pct">78%</div>
          </div>
          <div class="ring-item">
            <svg class="ring" width="80" height="80" viewBox="0 0 36 36">
              <circle class="ring-track" cx="18" cy="18" r="15.9"/>
              <circle class="ring-fill" cx="18" cy="18" r="15.9" data-pct="65"/>
            </svg>
            <div class="ring-label">Python</div>
            <div class="ring-pct">65%</div>
          </div>
          <div class="ring-item">
            <svg class="ring" width="80" height="80" viewBox="0 0 36 36">
              <circle class="ring-track" cx="18" cy="18" r="15.9"/>
              <circle class="ring-fill" cx="18" cy="18" r="15.9" data-pct="55"/>
            </svg>
            <div class="ring-label">SIEM</div>
            <div class="ring-pct">55%</div>
          </div>
          <div class="ring-item">
            <svg class="ring" width="80" height="80" viewBox="0 0 36 36">
              <circle class="ring-track" cx="18" cy="18" r="15.9"/>
              <circle class="ring-fill" cx="18" cy="18" r="15.9" data-pct="60"/>
            </svg>
            <div class="ring-label">Java</div>
            <div class="ring-pct">60%</div>
          </div>
        </div>
        <br>
        <div class="skill-grid">
          <div class="skill-item" style="--lvl:78%">
            <div class="skill-name">Wireshark</div>
            <div class="skill-cat">Network Packet Analysis</div>
            <div class="skill-level">▰▰▰▰▰▰▰▱▱▱ PROFICIENT</div>
          </div>
          <div class="skill-item" style="--lvl:65%">
            <div class="skill-name">Python</div>
            <div class="skill-cat">Scripting / Automation</div>
            <div class="skill-level">▰▰▰▰▰▰▱▱▱▱ INTERMEDIATE</div>
          </div>
          <div class="skill-item" style="--lvl:55%">
            <div class="skill-name">SIEM</div>
            <div class="skill-cat">Log Monitoring / Correlation</div>
            <div class="skill-level">▰▰▰▰▰▱▱▱▱▱ DEVELOPING</div>
          </div>
          <div class="skill-item" style="--lvl:60%">
            <div class="skill-name">Java</div>
            <div class="skill-cat">Application Development</div>
            <div class="skill-level">▰▰▰▰▰▰▱▱▱▱ INTERMEDIATE</div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ── LIVE ALERT FEED ── -->
  <section class="fade-in delay-3">
    <div class="panel">
      <div class="panel-header"><span class="icon">⚡</span> SIMULATED ALERT FEED <span class="panel-id">SYS::SIEM_FEED</span></div>
      <div class="panel-body">
        <div class="alert-feed" id="alertFeed">
          <div class="alert-item high" style="animation-delay:0.1s">
            <span class="alert-badge badge-high">CLEARED</span>
            <span class="alert-text">Brute-force SSH detected on 192.168.1.44 — blocked at perimeter</span>
            <span class="alert-time">00:04:12</span>
          </div>
          <div class="alert-item med" style="animation-delay:0.2s">
            <span class="alert-badge badge-med">REVIEW</span>
            <span class="alert-text">Anomalous DNS query volume from internal host — flagged for analysis</span>
            <span class="alert-time">00:11:37</span>
          </div>
          <div class="alert-item low" style="animation-delay:0.3s">
            <span class="alert-badge badge-low">INFO</span>
            <span class="alert-text">Port scan sweep detected from 10.0.0.89 — logged</span>
            <span class="alert-time">00:23:55</span>
          </div>
          <div class="alert-item high" style="animation-delay:0.4s">
            <span class="alert-badge badge-high">CLOSED</span>
            <span class="alert-text">Privilege escalation attempt caught by EDR on WIN-WS04</span>
            <span class="alert-time">00:41:09</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ── PROJECTS ── -->
  <section class="fade-in delay-4">
    <div class="panel">
      <div class="panel-header"><span class="icon">◧</span> PROJECTS // ARSENAL <span class="panel-id">SYS::PROJ_03</span></div>
      <div class="panel-body">
        <div class="project-placeholder">
          <div class="project-placeholder-icon">🔐</div>
          <div class="project-placeholder-title">// CLASSIFIED //</div>
          <div class="project-placeholder-text">
            No active deployments detected.<br>
            Projects currently in development.<br>
            <br>
            <span style="color:var(--blue)">[ CTF writeups & homelab coming soon ]</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ── CONTACT ── -->
  <section class="fade-in delay-5">
    <div class="panel">
      <div class="panel-header"><span class="icon">◎</span> ESTABLISH CONNECTION <span class="panel-id">SYS::CONTACT_04</span></div>
      <div class="panel-body" style="display:flex;flex-direction:column;gap:10px;">
        <a href="https://www.linkedin.com/in/jackson-lind/" target="_blank" class="contact-link">
          <span class="contact-icon">💼</span>
          <div class="contact-info">
            <span class="contact-platform">LinkedIn</span>
            <span class="contact-handle">jackson-lind</span>
          </div>
        </a>
      </div>
    </div>
  </section>

  <!-- ── FOOTER ── -->
  <div class="footer fade-in delay-6">
    <div>◈ &nbsp; JacksonLind &nbsp; // &nbsp; Blue Team Operator &nbsp; // &nbsp; SOC Analyst &nbsp; ◈</div>
    <div style="margin-top:8px;font-size:9px;opacity:0.5;">"The quieter you become, the more you are able to hear." — Kali Linux</div>
  </div>

</div>

<script>
// ── HEX GRID CANVAS ──
const canvas = document.getElementById('hexCanvas');
const ctx = canvas.getContext('2d');
let hexes = [];

function resize() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  buildHexes();
}

function buildHexes() {
  hexes = [];
  const size = 28, w = size * 2, h = Math.sqrt(3) * size;
  for(let row = -1; row < canvas.height/h + 2; row++) {
    for(let col = -1; col < canvas.width/w + 2; col++) {
      const x = col * w * 0.75 + (row % 2 === 0 ? 0 : w * 0.375);
      const y = row * h;
      hexes.push({ x, y, size, alpha: Math.random() * 0.3, phase: Math.random() * Math.PI * 2 });
    }
  }
}

function drawHex(x, y, size, alpha) {
  ctx.beginPath();
  for(let i = 0; i < 6; i++) {
    const a = Math.PI / 180 * (60 * i);
    ctx[i===0?'moveTo':'lineTo'](x + size * Math.cos(a), y + size * Math.sin(a));
  }
  ctx.closePath();
  ctx.strokeStyle = `rgba(0,180,255,${alpha})`;
  ctx.lineWidth = 0.5;
  ctx.stroke();
}

let frame = 0;
function animateHex() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  frame += 0.008;
  hexes.forEach(h => {
    const a = h.alpha * (0.5 + 0.5 * Math.sin(frame + h.phase));
    drawHex(h.x, h.y, h.size, a);
  });
  requestAnimationFrame(animateHex);
}

resize();
animateHex();
window.addEventListener('resize', resize);

// ── CLOCK ──
function updateClock() {
  const now = new Date();
  const h = String(now.getUTCHours()).padStart(2,'0');
  const m = String(now.getUTCMinutes()).padStart(2,'0');
  const s = String(now.getUTCSeconds()).padStart(2,'0');
  document.getElementById('clock').textContent = `${h}:${m}:${s} UTC`;
}
setInterval(updateClock, 1000); updateClock();

// ── TAGLINE TYPER ──
const lines = [
  'Entry Level SOC Analyst',
  'Full Time CTF Player',
  'Blue Team Defender',
  'Threat Hunter in Training',
];
let li = 0, ci = 0, deleting = false;
const el = document.getElementById('tagline-text');
function typeLoop() {
  const word = lines[li];
  if(!deleting) {
    el.textContent = word.slice(0, ++ci);
    if(ci === word.length) { deleting = true; setTimeout(typeLoop, 1800); return; }
  } else {
    el.textContent = word.slice(0, --ci);
    if(ci === 0) { deleting = false; li = (li+1)%lines.length; }
  }
  setTimeout(typeLoop, deleting ? 40 : 80);
}
typeLoop();

// ── COUNTER ANIMATION ──
function animateCounters() {
  document.querySelectorAll('.stat-value[data-target]').forEach(el => {
    const target = parseInt(el.dataset.target);
    let current = 0;
    const step = Math.ceil(target / 40);
    const timer = setInterval(() => {
      current = Math.min(current + step, target);
      el.textContent = current;
      if(current >= target) clearInterval(timer);
    }, 40);
  });
}
setTimeout(animateCounters, 800);

// ── SKILL BARS ──
setTimeout(() => {
  document.querySelectorAll('.t-bar-fill[data-width]').forEach(el => {
    el.style.width = el.dataset.width;
  });
}, 600);

// ── RING ANIMATIONS ──
setTimeout(() => {
  document.querySelectorAll('.ring-fill[data-pct]').forEach(el => {
    const pct = parseInt(el.dataset.pct);
    el.style.strokeDasharray = `${pct} 100`;
    el.style.strokeDashoffset = '0';
  });
}, 800);

// ── LIVE ALERT INJECTOR ──
const alertTemplates = [
  { level:'med', badge:'REVIEW', text:'TLS certificate anomaly on external endpoint — monitoring', time: null },
  { level:'low', badge:'INFO',   text:'New endpoint enrolled in MDM — baseline recorded', time: null },
  { level:'high',badge:'CLOSED', text:'Ransomware signature match quarantined by AV on WS-09', time: null },
  { level:'med', badge:'REVIEW', text:'Lateral movement indicator flagged in SIEM correlation rule', time: null },
];

let alertIdx = 0;
function injectAlert() {
  const feed = document.getElementById('alertFeed');
  const t = alertTemplates[alertIdx % alertTemplates.length];
  alertIdx++;
  const now = new Date();
  const ts = `${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}:${String(now.getSeconds()).padStart(2,'0')}`;
  const el = document.createElement('div');
  el.className = `alert-item ${t.level}`;
  el.innerHTML = `<span class="alert-badge badge-${t.level}">${t.badge}</span><span class="alert-text">${t.text}</span><span class="alert-time">${ts}</span>`;
  el.style.opacity = '0'; el.style.transform = 'translateX(-10px)';
  feed.prepend(el);
  requestAnimationFrame(() => { el.style.transition='all 0.4s'; el.style.opacity='1'; el.style.transform='none'; });
  if(feed.children.length > 6) feed.removeChild(feed.lastChild);
}
setInterval(injectAlert, 5000);
</script>
</body>
</html>
