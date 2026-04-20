<!DOCTYPE html>

<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Attrape Noé 💕</title>
<style>
  html, body {
    margin: 0 !important;
    padding: 0 !important;
    width: 100% !important;
    height: 100% !important;
    overflow: hidden !important;
    background: #1C1028 !important;
  }
  #root {
    position: fixed !important;
    inset: 0 !important;
    z-index: 99999 !important;
  }
</style>
</head>
<body>
<div id="root"></div>
<script>
// ── Mount everything inside a Shadow DOM so tiiny.site can't touch it ──
const root = document.getElementById('root');
const shadow = root.attachShadow({ mode: 'open' });

shadow.innerHTML = `

<style>
@import url('https://fonts.googleapis.com/css2?family=Pacifico&family=Quicksand:wght@500;600;700&display=swap');

*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }

:host {
  display:block;
  width:100%;
  height:100%;
  font-family:'Quicksand',sans-serif;
  background:#1C1028;
  overflow:hidden;
  cursor:none;
  user-select:none;
  -webkit-user-select:none;
  color:white;
}

#cursor {
  position:fixed; z-index:9999; pointer-events:none;
  font-size:2rem; line-height:1;
  left:-200px; top:-200px;
  transform:translate(-50%,-50%);
  filter:drop-shadow(0 0 8px rgba(248,113,113,0.9));
}

#starfield { position:fixed; inset:0; z-index:0; pointer-events:none; }

/* ─── SCREENS ─── */
.screen {
  position:fixed;
  inset:0;
  z-index:10;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  padding:16px;
  display:none;
}
.screen.active { display:flex; }

/* ─── TITLE ─── */
#s-title {
  background:radial-gradient(ellipse at 50% 0%,#2D1B4E,#1C1028 70%);
  gap:10px;
  overflow-y:auto;
}
.badge {
  background:linear-gradient(135deg,#F87171,#FB923C);
  color:#fff; font-size:0.72rem; font-weight:700;
  letter-spacing:2px; text-transform:uppercase;
  padding:5px 16px; border-radius:99px;
}
.game-title {
  font-family:'Pacifico',cursive;
  font-size:clamp(2.6rem,9vw,5rem);
  background:linear-gradient(135deg,#FF9A9E,#FAD0C4,#FFD1FF);
  -webkit-background-clip:text; -webkit-text-fill-color:transparent;
  background-clip:text; line-height:1.1; text-align:center;
}
.subtitle { font-size:0.95rem; color:#9B8AAB; font-weight:600; letter-spacing:1px; }

.rules-card {
  background:rgba(255,255,255,0.05);
  border:1.5px solid rgba(255,255,255,0.1);
  border-radius:20px; padding:18px 22px;
  width:100%; max-width:400px; backdrop-filter:blur(10px);
}
.rlabel { font-size:0.65rem; font-weight:700; letter-spacing:2.5px; text-transform:uppercase; color:#F87171; margin-bottom:12px; }
.rule { display:flex; align-items:center; gap:12px; padding:7px 0; border-bottom:1px solid rgba(255,255,255,0.06); }
.rule:last-child { border-bottom:none; }
.ricon { width:36px; height:36px; border-radius:10px; flex-shrink:0; display:flex; align-items:center; justify-content:center; font-size:1.15rem; }
.ricon.g { background:rgba(52,211,153,0.12); border:1.5px solid rgba(52,211,153,0.3); }
.ricon.b { background:rgba(248,113,113,0.12); border:1.5px solid rgba(248,113,113,0.3); }
.ricon.m { background:rgba(245,158,11,0.12);  border:1.5px solid rgba(245,158,11,0.3); }
.ricon.t { background:rgba(167,139,250,0.12); border:1.5px solid rgba(167,139,250,0.3); }
.rtext strong { display:block; font-size:0.88rem; color:rgba(255,255,255,0.9); }
.rtext span   { font-size:0.74rem; color:#9B8AAB; }

.chips { display:flex; flex-wrap:wrap; gap:7px; justify-content:center; max-width:400px; }
.chip {
  background:rgba(255,255,255,0.06); border:1px solid rgba(255,255,255,0.1);
  border-radius:99px; padding:4px 11px; font-size:0.75rem;
  color:rgba(255,255,255,0.55); font-weight:700; display:flex; align-items:center; gap:5px;
}
.chip b { color:#F59E0B; }

.btn-play {
  font-family:'Pacifico',cursive; font-size:1.25rem;
  background:linear-gradient(135deg,#F87171,#FB923C);
  color:#fff; border:none; border-radius:99px; padding:15px 50px; cursor:pointer;
  animation:ppulse 2.5s ease infinite;
}
.btn-play:active { transform:scale(0.95); }
@keyframes ppulse {
  0%,100%{box-shadow:0 8px 24px rgba(248,113,113,0.45),0 0 0 0 rgba(248,113,113,0.3);}
  50%{box-shadow:0 8px 32px rgba(248,113,113,0.6),0 0 0 12px rgba(248,113,113,0);}
}

/* ─── GAME ─── */
#s-game { padding:0; justify-content:flex-start; }

#hud {
  width:100%; display:flex; align-items:center; justify-content:space-between;
  padding:10px 14px; gap:10px; flex-shrink:0;
  background:rgba(28,16,40,0.97); border-bottom:1px solid rgba(255,255,255,0.08);
}
.hpill {
  background:rgba(255,255,255,0.07); border:1px solid rgba(255,255,255,0.12);
  border-radius:99px; padding:5px 14px;
  font-family:'Pacifico',cursive; font-size:1rem; color:#fff;
  display:flex; align-items:center; gap:7px; min-width:70px; justify-content:center;
}
.hpill span { color:#F87171; }
#tw { flex:1; display:flex; flex-direction:column; align-items:center; gap:4px; }
#tlabel { font-size:0.6rem; font-weight:700; letter-spacing:1.5px; text-transform:uppercase; color:#9B8AAB; }
#ttrack { width:100%; max-width:180px; height:7px; background:rgba(255,255,255,0.08); border-radius:99px; overflow:hidden; }
#tfill  { height:100%; background:linear-gradient(90deg,#F87171,#FB923C); border-radius:99px; transition:width 0.1s linear; width:100%; }
#nxt    { font-size:0.6rem; font-weight:700; color:#F59E0B; white-space:nowrap; }

#game-area { flex:1; width:100%; display:flex; flex-direction:row; min-height:0; }

#rwd-panel {
  width:50px; flex-shrink:0; display:flex; flex-direction:column;
  align-items:center; padding:10px 0; gap:5px;
  background:rgba(28,16,40,0.7); border-right:1px solid rgba(255,255,255,0.07); overflow:hidden;
}
.ri {
  position:relative; width:36px; height:36px; border-radius:10px; flex-shrink:0;
  display:flex; align-items:center; justify-content:center; font-size:1.2rem;
  background:rgba(255,255,255,0.04); border:1.5px solid rgba(255,255,255,0.08); transition:all 0.4s;
}
.ri.locked { filter:grayscale(1); opacity:0.3; }
.ri.next   { opacity:0.6; border-color:rgba(245,158,11,0.4); border-style:dashed; }
.ri.unlocked { background:rgba(245,158,11,0.15); border-color:#F59E0B; opacity:1; filter:none; box-shadow:0 0 12px rgba(245,158,11,0.35); animation:upop 0.5s ease; }
@keyframes upop { 0%{transform:scale(0.4)} 60%{transform:scale(1.2)} 100%{transform:scale(1)} }
.ri .rscore {
  position:absolute; bottom:-5px; left:50%; transform:translateX(-50%);
  background:#1C1028; border:1px solid rgba(255,255,255,0.15); border-radius:99px;
  font-size:0.46rem; font-weight:700; color:#F59E0B; padding:1px 3px; white-space:nowrap;
}

#gc { flex:1; display:block; min-width:0; }

/* combo */
#combo {
  position:fixed; bottom:18px; left:50%; transform:translateX(-50%);
  z-index:30; display:flex; align-items:center; gap:8px;
  background:rgba(28,16,40,0.9); border:1px solid rgba(255,255,255,0.1);
  border-radius:99px; padding:7px 16px;
  font-family:'Pacifico',cursive; font-size:1rem; color:#F59E0B;
  opacity:0; transition:opacity 0.3s;
}
#combo.on { opacity:1; }
#cdots { display:flex; gap:5px; }
.cdot { width:9px; height:9px; border-radius:99px; background:rgba(255,255,255,0.15); transition:background 0.2s; }
.cdot.on { background:#F59E0B; box-shadow:0 0 6px #F59E0B; }

/* toast */
#toast {
  position:fixed; top:74px; left:50%;
  transform:translateX(-50%) translateY(-14px);
  z-index:400; opacity:0; pointer-events:none;
  background:rgba(28,16,40,0.92); border:1.5px solid rgba(245,158,11,0.45);
  border-radius:14px; padding:10px 20px;
  display:flex; align-items:center; gap:12px; white-space:nowrap;
  transition:all 0.35s ease;
}
#toast.on { opacity:1; transform:translateX(-50%) translateY(0); }
#te { font-size:1.7rem; }
.tl { font-size:0.6rem; font-weight:700; letter-spacing:2px; text-transform:uppercase; color:#F59E0B; }
.tn { font-size:0.9rem; font-weight:700; color:#fff; }

/* float */
.ft {
  position:fixed; pointer-events:none; z-index:500;
  font-family:'Pacifico',cursive; font-size:1.3rem;
  animation:ftup 0.85s ease forwards; white-space:nowrap;
  text-shadow:0 2px 8px rgba(0,0,0,0.6);
}
@keyframes ftup {
  0%  { opacity:1; transform:translateY(0) scale(1); }
  80% { opacity:1; transform:translateY(-50px) scale(1.08); }
  100%{ opacity:0; transform:translateY(-64px) scale(1); }
}

/* ─── END ─── */
#s-end {
  background:radial-gradient(ellipse at 50% 30%,#2D1B4E,#1C1028 70%);
  gap:14px; overflow-y:auto;
}
.etrophy { font-size:4.5rem; animation:trop 0.6s ease; line-height:1; }
@keyframes trop { 0%{transform:scale(0) rotate(-20deg);opacity:0} 70%{transform:scale(1.15) rotate(5deg);opacity:1} 100%{transform:scale(1) rotate(0)} }
.etitle {
  font-family:'Pacifico',cursive; font-size:clamp(1.7rem,6vw,2.8rem); text-align:center;
  background:linear-gradient(135deg,#FF9A9E,#FAD0C4);
  -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text;
}
.sbox { display:flex; align-items:center; gap:12px; background:rgba(255,255,255,0.05); border:1px solid rgba(255,255,255,0.1); border-radius:18px; padding:14px 26px; }
.sbig { font-family:'Pacifico',cursive; font-size:3.2rem; color:#F87171; line-height:1; }
.ssub { font-size:0.82rem; color:#9B8AAB; font-weight:600; line-height:1.4; }
.emsg {
  background:rgba(248,113,113,0.07); border:1.5px solid rgba(248,113,113,0.2);
  border-radius:16px; padding:14px 20px; max-width:360px;
  font-size:0.92rem; color:rgba(255,255,255,0.85); line-height:1.6; text-align:center; font-weight:500;
}
.rtitle { font-size:0.65rem; font-weight:700; letter-spacing:2px; text-transform:uppercase; color:#F59E0B; width:100%; max-width:400px; }
.rgrid { display:grid; grid-template-columns:repeat(4,1fr); gap:8px; width:100%; max-width:400px; }
.rcard { background:rgba(255,255,255,0.04); border:1.5px solid rgba(255,255,255,0.08); border-radius:14px; padding:10px 6px; text-align:center; position:relative; }
.rcard.got { background:rgba(245,158,11,0.09); border-color:rgba(245,158,11,0.35); box-shadow:0 0 18px rgba(245,158,11,0.12); }
.rcard.got::before { content:'✓'; position:absolute; top:4px; right:7px; font-size:0.58rem; font-weight:700; color:#F59E0B; }
.rcard.nope { filter:grayscale(0.8); opacity:0.35; }
.rcard .re { font-size:1.7rem; display:block; margin-bottom:3px; }
.rcard .rn { font-size:0.6rem; font-weight:700; color:rgba(255,255,255,0.7); line-height:1.2; }
.rcard .rp { font-size:0.55rem; color:#F59E0B; font-weight:700; margin-top:2px; }
.btn-replay {
  font-family:'Pacifico',cursive; font-size:1rem;
  background:rgba(255,255,255,0.07); color:#fff;
  border:1.5px solid rgba(255,255,255,0.15); border-radius:99px;
  padding:12px 36px; cursor:pointer;
}

/* heart particles */
.hp { position:fixed; pointer-events:none; z-index:5; font-size:1.1rem; animation:hf linear forwards; }
@keyframes hf { 0%{transform:translateY(-20px) rotate(0);opacity:1} 100%{transform:translateY(110vh) rotate(360deg);opacity:0} }
</style>

<canvas id="starfield"></canvas>

<div id="cursor">💕</div>

<div id="combo" class=""><span>Combo</span><div id="cdots"><div class="cdot" id="cd0"></div><div class="cdot" id="cd1"></div><div class="cdot" id="cd2"></div><div class="cdot" id="cd3"></div><div class="cdot" id="cd4"></div></div></div>

<div id="toast"><div id="te">🍽️</div><div><div class="tl">Récompense débloquée 🎉</div><div class="tn" id="tname">Restaurant</div></div></div>

<!-- TITLE -->

<div id="s-title" class="screen active">
  <div class="badge">🌸 Un cadeau de Noé</div>
  <h1 class="game-title">Attrape Noé !</h1>
  <p class="subtitle">fait avec amour pour Alice 💕</p>
  <div class="rules-card">
    <div class="rlabel">📖 Comment jouer</div>
    <div class="rule"><div class="ricon g">🧑‍🦱</div><div class="rtext"><strong>Attrape les Noés !</strong><span>Passe ta souris / ton doigt dessus</span></div></div>
    <div class="rule"><div class="ricon b">👻</div><div class="rtext"><strong>Évite les imposteurs</strong><span>Les toucher = perdre une vie 💔</span></div></div>
    <div class="rule"><div class="ricon m">⬇️</div><div class="rtext"><strong>Ne les laisse pas tomber</strong><span>Un Noé au sol = vie perdue aussi</span></div></div>
    <div class="rule"><div class="ricon t">⏳</div><div class="rtext"><strong>40 sec · 3 vies</strong><span>Cumule des points pour débloquer des récompenses !</span></div></div>
  </div>
  <div class="chips">
    <div class="chip">🍽️ <b>10pts</b></div>
    <div class="chip">💐 <b>18pts</b></div>
    <div class="chip">🐶 <b>27pts</b></div>
    <div class="chip">🏠 <b>37pts</b></div>
    <div class="chip">✈️ <b>48pts</b></div>
    <div class="chip">💍 <b>60pts</b></div>
    <div class="chip">🌴 <b>75pts</b></div>
  </div>
  <button class="btn-play" id="btn-start">Jouer 🌸</button>
</div>

<!-- GAME -->

<div id="s-game" class="screen">
  <div id="hud">
    <div class="hpill">❤️ <span id="hs">0</span></div>
    <div id="tw"><div id="tlabel">Temps restant</div><div id="ttrack"><div id="tfill"></div></div><div id="nxt"></div></div>
    <div class="hpill">💔 <span id="hl">3</span></div>
  </div>
  <div id="game-area">
    <div id="rwd-panel"></div>
    <canvas id="gc"></canvas>
  </div>
</div>

<!-- END -->

<div id="s-end" class="screen">
  <div class="etrophy" id="etrophy">🏆</div>
  <h2 class="etitle" id="etitle">Bravo Alice !</h2>
  <div class="sbox"><div class="sbig" id="escore">0</div><div class="ssub">Noés<br>attrapés 💖</div></div>
  <div class="emsg" id="emsg"></div>
  <div class="rtitle">🎁 Tes récompenses</div>
  <div class="rgrid" id="erwd"></div>
  <button class="btn-replay" id="btn-replay">Rejouer 🔁</button>
</div>
`;

// ── JS runs inside shadow, gets elements from shadow ──
const $ = id => shadow.getElementById(id);
const $$ = sel => shadow.querySelectorAll(sel);

// STARFIELD
const sf = $(‘starfield’), sc = sf.getContext(‘2d’);
let stars = [];
function initSF() {
sf.width = innerWidth; sf.height = innerHeight;
stars = Array.from({length:100}, () => ({
x: Math.random()*sf.width, y: Math.random()*sf.height,
r: Math.random()*1.4+0.3, a: Math.random(), da: (Math.random()-0.5)*0.009
}));
}
function drawSF() {
sc.clearRect(0,0,sf.width,sf.height);
stars.forEach(s => {
s.a += s.da;
if(s.a<0.08||s.a>1) s.da*=-1;
sc.beginPath(); sc.arc(s.x,s.y,s.r,0,Math.PI*2);
sc.fillStyle=`rgba(255,255,255,${s.a})`; sc.fill();
});
requestAnimationFrame(drawSF);
}
initSF(); drawSF();
window.addEventListener(‘resize’, initSF);

// CURSOR
const cur = $(‘cursor’);
let mx=-300, my=-300;
function mv(x,y){ mx=x; my=y; cur.style.left=x+‘px’; cur.style.top=y+‘px’; }
document.addEventListener(‘mousemove’, e => mv(e.clientX, e.clientY));
document.addEventListener(‘touchmove’, e => { e.preventDefault(); mv(e.touches[0].clientX, e.touches[0].clientY); }, {passive:false});
document.addEventListener(‘touchstart’, e => mv(e.touches[0].clientX, e.touches[0].clientY), {passive:false});

// CONSTANTS
const REWARDS = [
{score:10, emoji:‘🍽️’, name:‘Un restaurant’},
{score:18, emoji:‘💐’, name:‘Des fleurs’},
{score:27, emoji:‘🐶’, name:‘Un golden retriever’},
{score:37, emoji:‘🏠’, name:‘Notre maison’},
{score:48, emoji:‘✈️’, name:‘Un voyage’},
{score:60, emoji:‘💍’, name:‘Le mariage’},
{score:75, emoji:‘🌴’, name:‘Costa Rica’},
];
const GOOD = [‘🧑‍🦱’,‘👦’,‘🧒’,‘🙋‍♂️’,‘🤗’,‘😊’,‘🥰’,‘😍’];
const BAD  = [‘👻’,‘🤡’,‘👽’,‘😈’,‘🦠’,‘🐸’,‘🙈’,‘💀’];
const DURATION = 40;

// STATE
let score, lives, items, over, t0, lastSpawn, spawnMs, combo, comboTmr, unlocked, raf;
const cv = $(‘gc’), cx = cv.getContext(‘2d’);

function sizeCv() {
const rect = cv.getBoundingClientRect();
cv.width  = rect.width  || (innerWidth - 50);
cv.height = rect.height || (innerHeight - 57);
}

// SCREEN
function show(id) {
$$(’.screen’).forEach(s => s.classList.remove(‘active’));
$(id).classList.add(‘active’);
}

// PANEL
function buildPanel() {
const p = $(‘rwd-panel’); p.innerHTML = ‘’;
REWARDS.forEach((r,i) => {
const d = document.createElement(‘div’);
d.className=‘ri locked’; d.id=‘ri’+i;
d.innerHTML = r.emoji + ‘<span class="rscore">’+r.score+’</span>’;
p.appendChild(d);
});
}
function updatePanel(s) {
REWARDS.forEach((r,i) => {
const el = shadow.getElementById(‘ri’+i); if(!el) return;
if(s >= r.score) {
if(!el.classList.contains(‘unlocked’)) {
el.className=‘ri unlocked’;
if(!unlocked.has(i)) { unlocked.add(i); showToast(r); }
}
} else if(r.score-s <= 8) {
if(!el.classList.contains(‘unlocked’)) el.className=‘ri next’;
}
});
const nx = REWARDS.find(r => s < r.score);
$(‘nxt’).textContent = nx ? `→ ${nx.emoji} à ${nx.score} pts` : ‘🏆 MAX !’;
}

// TOAST
let tTmr;
function showToast(r) {
$(‘te’).textContent = r.emoji;
$(‘tname’).textContent = r.name;
const t = $(‘toast’); t.classList.add(‘on’);
clearTimeout(tTmr); tTmr = setTimeout(() => t.classList.remove(‘on’), 2800);
}

// COMBO
function setCombo(c) {
for(let i=0;i<5;i++) shadow.getElementById(‘cd’+i).classList.toggle(‘on’, i<c);
$(‘combo’).classList.toggle(‘on’, c>0);
}

// FLOAT TEXT
function ft(x, y, txt, col) {
const e = document.createElement(‘div’);
e.className=‘ft’; e.textContent=txt;
e.style.cssText=`left:${x}px;top:${y}px;color:${col}`;
shadow.appendChild(e);
setTimeout(() => e.remove(), 900);
}

// SHAKE HUD
function shakeHud() {
const h = $(‘hud’);
h.style.transform=‘translateX(7px)’;
setTimeout(()=>h.style.transform=‘translateX(-7px)’,75);
setTimeout(()=>h.style.transform=’’,155);
}

// HEARTS
function hearts() {
for(let i=0;i<28;i++) setTimeout(() => {
const e = document.createElement(‘div’);
e.className=‘hp’;
e.textContent = [‘💕’,‘💖’,‘💗’,‘🌸’,‘✨’][Math.floor(Math.random()*5)];
e.style.left = Math.random()*100+‘vw’;
e.style.animationDuration = (2+Math.random()*3)+‘s’;
shadow.appendChild(e);
setTimeout(() => e.remove(), 5000);
}, i*130);
}

// START
function startGame() {
items=[]; score=0; lives=3; over=false; combo=0; unlocked=new Set(); spawnMs=1300; lastSpawn=0;
$(‘hs’).textContent=‘0’; $(‘hl’).textContent=‘3’;
$(‘toast’).classList.remove(‘on’);
setCombo(0);
show(‘s-game’);
requestAnimationFrame(() => requestAnimationFrame(() => {
sizeCv();
buildPanel(); updatePanel(0);
t0 = performance.now();
cancelAnimationFrame(raf);
raf = requestAnimationFrame(loop);
}));
}

// SPAWN
function spawn() {
const good = Math.random() < 0.62;
const em = good ? GOOD[Math.floor(Math.random()*GOOD.length)] : BAD[Math.floor(Math.random()*BAD.length)];
const sz = 34 + Math.random()*22;
const maxX = Math.max(sz*2, cv.width - sz);
items.push({
x: sz + Math.random()*maxX, y:-sz-8, sz, em, good,
spd: 2+Math.random()*2.5+score*0.035,
wob: Math.random()*Math.PI*2,
wamp: 0.6+Math.random()*1.1,
wspd: 0.038+Math.random()*0.038,
caught:false, missed:false, op:1, sc:1
});
}

// LOOP
function loop(ts) {
if(over) return;
const elapsed = (ts-t0)/1000;
const rem = Math.max(0, DURATION-elapsed);

spawnMs = Math.max(460, 1300-elapsed*13);
if(ts-lastSpawn > spawnMs) { spawn(); lastSpawn=ts; }

$(‘tfill’).style.width = (rem/DURATION*100)+’%’;

const rect = cv.getBoundingClientRect();

items.forEach(it => {
if(it.caught||it.missed) { it.op-=0.07; it.sc+=0.025; return; }
it.y+=it.spd; it.wob+=it.wspd; it.x+=Math.sin(it.wob)*it.wamp;

```
// cursor hit — convert to canvas coords
const cx2 = mx - rect.left;
const cy2 = my - rect.top;
const dx=it.x-cx2, dy=it.y-cy2;
if(Math.sqrt(dx*dx+dy*dy) < it.sz*0.8) {
  it.caught=true;
  if(it.good) {
    combo=Math.min(combo+1,5); score++;
    const bon = combo>=5?' x5🔥':combo>=3?' x'+combo+'✨':'';
    ft(mx, my-24, '+1'+bon, combo>=3?'#F59E0B':'#34D399');
    updatePanel(score);
    $('hs').textContent=score;
  } else {
    combo=0; lives--;
    ft(mx, my-24, 'Aïe 💔', '#F87171');
    $('hl').textContent=lives;
    shakeHud();
  }
  setCombo(combo);
  clearTimeout(comboTmr);
  comboTmr=setTimeout(()=>{combo=0;setCombo(0);},2000);
}
if(it.y > cv.height+it.sz && !it.caught) {
  it.missed=true;
  if(it.good) { lives--; combo=0; setCombo(0); $('hl').textContent=lives; }
}
```

});
items=items.filter(it=>it.op>0);

cx.clearRect(0,0,cv.width,cv.height);
items.forEach(it => {
cx.save();
cx.globalAlpha=Math.max(0,it.op);
cx.translate(it.x,it.y); cx.scale(it.sc,it.sc);
cx.font=it.sz+‘px serif’;
cx.textAlign=‘center’; cx.textBaseline=‘middle’;
cx.shadowColor=‘rgba(0,0,0,0.25)’; cx.shadowBlur=10;
cx.fillText(it.em,0,0);
cx.restore();
});

if(lives<=0||rem<=0) { endGame(); return; }
raf=requestAnimationFrame(loop);
}

// END
function endGame() {
over=true; cancelAnimationFrame(raf);
let tr,ti,mg;
if(score>=75)     {tr=‘🌴’;ti=‘Alice est LÉGENDAIRE !’;mg=‘Costa Rica débloqué 🌴🌴 75 Noés !! Tu es absolument incroyable. Noé t'aime plus que toutes les récompenses réunies 💕’;}
else if(score>=60){tr=‘💍’;ti=‘Mon Dieu Alice…’;mg=‘Le mariage est débloqué 💍😭 60 Noés… Est-ce un signe ? Noé dit oui en tout cas.’;}
else if(score>=48){tr=‘✈️’;ti=‘Quelle joueuse !’;mg=‘Un voyage mérité ! 48 Noés dans tes bras… La valise est déjà prête dans la tête de Noé 🗺️’;}
else if(score>=37){tr=‘🏠’;ti=‘Bienvenue chez nous !’;mg=‘Notre maison est débloquée 🏡 Noé a déjà plein d'idées pour la déco (et pour la chambre 😏).’;}
else if(score>=27){tr=‘🐶’;ti=‘Trop forte Alice !’;mg=‘Le golden est débloqué 🐾 Il s'appellera comment à ton avis ?’;}
else if(score>=18){tr=‘💐’;ti=‘Bravo ma chérie !’;mg=‘Des fleurs méritées pour la joueuse la plus mignonne du monde 🌸’;}
else if(score>=10){tr=‘🍽️’;ti=‘Pas mal du tout !’;mg=‘Le restaurant est réservé 🕯️ Noé t'emmène bientôt !’;}
else              {tr=‘🥰’;ti=‘C'est l'amour !’;mg=‘Le score compte peu… Ce qui compte c'est que Noé t'aime infiniment 💕 Réessaie pour débloquer des récompenses !’;}

$(‘etrophy’).textContent=tr; $(‘etitle’).textContent=ti;
$(‘escore’).textContent=score; $(‘emsg’).textContent=mg;

const g=$(‘erwd’); g.innerHTML=’’;
REWARDS.forEach(r => {
const d=document.createElement(‘div’);
d.className=’rcard ’+(score>=r.score?‘got’:‘nope’);
d.innerHTML=`<span class="re">${r.emoji}</span><div class="rn">${r.name}</div><div class="rp">${r.score} pts</div>`;
g.appendChild(d);
});

hearts(); show(‘s-end’);
}

// Buttons
$(‘btn-start’).addEventListener(‘click’, startGame);
$(‘btn-replay’).addEventListener(‘click’, startGame);

window.addEventListener(‘resize’, () => {
if($(‘s-game’).classList.contains(‘active’)) sizeCv();
});
</script>

</body>
</html>