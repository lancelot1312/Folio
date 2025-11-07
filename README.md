# Folio
<!doctype html>
<html lang="fr">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<meta name="description" content="Portfolio participatif d'art numérique — un espace où l'œuvre prend vie sous vos yeux et où chacun peut contribuer.">
<meta name="author" content="Ton Nom">
<link rel="icon" href="assets/favicon.png">
<title>Art en devenir — Portfolio participatif</title>

<!-- Styles minimalistes -->
<style>
  :root{
    --bg:#fff;--fg:#0b0b0b;--muted:#6b6b6b;--shadow:0 6px 30px rgba(15,15,15,0.08);--radius:14px;
  }
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,"Helvetica Neue",Arial,sans-serif;
       background:var(--bg);color:var(--fg);min-height:100vh;display:flex;align-items:center;justify-content:center;padding:48px;}
  .wrap{max-width:960px;text-align:center;}
  .title{font-size:22px;font-weight:600;margin-bottom:12px;}
  .headline{font-size:17px;color:var(--muted);margin-bottom:36px;line-height:1.5;}
  .square{width:340px;height:340px;margin:0 auto;border-radius:var(--radius);
          background:#fafafa;box-shadow:var(--shadow);display:grid;place-items:center;cursor:pointer;transition:transform .25s ease;}
  .square:hover{transform:scale(1.02);}
  .caption{position:absolute;bottom:16px;left:0;right:0;text-align:center;font-size:13px;color:var(--muted);}
  .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,.6);}
  .modal.open{display:flex;}
  .panel{background:#fff;border-radius:var(--radius);padding:24px;max-width:900px;width:95%;height:90%;box-shadow:0 30px 80px rgba(0,0,0,.4);display:flex;flex-direction:column;align-items:center;}
  .drawing{width:100%;height:100%;}
  button{cursor:pointer;border:none;background:#111;color:#fff;padding:10px 16px;border-radius:8px;font-size:14px;margin-top:16px;}
  @media(max-width:600px){body{padding:24px;}.square{width:280px;height:280px;}}
</style>
</head>

<body>
  <div class="wrap">
    <div class="title">Art en devenir — Portfolio participatif</div>
    <div class="headline">
      <strong>On peut passer notre vie à imaginer le monde.</strong><br>
      La seule réelle question est : <em>qu'est-ce qu'on va en faire ?</em>
    </div>

    <div class="square" id="square">
      <svg width="200" height="200" viewBox="0 0 600 600">
        <path d="M40 140 C120 30, 360 20, 520 120 S580 420, 360 500 s-200 -60 -280 -120"
              stroke="#111" fill="none" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <div class="caption">Cliquez pour révéler le dessin</div>
    </div>
  </div>

  <!-- Modal -->
  <div class="modal" id="modal">
    <div class="panel">
      <svg class="drawing" viewBox="0 0 800 800" id="mainSVG">
        <g id="strokes" stroke="#111" stroke-width="10" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path d="M80 420 C150 260, 320 200, 460 230 S720 300, 650 460"/>
          <path d="M120 500 C200 420, 340 390, 520 420"/>
          <path d="M200 600 C300 520, 420 520, 560 580"/>
          <path d="M530 170 C470 210, 400 200, 320 270"/>
          <path d="M60 220 C160 120, 300 100, 420 160"/>
        </g>
      </svg>
      <button id="closeModal">Fermer</button>
    </div>
  </div>

<script>
const square=document.getElementById('square');
const modal=document.getElementById('modal');
const closeModal=document.getElementById('closeModal');
const paths=document.querySelectorAll('#strokes path');
let anim=null;
function resetPaths(){
  paths.forEach(p=>{
    const len=p.getTotalLength();
    p.style.strokeDasharray=len;
    p.style.strokeDashoffset=len;
  });
}
function animate(){
  let i=0;
  function next(){
    if(i>=paths.length)return;
    const p=paths[i];
    const len=p.getTotalLength();
    p.style.transition='stroke-dashoffset 2s ease';
    p.style.strokeDashoffset=0;
    i++;
    setTimeout(next,2000);
  }
  next();
}
square.onclick=()=>{modal.classList.add('open');resetPaths();setTimeout(animate,300);}
closeModal.onclick=()=>modal.classList.remove('open');
window.onclick=(e)=>{if(e.target===modal)modal.classList.remove('open');};
resetPaths();
</script>
</body>
</html>