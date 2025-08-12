<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>มอนิ่งค้าบ 🌞</title>
<style>
  :root{
    --bg:#0f1724;
    --card:#0b1220;
    --accent:#ffd166;
    --text:#e6eef8;
  }
  html,body{height:100%;margin:0;font-family:Inter, "Segoe UI", Roboto, system-ui, sans-serif;background:
    radial-gradient(circle at 10% 10%, rgba(255,209,102,0.06), transparent 8%),
    linear-gradient(180deg,#071021 0%, #071427 60%); color:var(--text);}
  .wrap{
    min-height:100vh; display:flex; align-items:center; justify-content:center; padding:24px;
  }
  .card{
    background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.06));
    border-radius:22px; padding:36px; text-align:center; width:min(520px,94%);
    box-shadow: 0 10px 30px rgba(2,6,23,0.6), inset 0 1px 0 rgba(255,255,255,0.02);
    transform: translateY(0); transition:transform .45s ease, box-shadow .45s ease;
  }
  .card:hover{ transform: translateY(-6px); box-shadow: 0 18px 48px rgba(2,6,23,0.7); }
  .emoji{
    font-size:48px; display:block; margin-bottom:8px;
  }
  .greet{
    font-size:44px; font-weight:700; letter-spacing: -0.02em; margin:6px 0;
    background: linear-gradient(90deg, var(--accent), #ff7b7b);
    -webkit-background-clip: text; background-clip: text; color: transparent;
  }
  .sub{
    color: #9fb0d1; margin-bottom:18px;
  }
  .pulse{
    display:inline-block; padding:10px 18px; border-radius:999px;
    background: linear-gradient(90deg, rgba(255,209,102,0.12), rgba(255,123,123,0.07));
    border: 1px solid rgba(255,255,255,0.03);
    cursor:pointer; user-select:none;
    transition: transform .12s ease;
  }
  .pulse:active{ transform: scale(.98); }
  .small{ font-size:13px; color:#7f98ba; margin-top:12px; display:block;}
  /* reveal animation */
  .reveal {
    opacity:0;
    transform: translateY(10px) scale(.98);
    animation: appear .7s ease forwards;
    animation-delay: 0.25s;
  }
  @keyframes appear{
    to{ opacity:1; transform: translateY(0) scale(1); }
  }
  /* confetti simple */
  .confetti{
    position: absolute; inset:0; pointer-events:none; overflow:hidden;
  }
  .hidden{ display:none; }
</style>
</head>
<body>
<div class="wrap">
  <div class="card reveal" id="card">
    <div class="emoji">🌞</div>
    <div class="greet" id="greet">มอนิ่งค้าบ เบส</div>
    <div class="sub">หวังว่าวันนี้จะเป็นวันที่ดีสำหรับคุณนะ กลับบ้านปลอดภัยนะ 💛</div>
    <button class="pulse" id="btn">กดตรงนี้อีกที</button>
    <span class="small">กดปุ่มเพื่อโชว์เอฟเฟกต์นิดหน่อย</span>
  </div>
</div>

<!-- confetti layer -->
<canvas id="confetti" class="confetti hidden"></canvas>

<script>
  // ปุ่มกดให้มีเอฟเฟกต์ confetti ง่ายๆ
  const btn = document.getElementById('btn');
  const canvas = document.getElementById('confetti');
  const greet = document.getElementById('greet');

  btn.addEventListener('click', ()=> {
    // เปลี่ยนข้อความชั่วคราว
    greet.textContent = 'กลับบ้านปลอดภัยนะ 💕';
    // สั้นๆ แล้วกลับเป็นเดิม
    setTimeout(()=> greet.textContent = 'มอนิ่งค้าบ', 2200);
    startConfetti();
  });

  // simple confetti (no libraries)
  function startConfetti(){
    canvas.classList.remove('hidden');
    const ctx = canvas.getContext('2d');
    function resize(){ canvas.width = innerWidth; canvas.height = innerHeight; }
    resize(); window.addEventListener('resize', resize);
    const pieces = [];
    const colors = ['#ffd166','#ff7b7b','#8bd3dd','#b5ffb1','#c9b3ff'];
    for(let i=0;i<70;i++){
      pieces.push({
        x: Math.random()*canvas.width,
        y: Math.random()*canvas.height - canvas.height,
        r: 6+Math.random()*8,
        dX: -2 + Math.random()*4,
        dY: 2 + Math.random()*6,
        color: colors[Math.floor(Math.random()*colors.length)],
        rot: Math.random()*360,
        rotSpeed: -4 + Math.random()*8
      });
    }
    let t0 = performance.now();
    function frame(t){
      const dt = (t - t0) / 1000;
      t0 = t;
      ctx.clearRect(0,0,canvas.width,canvas.height);
      pieces.forEach(p=>{
        p.x += p.dX * (1 + dt*8);
        p.y += p.dY * (1 + dt*8);
        p.rot += p.rotSpeed * dt * 60;
        ctx.save();
        ctx.translate(p.x, p.y);
        ctx.rotate(p.rot * Math.PI / 180);
        ctx.fillStyle = p.color;
        ctx.fillRect(-p.r/2, -p.r/2, p.r, p.r*0.6);
        ctx.restore();
      });
      // remove pieces off screen
      if(pieces.filter(p=>p.y < canvas.height + 50).length === 0){
        canvas.classList.add('hidden');
        return;
      }
      requestAnimationFrame(frame);
    }
    requestAnimationFrame(frame);
    // auto stop after some seconds (safety)
    setTimeout(()=>{ canvas.classList.add('hidden'); }, 3200);
  }

  // auto-focus / accessibility: allow pressing Enter on button
  btn.addEventListener('keyup', (e)=> { if(e.key === 'Enter') btn.click(); });

  // Optional: show message nicely when page loads (already via CSS animation)
</script>
</body>
</html>
