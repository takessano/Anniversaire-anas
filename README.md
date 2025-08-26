<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Joyeux Anniversaire 🎉</title>
  <style>
    :root{
      --bg1:#ff9a9e; --bg2:#fad0c4; --card:#ffffffdd; --txt:#222;
      --accent:#ff4d6d;
    }
    *{box-sizing:border-box}
    body{
      margin:0; min-height:100vh;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      color:var(--txt);
      background: linear-gradient(135deg,var(--bg1),var(--bg2));
      display:flex; align-items:center; justify-content:center; padding:20px;
      overflow-x:hidden;
    }
    .card{
      width:min(680px,100%);
      background:var(--card);
      border-radius:20px;
      padding:28px 22px;
      box-shadow:0 10px 30px rgba(0,0,0,.15);
      text-align:center;
      backdrop-filter: blur(6px);
    }
    h1{font-size:clamp(28px,7vw,44px); margin:6px 0 10px}
    p.lead{font-size:clamp(16px,4.8vw,20px); margin:0 0 14px}
    .countdown{
      display:flex; gap:10px; justify-content:center; margin:18px 0 10px; flex-wrap:wrap;
    }
    .tile{
      min-width:76px; padding:10px 12px; border-radius:14px;
      background:#fff; box-shadow:0 6px 18px rgba(0,0,0,.08);
    }
    .num{font-size:28px; font-weight:700; line-height:1}
    .lbl{font-size:12px; opacity:.7}
    .btns{display:flex; gap:10px; justify-content:center; flex-wrap:wrap; margin-top:16px}
    button{
      border:0; border-radius:999px; padding:12px 18px; font-size:16px; cursor:pointer;
      background:var(--accent); color:#fff; box-shadow:0 6px 18px rgba(255,77,109,.35);
    }
    button.secondary{ background:#111; box-shadow:0 6px 18px rgba(0,0,0,.25); }
    .msg{margin-top:12px; font-weight:600}
    /* Confettis */
    .confetti-piece{
      position:fixed; top:-8vh; width:8px; height:14px; opacity:.9; z-index:9999; pointer-events:none;
    }
    @keyframes fall{
      to { transform: translateY(120vh) rotate(720deg); opacity:1; }
    }
    /* Bougies animées */
    .candles{font-size:28px; letter-spacing:6px; margin-top:6px}
    .candles span{display:inline-block; animation:float 2.6s ease-in-out infinite}
    .candles span:nth-child(2){animation-delay:.2s}
    .candles span:nth-child(3){animation-delay:.4s}
    @keyframes float { 50% { transform: translateY(-6px); } }
    footer{margin-top:16px; font-size:12px; opacity:.6}
  </style>
</head>
<body>
  <main class="card">
    <h1>🎂 Joyeux 16e Anniversaire <span id="friend-name">Anas</span> ! 🎉</h1>
    <div class="candles"><span>🕯️</span><span>🕯️</span><span>🕯️</span></div>
    <p class="lead">Surprise ! Voici une petite page rien que pour toi 🥳</p>
    <p class="lead">⚠️ Attends la fin du compte à rebours pour découvrir ta surprise 🎁⏳</p>

    <section id="countwrap" aria-live="polite">
      <div class="countdown" id="countdown">
        <div class="tile"><div class="num" id="d">0</div><div class="lbl">jours</div></div>
        <div class="tile"><div class="num" id="h">0</div><div class="lbl">heures</div></div>
        <div class="tile"><div class="num" id="m">0</div><div class="lbl">minutes</div></div>
        <div class="tile"><div class="num" id="s">0</div><div class="lbl">secondes</div></div>
      </div>
      <div class="msg" id="msg"></div>
    </section>

    <div class="btns">
      <button id="blow">Souffler les bougies 🎂</button>
      <button class="secondary" id="wish">Faire un vœu ✨</button>
    </div>

    <footer>fait par ton pote (youssef)</footer>
  </main>

  <script>
    /*********** RÉGLAGES ***********/
    const NAME = "Anas"; 
    // Aujourd'hui à 12h00 (si déjà passé → demain 12h00)
    let now = new Date();
    let TARGET = new Date(now.getFullYear(), now.getMonth(), now.getDate(), 12, 0, 0);
    if (TARGET <= now) {
      TARGET.setDate(TARGET.getDate() + 1);
    }

    /*********** CODE ***********/
    document.getElementById("friend-name").textContent = NAME;

    const d=document.getElementById("d"),
          h=document.getElementById("h"),
          m=document.getElementById("m"),
          s=document.getElementById("s"),
          msg=document.getElementById("msg");

    function pad(n){ return n.toString().padStart(2,"0"); }

    function tick(){
      const now = new Date();
      const diff = TARGET - now;
      if(diff <= 0){
        d.textContent=h.textContent=m.textContent=s.textContent="00";
        msg.innerHTML = `
          🎉 Joyeux 16e Anniversaire ${NAME} 🎉 <br><br>
          Bon, généralement je suis pas trop sentimental 😅... 
          mais aujourd’hui je fais une exception. <br><br>
          Je t’espère le meilleur du monde : 
          une femme, 5 enfants, ton SUV full option 🚙✨ <br>
          et surtout réussir tes études pour devenir 
          l’avocat que tu veux être ⚖️🔥
        `;
        dropConfetti();
        return;
      }
      const days = Math.floor(diff / (1000*60*60*24));
      const hours = Math.floor((diff / (1000*60*60)) % 24);
      const mins = Math.floor((diff / (1000*60)) % 60);
      const secs = Math.floor((diff / 1000) % 60);
      d.textContent = days;
      h.textContent = pad(hours);
      m.textContent = pad(mins);
      s.textContent = pad(secs);
      requestAnimationFrame(()=>setTimeout(tick, 250));
    }
    tick();

    // Confettis
    function dropConfetti(count=140){
      for(let i=0;i<count;i++){
        const el=document.createElement("span");
        el.className="confetti-piece";
        el.style.left = Math.random()*100 + "vw";
        const size = 6 + Math.random()*10;
        el.style.width = size + "px";
        el.style.height = (size*1.4) + "px";
        el.style.background = `hsl(${Math.random()*360},90%,60%)`;
        el.style.animation = `fall ${3 + Math.random()*2}s linear forwards`;
        el.style.transform = `rotate(${(Math.random()*360)}deg)`;
        document.body.appendChild(el);
        setTimeout(()=>el.remove(), 6000);
      }
      if(navigator.vibrate) navigator.vibrate([40,60,40]);
    }

    const wishes=[
      "Tout ce que tu veux se réalise ✨",
      "Encore une année incroyable qui commence 🚀",
      "Santé, bonheur et fous rires 🎁",
      "Tu es le futur avocat, mec ! ⚖️🔥"
    ];

    document.getElementById("blow").addEventListener("click", ()=>{
      dropConfetti();
      msg.textContent = "Bougies soufflées ! Fais un vœu 🎂";
    });

    document.getElementById("wish").addEventListener("click", ()=>{
      msg.textContent = wishes[Math.floor(Math.random()*wishes.length)];
    });
  </script>
</body>
</html>
