 <!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Birthday Surprise 🎉</title>
<style>
  :root{
    --bg:#0f172a;
    --card:#0b1220;
    --accent:#ff7ab6;
    --text:#f8fafc;
  }
  html,body{height:100%;margin:0;font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;}
  body{
    background: radial-gradient( circle at 10% 10%, rgba(255,122,182,0.08), transparent 10%),
                linear-gradient(180deg, #071028 0%, #081328 100%);
    color:var(--text);
    display:flex;align-items:center;justify-content:center;
  }

  .stage{
    width:920px; max-width:95%; min-height:540px;
    background: linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));
    border-radius:16px; padding:28px; box-shadow: 0 12px 40px rgba(2,6,23,0.7);
    position:relative; overflow:hidden;
  }

  header{display:flex;gap:16px;align-items:center}
  .logo{width:64px;height:64px;border-radius:12px;background:linear-gradient(135deg,var(--accent),#6ee7b7);
    display:flex;align-items:center;justify-content:center;font-weight:700;color:#081328;font-size:26px}
  h1{margin:0;font-size:20px}
  p.lead{margin:6px 0 0;color:rgba(255,255,255,0.75)}

  .center{
    display:grid;place-items:center;height:calc(100% - 110px);
    text-align:center;padding:0 24px 24px;
  }

  .open-btn{
    background:linear-gradient(90deg,var(--accent),#ffd166);
    border:none;padding:14px 22px;border-radius:10px;font-size:18px;font-weight:700;
    cursor:pointer;box-shadow:0 8px 18px rgba(0,0,0,0.35);color:#081328;
  }
  .open-btn:active{transform:translateY(1px)}
  .hint{margin-top:12px;color:rgba(255,255,255,0.75);font-size:14px}

  /* Surprise overlay */
  .overlay{
    position:absolute;inset:0;background:linear-gradient(180deg, rgba(2,6,23,0.85), rgba(2,6,23,0.95));
    display:none;align-items:center;justify-content:center;flex-direction:column;padding:30px;
  }
  .card{
    width:80%; max-width:720px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    padding:28px; box-shadow: 0 20px 60px rgba(2,6,23,0.7); text-align:center; position:relative;
  }
  .greet{
    font-size:36px;margin:0 0 8px;letter-spacing:0.5px;
  }
  .message{font-size:18px;margin:0 0 18px;color:rgba(255,255,255,0.95)}
  .typewriter{font-weight:700;color:var(--accent);min-height:32px}
  .close-btn{
    margin-top:18px;background:transparent;border:2px solid rgba(255,255,255,0.07);
    color:var(--text);padding:10px 16px;border-radius:10px;cursor:pointer;
  }

  /* Balloons */
  .balloon{
    position:absolute;width:64px;height:84px;border-radius:40px 40px 42px 42px; background:var(--accent);
    bottom:-140px; left:10%; opacity:0.95; transform-origin:center;box-shadow: 0 8px 20px rgba(2,6,23,0.5);
    animation: floatUp 8s linear infinite;
  }
  .balloon::after{
    content:""; width:2px;height:60px;background:#fff;position:absolute;left:50%;transform:translateX(-50%);bottom:-58px;border-radius:2px;opacity:0.75;
  }
  .balloon.b2{left:30%;animation-duration:9s; background:#7dd3fc}
  .balloon.b3{left:55%; animation-duration:7s; background:#fde68a}
  .balloon.b4{left:75%; animation-duration:10s; background:#fca5a5}

  @keyframes floatUp{
    0%{transform: translateY(0) rotate(-6deg)}
    50%{transform: translateY(-160vh) rotate(6deg)}
    100%{transform: translateY(-320vh) rotate(-6deg)}
  }

  /* Responsive */
  @media (max-width:560px){
    .greet{font-size:26px}
    .card{padding:18px}
  }

  /* Confetti canvas covers entire stage when active */
  canvas#confettiCanvas{position:absolute;inset:0;pointer-events:none;display:none}
</style>
</head>
<body>
  <div class="stage" id="stage">
    <canvas id="confettiCanvas"></canvas>

    <header>
      <div class="logo">BD</div>
      <div>
        <h1>Birthday Surprise</h1>
        <p class="lead">Make it special — personalize below & press the button to start</p>
      </div>
    </header>

    <div class="center">
      <div>
        <div style="margin-bottom:16px">
          <label style="display:block;text-align:left;font-size:13px;margin-bottom:6px">Recipient name</label>
          <input id="nameInput" value="Amara" style="padding:10px 12px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:var(--text);width:220px" />
        </div>

        <div style="margin-bottom:18px">
          <label style="display:block;text-align:left;font-size:13px;margin-bottom:6px">Short message</label>
          <input id="messageInput" value="Wishing you endless smiles, love, and cake 🎂" style="padding:10px 12px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:var(--text);width:420px" />
        </div>

        <button class="open-btn" id="openBtn">Open the surprise 🎉</button>
        <div class="hint">Tip: replace the audio file link in the code with your chosen song.</div>
      </div>
    </div>

    <div class="overlay" id="overlay" aria-hidden="true">
      <div class="balloon" style="display:block"></div>
      <div class="balloon b2"></div>
      <div class="balloon b3"></div>
      <div class="balloon b4"></div>

      <div class="card" role="dialog" aria-modal="true">
        <h2 class="greet" id="greet">Happy Birthday, <span id="recipientName">Friend</span>!</h2>
        <p class="message"><span id="typed" class="typewriter"></span></p>
        <div style="font-size:52px;line-height:1">🎂✨</div>
        <button class="close-btn" id="closeBtn">Close</button>
      </div>
    </div>

    <!-- Hidden audio element: replace src with your own mp3 (royalty-free or personal). -->
    <audio id="bgAudio" src="https://cdn.pixabay.com/download/audio/2021/08/04/audio_aa0d6b7f8a.mp3?filename=birthday-ambient-7311.mp3" preload="auto"></audio>
  </div>

<script>
/* ---- Simple confetti implementation ---- */
function Confetti(canvas){
  this.canvas = canvas;
  this.ctx = canvas.getContext('2d');
  this.particles = [];
  this.w = canvas.width = canvas.offsetWidth;
  this.h = canvas.height = canvas.offsetHeight;
  this.running = false;

  window.addEventListener('resize', ()=> {
    this.w = canvas.width = canvas.offsetWidth;
    this.h = canvas.height = canvas.offsetHeight;
  });

  this.spawn = (count=80) => {
    const colors = ['#ff7ab6','#7dd3fc','#fde68a','#fca5a5','#c7f9cc','#ffd166'];
    for(let i=0;i<count;i++){
      this.particles.push({
        x: Math.random()*this.w,
        y: Math.random()*this.h - this.h,
        vx: (Math.random()-0.5)*4,
        vy: 2 + Math.random()*4,
        r: 6+Math.random()*8,
        color: colors[Math.floor(Math.random()*colors.length)],
        rot: Math.random()*360,
        vr: (Math.random()-0.5)*10,
      });
    }
  };

  this.update = () => {
    this.ctx.clearRect(0,0,this.w,this.h);
    for(let p of this.particles){
      p.x += p.vx;
      p.y += p.vy;
      p.vy += 0.02;
      p.rot += p.vr;

      this.ctx.save();
      this.ctx.translate(p.x, p.y);
      this.ctx.rotate(p.rot * Math.PI/180);
      this.ctx.fillStyle = p.color;
      this.ctx.fillRect(-p.r/2, -p.r/2, p.r, p.r*0.6);
      this.ctx.restore();
    }
    // remove old
    this.particles = this.particles.filter(p => p.y < this.h + 100);
  };

  this.loop = ()=>{
    if(!this.running) return;
    this.update();
    requestAnimationFrame(this.loop);
  };

  this.start = ()=>{
    if(this.running) return;
    this.running = true;
    canvas.style.display = 'block';
    this.spawn(160);
    this.loop();
    // periodically add more
    this.interval = setInterval(()=> this.spawn(40),900);
  };

  this.stop = ()=>{
    this.running = false;
    clearInterval(this.interval);
    setTimeout(()=> canvas.style.display='none', 900);
  };
}

/* ---- Typewriter effect ---- */
function typeWriter(element, text, speed=36){
  element.textContent = '';
  let i=0;
  return new Promise(resolve=>{
    const t = setInterval(()=>{
      element.textContent += text.charAt(i);
      i++;
      if(i>text.length-1){ clearInterval(t); resolve(); }
    }, speed);
  });
}

/* ---- Wiring it up ---- */
const openBtn = document.getElementById('openBtn');
const overlay = document.getElementById('overlay');
const closeBtn = document.getElementById('closeBtn');
const nameInput = document.getElementById('nameInput');
const messageInput = document.getElementById('messageInput');
const recipientName = document.getElementById('recipientName');
const typedEl = document.getElementById('typed');
const audio = document.getElementById('bgAudio');
const confettiCanvas = document.getElementById('confettiCanvas');
const confetti = new Confetti(confettiCanvas);

openBtn.addEventListener('click', async ()=>{
  // personalization
  const name = nameInput.value.trim() || 'Friend';
  const msg = messageInput.value.trim() || 'Wishing you joy and love!';
  recipientName.textContent = name;

  // show overlay
  overlay.style.display = 'flex';
  overlay.setAttribute('aria-hidden','false');

  // try to play music (must be initiated by user gesture; click qualifies)
  try {
    audio.currentTime = 0;
    await audio.play();
  } catch(e){
    // if playback fails, it's usually due to autoplay policy or invalid file
    console.warn('audio play failed:', e);
  }

  // start confetti & balloons visible already via overlay
  confetti.start();

  // typewriter the message
  typedEl.textContent = '';
  await typeWriter(typedEl, msg, 24);
});

closeBtn.addEventListener('click', ()=>{
  overlay.style.display = 'none';
  overlay.setAttribute('aria-hidden','true');
  audio.pause();
  confetti.stop();
});

/* Optional: if you want the surprise to auto-open when url has ?surprise=1 */
(function tryAutoOpen(){
  const url = new URL(window.location.href);
  if(url.searchParams.get('surprise') === '1'){
    setTimeout(()=> openBtn.click(), 700);
  }
})();
</script>
</body>
</html>
o
