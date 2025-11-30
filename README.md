<!doctype html>
<html lang="ar">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>لعبة حبّية لسجى 💖</title>
<style>
  :root{
    --bg:#0f1724;
    --card:#081226;
    --accent:#ff6b9a;
    --muted:#a8b3c7;
  }
  html,body{height:100%;margin:0;font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;direction:rtl}
  body{
    background:
      radial-gradient(1200px 400px at 10% 10%, rgba(255,107,154,0.06), transparent 10%),
      radial-gradient(900px 300px at 90% 90%, rgba(255,107,154,0.04), transparent 10%),
      var(--bg);
    color:#e6eef8;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:24px;
  }

  .container{
    width:100%;
    max-width:980px;
    background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.06));
    border-radius:16px;
    box-shadow:0 12px 30px rgba(2,6,23,0.7);
    overflow:hidden;
    display:grid;
    grid-template-columns: 360px 1fr;
    gap:0;
  }

  .sidebar{
    padding:28px;
    background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.04));
    border-right:1px solid rgba(255,255,255,0.02);
    min-height:420px;
  }

  .title{
    font-size:20px;
    font-weight:700;
    color:var(--accent);
    display:flex;
    gap:10px;
    align-items:center;
  }

  .subtitle{color:var(--muted);margin-top:6px;font-size:13px}
  .bigscore{
    margin-top:22px;
    font-size:48px;
    font-weight:800;
    letter-spacing:1px;
  }

  .meta{margin-top:12px;color:var(--muted);font-size:13px}
  .controls{margin-top:20px;display:flex;gap:8px;flex-wrap:wrap}
  button{
    background:transparent;color:var(--accent);border:1px solid rgba(255,107,154,0.18);
    padding:10px 14px;border-radius:10px;cursor:pointer;font-weight:600;
    transition:all .15s ease;
  }
  button:hover{transform:translateY(-3px);box-shadow:0 6px 18px rgba(255,107,154,0.06)}
  button.secondary{background:rgba(255,255,255,0.03);color:#eaf4ff;border:1px solid rgba(255,255,255,0.02)}

  .note{margin-top:14px;color:var(--muted);font-size:13px}

  /* game area */
  .game{
    position:relative;
    min-height:420px;
    background:
      linear-gradient(180deg, rgba(255,255,255,0.01), rgba(0,0,0,0.06));
    overflow:hidden;
    display:flex;
    align-items:center;
    justify-content:center;
  }

  .stage{
    width:100%;
    height:100%;
    position:relative;
  }

  /* heart style */
  .heart{
    position:absolute;
    width:56px;
    height:56px;
    pointer-events:auto;
    transform-origin:center;
    user-select:none;
    touch-action:manipulation;
    will-change:transform,opacity,top,left;
  }

  .heart svg{filter:drop-shadow(0 8px 20px rgba(0,0,0,0.45))}

  .hud{
    position:absolute;left:18px;top:18px;background:rgba(0,0,0,0.25);padding:10px 14px;border-radius:12px;font-weight:700;
    backdrop-filter: blur(6px);
  }
  .hud .time{font-size:14px;color:#fff}
  .hud .label{display:block;font-size:12px;color:var(--muted);font-weight:500;margin-top:4px}

  .msg{
    position:absolute;right:18px;top:18px;background:linear-gradient(90deg,var(--accent),#ff8fb2);padding:10px 14px;border-radius:12px;color:#012; font-weight:700;
    transform:translateZ(0);
  }

  /* final modal */
  .modal{
    position:absolute;inset:0;display:flex;align-items:center;justify-content:center;background:linear-gradient(0deg, rgba(2,6,23,0.6), rgba(2,6,23,0.4));backdrop-filter: blur(4px);
    opacity:0;pointer-events:none;transition:opacity .25s ease;
  }
  .modal.show{opacity:1;pointer-events:auto}
  .card{
    background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    border-radius:14px;padding:28px;max-width:640px;text-align:center;
    box-shadow:0 18px 50px rgba(2,6,23,0.8);
  }
  .card h2{margin:0 0 10px 0;font-size:28px;color:var(--accent)}
  .card p{color:var(--muted);margin:10px 0 18px 0}
  .card .finalscore{font-size:40px;font-weight:800;color:#fff}
  .heart-small{display:inline-block;vertical-align:middle;margin-left:8px}

  /* small screens */
  @media (max-width:880px){
    .container{grid-template-columns:1fr;max-width:720px}
    .sidebar{order:2;border-right:none;border-top:1px solid rgba(255,255,255,0.02)}
  }
</style>
</head>
<body>

<div class="container" role="application">
  <aside class="sidebar" aria-label="controls">
    <div class="title">لعبة حبّية — <span id="girlName">سجى</span> 💖</div>
    <div class="subtitle">التقط القلوب قبل أن تختفي — كل قلب = +1 نقطة</div>

    <div class="bigscore"><span id="score">0</span> <span style="font-size:18px;color:var(--muted);font-weight:600">نقطة</span></div>

    <div class="meta">الوقت المتبقي: <strong id="timeLeft">30</strong> ثانية</div>

    <div class="controls">
      <button id="startBtn">ابدأ اللعبة</button>
      <button id="stopBtn" class="secondary">إيقاف</button>
      <button id="resetBtn" class="secondary">إعادة</button>
    </div>

    <div class="note">لمستخدِم GitHub: ارفع هذا الملف كـ <code>index.html</code> في مستودع GitHub Pages لعرض الموقع.</div>
    <div style="margin-top:14px;color:var(--muted);font-size:13px">تخصيص: غيّر اسم الحبيبة في المتصفح بالأسفل.</div>

    <!-- small form to customise name / time -->
    <div style="margin-top:14px">
      <label style="display:block;font-size:13px;color:var(--muted);margin-bottom:6px">اسم الحبيبة</label>
      <input id="nameInput" type="text" placeholder="سجى" style="width:100%;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:#fff;font-size:14px" />
      <label style="display:block;font-size:13px;color:var(--muted);margin:10px 0 6px">مدة اللعبة (ثواني)</label>
      <input id="timeInput" type="number" min="5" max="120" value="30" style="width:100%;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:#fff;font-size:14px" />
      <div style="display:flex;gap:8px;margin-top:10px">
        <button id="applyBtn" class="secondary">تطبيق</button>
      </div>
    </div>
  </aside>

  <main class="game" aria-label="game area">
    <div class="stage" id="stage" tabindex="0" aria-live="polite">
      <div class="hud" role="status">
        <div class="time">Score: <span id="hudScore">0</span></div>
        <span class="label">التقط القلوب بإصبعك أو الفأرة</span>
      </div>

      <div class="msg">For <strong id="msgName">سجى</strong> — with love ❤️</div>

      <!-- modal -->
      <div id="modal" class="modal" role="dialog" aria-hidden="true">
        <div class="card" role="document">
          <h2>أحبّك يا <span id="finalName">سجى</span> 💗</h2>
          <p id="finalText">لقد جمعت <span class="finalscore" id="finalScore">0</span> قلب — أهديك كل قلبي.</p>
          <div style="margin-top:18px">
            <button id="playAgain">العب مرة أخرى</button>
            <button id="closeModal" class="secondary">إغلاق</button>
          </div>
        </div>
      </div>

    </div>
  </main>
</div>

<script>
/* Romantic Hearts Game
   - Click / tap hearts to collect points
   - Hearts spawn randomly and fall; some are "spark" (bonus)
*/

const stage = document.getElementById('stage');
const scoreEl = document.getElementById('score');
const hudScore = document.getElementById('hudScore');
const timeLeftEl = document.getElementById('timeLeft');
const startBtn = document.getElementById('startBtn');
const stopBtn = document.getElementById('stopBtn');
const resetBtn = document.getElementById('resetBtn');
const applyBtn = document.getElementById('applyBtn');
const nameInput = document.getElementById('nameInput');
const timeInput = document.getElementById('timeInput');
const girlName = document.getElementById('girlName');
const msgName = document.getElementById('msgName');
const finalName = document.getElementById('finalName');
const modal = document.getElementById('modal');
const finalScore = document.getElementById('finalScore');
const playAgain = document.getElementById('playAgain');
const closeModal = document.getElementById('closeModal');

let score = 0;
let timeLeft = parseInt(timeInput.value,10) || 30;
let spawnInterval = null;
let gameTimer = null;
let running = false;

function setName(name){
  name = (name || 'سجى').trim();
  girlName.textContent = name;
  msgName.textContent = name;
  finalName.textContent = name;
  nameInput.value = name;
}
function setTime(t){
  timeLeft = Number(t) || 30;
  timeLeftEl.textContent = timeLeft;
  timeInput.value = timeLeft;
}

// create heart element
function makeHeart(isBonus=false){
  const el = document.createElement('div');
  el.className = 'heart';
  const size = isBonus ? 72 : (40 + Math.random()*30);
  el.style.width = size + 'px';
  el.style.height = size + 'px';
  const left = Math.random() * (stage.clientWidth - size);
  el.style.left = left + 'px';
  el.style.top = (-size) + 'px';
  el.dataset.speed = (1 + Math.random()*1.6) * (isBonus ? 0.8 : 1.2);
  el.dataset.bonus = isBonus ? '1' : '0';
  el.innerHTML = `
    <svg viewBox="0 0 32 29" width="${size}" height="${size}" xmlns="http://www.w3.org/2000/svg">
      <path d="M23.6 2c-2.6 0-4.8 1.5-6 3.7C16.2 3.5 14 2 11.4 2 7 2 4 5 4 9.2c0 7 12 13.6 12 13.6s12-6.6 12-13.6C28 5 25 2 23.6 2z" fill="${isBonus ? '#ffd6e6' : '#ff6b9a'}"/>
    </svg>
  `;
  // animation via requestAnimationFrame
  el.addEventListener('pointerdown', e=>{
    e.stopPropagation();
    collectHeart(el);
  }, {passive:true});

  stage.appendChild(el);
  return el;
}

function collectHeart(el){
  if(!el || el.dataset.collected) return;
  el.dataset.collected = '1';
  const bonus = el.dataset.bonus === '1';
  const add = bonus ? 3 : 1;
  score += add;
  scoreEl.textContent = score;
  hudScore.textContent = score;
  // small pop animation
  el.style.transition = 'transform .25s ease, opacity .25s ease';
  el.style.transform = 'scale(1.3) rotate(10deg)';
  el.style.opacity = '0';
  setTimeout(()=>{ if(el && el.remove) el.remove(); }, 300);
  // little heart burst (simple)
  if(bonus){ burst(el); }
}

function burst(el){
  // spawn a few tiny hearts
  for(let i=0;i<6;i++){
    const tiny = document.createElement('div');
    tiny.style.position='absolute';
    tiny.style.left = (parseFloat(el.style.left)||0) + (Math.random()*40-20) + 'px';
    tiny.style.top = (parseFloat(el.style.top)||0) + 'px';
    tiny.style.width = '10px'; tiny.style.height='10px';
    tiny.style.pointerEvents='none';
    tiny.innerHTML = `<svg viewBox="0 0 32 29" width="10" height="10"><path d="M23.6 2c-2.6 0-4.8 1.5-6 3.7C16.2 3.5 14 2 11.4 2 7 2 4 5 4 9.2c0 7 12 13.6 12 13.6s12-6.6 12-13.6C28 5 25 2 23.6 2z" fill="#ffe4ee"/></svg>`;
    tiny.style.opacity='1';
    tiny.style.transition='transform .9s cubic-bezier(.2,.9,.2,1),opacity .9s';
    stage.appendChild(tiny);
    requestAnimationFrame(()=> {
      tiny.style.transform = `translate(${Math.random()*80-40}px, ${-80 - Math.random()*80}px) scale(0.3)`;
      tiny.style.opacity = '0';
    });
    setTimeout(()=>tiny.remove(),900);
  }
}

function updateHearts(){
  const hearts = Array.from(stage.querySelectorAll('.heart'));
  hearts.forEach(h=>{
    if(h.dataset.collected) return;
    const speed = parseFloat(h.dataset.speed) || 1.6;
    const top = (parseFloat(h.style.top) || 0) + (2 * speed);
    h.style.top = top + 'px';
    h.style.transform = `rotate(${Math.sin(top/40)*8}deg)`;
    // remove if out of view
    if(top > stage.clientHeight + 60){
      h.remove();
    }
  });
  // spawn chance each tick if running
  if(running && Math.random() < 0.12){
    const isBonus = Math.random() < 0.08;
    makeHeart(isBonus);
  }
  requestAnimationFrame(updateHearts);
}

function startGame(){
  if(running) return;
  running = true;
  score = 0;
  scoreEl.textContent = score;
  hudScore.textContent = score;
  timeLeft = Number(timeInput.value) || 30;
  timeLeftEl.textContent = timeLeft;
  // clear old hearts
  stage.querySelectorAll('.heart').forEach(n=>n.remove());
  // spawn steady
  spawnInterval = setInterval(()=>{
    const isBonus = Math.random() < 0.08;
    makeHeart(isBonus);
  }, 700);
  gameTimer = setInterval(()=>{
    timeLeft--;
    timeLeftEl.textContent = timeLeft;
    if(timeLeft <= 0) stopGame();
  }, 1000);
  requestAnimationFrame(updateHearts);
}

function stopGame(){
  if(!running) return;
  running = false;
  clearInterval(spawnInterval);
  clearInterval(gameTimer);
  spawnInterval = null;
  gameTimer = null;
  // show modal
  finalScore.textContent = score;
  modal.classList.add('show');
  modal.setAttribute('aria-hidden','false');
}

function resetGame(){
  running = false;
  clearInterval(spawnInterval);
  clearInterval(gameTimer);
  spawnInterval = null;
  gameTimer = null;
  score = 0;
  scoreEl.textContent = score;
  hudScore.textContent = score;
  setTime(timeInput.value);
  stage.querySelectorAll('.heart').forEach(n=>n.remove());
  modal.classList.remove('show');
  modal.setAttribute('aria-hidden','true');
}

// apply customizations
applyBtn.addEventListener('click', ()=>{
  const nm = nameInput.value || 'سجى';
  setName(nm);
  setTime(Number(timeInput.value) || 30);
});

// controls
startBtn.addEventListener('click', startGame);
stopBtn.addEventListener('click', stopGame);
resetBtn.addEventListener('click', resetGame);
playAgain.addEventListener('click', ()=>{
  modal.classList.remove('show');
  modal.setAttribute('aria-hidden','true');
  startGame();
});
closeModal.addEventListener('click', ()=>{
  modal.classList.remove('show');
  modal.setAttribute('aria-hidden','true');
});

// keyboard friendly: press space to spawn a heart (small surprise)
stage.addEventListener('keydown', (e)=>{
  if(e.code === 'Space'){ e.preventDefault(); makeHeart(true); }
});

// small accessibility: clicking empty stage spawns a heart
stage.addEventListener('pointerdown', (e)=>{
  if(!running) return;
  // spawn a heart at click location as a treat
  const el = makeHeart(Math.random() < 0.2);
  const rect = stage.getBoundingClientRect();
  el.style.left = (e.clientX - rect.left - (parseFloat(el.style.width)/2)) + 'px';
  el.style.top = (e.clientY - rect.top - (parseFloat(el.style.height)/2)) + 'px';
});

// initialize
setName('سجى');
setTime(timeInput.value);
requestAnimationFrame(updateHearts);
</script>

</body>
</html>
