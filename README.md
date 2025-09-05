# 25-greetings-for-those-who-are-25-years-old
Rizqi's Birthday
<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>🎂 Ucapan untuk Rizqi — 25</title>
  <meta name="description" content="Ucapan ulang tahun interaktif untuk Rizqi yang ke-25, bisa di-host di GitHub Pages" />
  <style>
    :root{
      --bg1:#8EC5FC; --bg2:#E0C3FC; --text:#0f172a; --accent:#ff5d9e; --card:#ffffffcc;
    }
    *{box-sizing:border-box}
    body{
      margin:0; min-height:100vh; font-family: system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, Noto Sans, "Apple Color Emoji", "Segoe UI Emoji";
      color:var(--text);
      background: linear-gradient(135deg,var(--bg1),var(--bg2));
      overflow-x:hidden;
    }
    .container{
      display:grid; place-items:center; min-height:100vh; padding:32px;
    }
    .card{
      width:min(840px, 100%); background:var(--card); backdrop-filter: blur(8px);
      border-radius:24px; padding:32px; box-shadow: 0 20px 80px rgba(0,0,0,.2);
      position:relative; overflow:hidden;
    }
    h1{ font-size: clamp(28px, 5vw, 56px); margin:8px 0; line-height:1.1 }
    p{ font-size: clamp(14px, 2.5vw, 18px); opacity:.9 }

    .name{ font-weight:800; letter-spacing:.5px; }
    .sparkle{
      position:absolute; inset:0; pointer-events:none; background:
        radial-gradient(1200px 600px at -10% -10%, rgba(255,255,255,.25), transparent 60%),
        radial-gradient(800px 400px at 110% 10%, rgba(255,255,255,.25), transparent 60%);
    }

    .controls{ display:flex; gap:12px; flex-wrap:wrap; margin-top:16px }
    button, .linklike{
      border:0; padding:12px 16px; border-radius:999px; cursor:pointer; font-weight:600;
      background:#111827; color:white; transition: transform .05s ease, box-shadow .2s ease;
      box-shadow: 0 6px 20px rgba(0,0,0,.25);
    }
    button:hover{ transform: translateY(-1px) }
    .ghost{ background:transparent; color:#111; border:2px solid #111 }

    .footer{ display:flex; gap:12px; align-items:center; justify-content:space-between; margin-top:24px; flex-wrap:wrap }
    .tiny{ opacity:.7; font-size:13px }

    /* Balloons */
    .balloons{ position:absolute; inset:0; overflow:hidden; pointer-events:none; }
    .balloon{ position:absolute; bottom:-120px; width:40px; height:54px; border-radius:50% 50% 45% 45%;
      background:var(--accent); box-shadow: inset -6px -10px 0 rgba(0,0,0,.08);
      animation: floatUp linear infinite; }
    .balloon:after{ content:""; position:absolute; left:50%; top:52px; width:2px; height:80px; background:rgba(0,0,0,.2) }
    @keyframes floatUp { to { transform: translateY(-120vh) } }

    /* Confetti bits */
    .confetti{ position:fixed; inset:0; pointer-events:none; overflow:hidden }
    .confetti > i{ position:absolute; width:8px; height:14px; opacity:.9; will-change: transform; }

    /* Message */
    .msg{ font-size: clamp(16px, 3vw, 22px); margin-top:8px }
  </style>
</head>
<body>
  <div class="container">
    <div class="card">
      <div class="sparkle"></div>
      <div class="balloons" id="balloons"></div>
      <h1 id="title">Selamat Ulang Tahun, <span class="name" id="name">Rizqi</span>! 🎉</h1>
      <p class="msg" id="msg">Rizqi's turns 25 — Selamat ulang tahun! Semoga sehat dan sukses selalu. 🎂</p>

      <div class="controls">
        <button id="btnConfetti">Lepas Konfeti</button>
        <button id="btnBalloons" class="ghost">Tambah Balon</button>
        <button id="btnShare" title="Buat tautan ucapan">Buat Tautan</button>
        <button id="btnMusic" class="ghost">Putar Lagu</button>
      </div>

      <div class="footer">
        <span class="tiny">Tip: Tambah query di URL, contoh: <code>?to=Rizqi&msg=Selamat%20ulang%20tahun%20Rizqi!</code></span>
        <a class="linklike tiny" href="https://github.com/" target="_blank" rel="noreferrer">Made for GitHub Pages</a>
      </div>
    </div>
  </div>

  <!-- Opsional: tambahkan file audio bernama happy-birthday.mp3 di root repo -->
  <audio id="audio" src="happy-birthday.mp3" preload="none"></audio>
  <div class="confetti" id="confetti"></div>

  <script>
    // --- Helpers ---
    const $ = (sel)=>document.querySelector(sel);
    const params = new URLSearchParams(location.search);

    // Personalize via URL params
    const name = params.get('to') || 'Rizqi';
    const message = params.get('msg') || "Rizqi's turns 25 — Selamat ulang tahun! Semoga sehat dan sukses selalu. 🎂";
    $('#name').textContent = name;
    $('#msg').textContent = message;

    // Confetti generator (no library)
    function launchConfetti(count=120){
      const root = $('#confetti');
      const W = window.innerWidth; const H = window.innerHeight;
      for(let i=0;i<count;i++){
        const bit = document.createElement('i');
        const x = Math.random()*W; const size = 6 + Math.random()*10; const dur = 2 + Math.random()*2.5;
        bit.style.left = x+'px'; bit.style.top = '-20px'; bit.style.width = size+'px'; bit.style.height = (size*1.4)+'px';
        // random color
        bit.style.background = `hsl(${Math.random()*360}, 90%, 55%)`;
        bit.style.transform = `translateY(0) rotate(0deg)`;
        bit.style.transition = `transform ${dur}s cubic-bezier(.15,.6,.2,1.0)`;
        root.appendChild(bit);
        requestAnimationFrame(()=>{
          const dx = (Math.random()*200 - 100);
          bit.style.transform = `translate(${dx}px, ${H+60}px) rotate(${Math.random()*720}deg)`;
        });
        setTimeout(()=>bit.remove(), dur*1000+600);
      }
    }

    // Balloons
    function addBalloons(n=12){
      const box = $('#balloons'); const W = window.innerWidth;
      for(let i=0;i<n;i++){
        const b = document.createElement('div'); b.className='balloon';
        const hue = Math.floor(Math.random()*360);
        b.style.background = `linear-gradient(180deg, hsl(${hue} 90% 65%), hsl(${hue} 90% 58%))`;
        b.style.left = Math.random()*(W-40)+'px';
        b.style.animationDuration = 8 + Math.random()*8 + 's';
        box.appendChild(b);
        setTimeout(()=>b.remove(), 16000);
      }
    }

    // Shareable link builder
    function buildLink(){
      const to = prompt('Nama yang berulang tahun?', name) || name;
      const msg = prompt('Pesan singkat ucapan?', message) || message;
      const url = `${location.origin}${location.pathname}?to=${encodeURIComponent(to)}&msg=${encodeURIComponent(msg)}`;
      navigator.clipboard?.writeText(url);
      alert('Tautan ucapan sudah disalin!

'+url);
      history.replaceState(null, '', url);
      $('#name').textContent = to; $('#msg').textContent = msg;
    }

    // Music
    const audio = $('#audio');
    let playing = false;
    async function toggleMusic(){
      try{
        if(!playing){ await audio.play(); playing=true; $('#btnMusic').textContent='Jeda Lagu'; }
        else{ audio.pause(); playing=false; $('#btnMusic').textContent='Putar Lagu'; }
      }catch(err){ alert('Tambahkan file \"happy-birthday.mp3\" ke repo agar lagu bisa diputar.'); }
    }

    // Bind controls
    $('#btnConfetti').addEventListener('click', ()=>launchConfetti());
    $('#btnBalloons').addEventListener('click', ()=>addBalloons());
    $('#btnShare').addEventListener('click', buildLink);
    $('#btnMusic').addEventListener('click', toggleMusic);

    // First load effects
    addBalloons(18);
    setTimeout(()=>launchConfetti(160), 300);
  </script>
</body>
</html>
