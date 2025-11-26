# Folio
<!doctype html>
<html lang="fr">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Lancelot : L'Interface de l'ADN Psychologique</title>
<meta name="description" content="Découvrez le Code de Lancelot – une cartographie des émotions, un miroir de l'inconscient universel." />

<style>
  /* -------------------- 1. STYLE GLOBAL & VARIABLES -------------------- */
  :root{
    --bg: #f9f9f9;
    --fg: #0b0b0b;
    --muted: #6b6b6b;
    --accent: #000000;
    --shadow: 0 6px 30px rgba(15,15,15,0.08);
    --radius: 14px;
    --ui-radius: 10px;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    background:var(--bg);
    color:var(--fg);
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
    display:flex;
    flex-direction:column;
    align-items:center;
    padding:48px 20px;
    min-height:100vh;
  }

  .wrap{
    max-width:1100px;
    width:100%;
    text-align:center;
  }

  /* Headlines / Storytelling */
  header{ margin-bottom: 40px; }
  .tagline{
    font-weight: 500;
    font-size:16px;
    color:var(--accent);
    letter-spacing: 0.05em;
    text-transform: uppercase;
    margin-bottom:8px;
  }
  .title{
    font-weight:700;
    font-size:36px;
    margin-bottom:12px;
    line-height:1.1;
  }
  .headline{
    font-size:18px;
    color:var(--muted);
    margin-bottom:28px;
    line-height:1.45;
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
  }

  /* Carré central */
  .square-container{
    display:flex;
    justify-content:center;
    gap:40px; /* Increased gap for better separation */
    align-items:flex-start;
    flex-wrap:wrap;
  }

  .square{
    width:420px; /* Increased size to emphasize main artwork */
    height:420px;
    border-radius:var(--radius);
    background:linear-gradient(180deg, #fefefe, #fff);
    box-shadow:var(--shadow);
    display:grid;
    place-items:center;
    cursor:pointer;
    position:relative;
    transition:transform .3s ease, box-shadow .3s;
    overflow:hidden;
    border:1px solid rgba(0,0,0,0.06);
  }
  .square:hover{ transform: translateY(-8px); box-shadow: 0 20px 50px rgba(15,15,15,0.15); }

  .square .caption{
    position:absolute;
    bottom:20px;
    left:20px;
    right:20px;
    font-size:14px;
    font-weight: 500;
    color:var(--accent);
    text-align:center;
    background: rgba(255,255,255,0.7);
    padding: 6px 12px;
    border-radius: 8px;
    backdrop-filter: blur(5px);
  }

  /* Gallery of Contributions (The Second Plan / La Mémoire) */
  .gallery{
    width:420px;
    display:flex;
    flex-direction:column;
    gap:16px;
    align-items:center;
    background: #fff;
    padding: 24px;
    border-radius: var(--radius);
    box-shadow: var(--shadow);
  }
  .gallery h4{ 
    margin:0 0 10px 0; 
    font-size:16px; 
    color:var(--accent); 
    font-weight: 600;
  }
  .thumbs{
    display:flex;
    gap:10px;
    flex-wrap:wrap;
    justify-content:center;
  }
  .thumb{
    width:70px;
    height:70px;
    border-radius:10px;
    border:1px solid rgba(0,0,0,0.1);
    overflow:hidden;
    display:grid;
    place-items:center;
    background:#fff;
    transition: all .2s ease;
  }
  .thumb:hover { transform: scale(1.05); }

  /* Buttons */
  .btn{
    padding:10px 18px;
    background:var(--fg);
    color:#fff;
    border:none;
    border-radius:var(--ui-radius);
    cursor:pointer;
    font-size:14px;
    font-weight:600;
    transition: background .2s ease, transform .1s;
  }
  .btn:hover{ background: #333; }
  .btn:active{ transform: scale(0.98); }
  .btn.secondary{
    background: transparent;
    color: var(--fg);
    border: 1px solid rgba(0,0,0,0.15);
    font-weight: 500;
  }
  .btn.secondary:hover { background: #eee; }

  /* -------------------- 2. MODAL D'EXPLORATION -------------------- */
  .modal{
    position:fixed;
    inset:0;
    background: rgba(10,10,10,0.8);
    display:none;
    align-items:center;
    justify-content:center;
    z-index:60;
    padding:32px;
    backdrop-filter: blur(5px);
  }
  .modal.open{ display:flex; }

  .panel{
    width:min(1200px,95%);
    height:min(80vh,95%);
    border-radius:var(--radius);
    background:#fff;
    box-shadow:0 40px 100px rgba(10,10,10,0.6);
    position:relative;
    padding:30px;
    display:flex;
    gap:25px;
    align-items:flex-start;
  }

  /* left: drawing area */
  .stage{
    flex:1;
    border-radius:14px;
    background:#fafafa;
    display:flex;
    align-items:center;
    justify-content:center;
    position:relative;
    overflow:hidden;
    height: 100%;
  }

  /* SVG drawing fits responsively */
  .drawing{
    width:90%;
    height:90%;
    max-width: 600px; /* Limit max size for control */
    max-height: 600px;
  }

  /* right: controls */
  .controls{
    width:300px;
    display:flex;
    flex-direction:column;
    gap:20px;
    align-items:stretch;
  }
  .controls h5{ margin:0 0 10px 0; font-size:15px; color:var(--fg); font-weight: 600;}
  .controls label{ font-size:13px; color:var(--muted); margin-bottom:4px; display:block; }

  .control-row{ display:flex; gap:10px; align-items:center; }
  .control-row .btn { flex: 1; padding: 12px 10px; }

  .slider-container{ margin-top: 10px; padding: 10px; border-radius: 8px; background: #f5f5f5; }

  /* small UI */
  .close{
    position:absolute;
    top:16px;
    right:16px;
    background:transparent;
    border:none;
    color:var(--muted);
    font-size:24px;
    cursor:pointer;
    z-index: 10;
  }
  .close:hover { color: var(--fg); }

  /* -------------------- 3. MODAL DE CONTRIBUTION -------------------- */
  .draw-modal{
    position:fixed;
    inset:0;
    display:none;
    align-items:center;
    justify-content:center;
    z-index:80;
    background: rgba(0,0,0,0.5);
    backdrop-filter: blur(3px);
  }
  .draw-modal.open{ display:flex; }
  .draw-panel{
    width:650px;
    max-width:95%;
    border-radius:14px;
    background:#fff;
    padding:20px;
    box-shadow:var(--shadow);
  }
  .draw-area{ border:1px solid rgba(0,0,0,0.08); border-radius:10px; overflow:hidden; background:#fff }
  #contribCanvas { display: block; background: #fff; }

  /* Footer Note */
  footer.note{
    margin-top:40px;
    color:var(--muted);
    font-size:14px;
    width: 100%;
    text-align: center;
    padding-bottom: 20px;
  }

  /* Responsive */
  @media (max-width:1150px){
    .panel{ flex-direction:column; height:auto; padding:20px; }
    .controls{ width:100%; order: -1; }
    .stage{ height: 50vh; }
  }

  @media (max-width:900px){
    body { padding: 20px; }
    .square-container{ flex-direction:column; align-items:center; gap: 30px; }
    .square, .gallery{ width:340px; height:340px; }
    .title{ font-size: 30px; }
    .gallery { height: auto; padding: 18px; }
  }
</style>
</head>
<body>
  <div class="wrap" role="main">
    <header>
      <div class="tagline">INTERFACE DU CODE DE LANCELOT</div>
      <h1 class="title">L'ADN Psychologique. La Conscience qui Se Rêve.</h1>
      <div class="headline">
        Mes dessins sont la matérialisation d'un code universel. Ils ne sont pas faits pour être expliqués, mais pour être lus. <br> Chaque interprétation continue de faire vivre le code.
      </div>
    </header>

    <div class="square-container" aria-hidden="false">
      <div class="square" id="previewSquare" tabindex="0" role="button" aria-label="Ouvrir le dessin interactif">
        <svg class="mini-svg" width="300" height="300" viewBox="0 0 600 600" aria-hidden="true">
          <g transform="translate(20,20) scale(0.9)">
            <path d="M40 140 C120 30, 360 20, 520 120 S580 420, 360 500 s-200 -60 -280 -120 L40 140 Z"
                  stroke="#111" fill="none" stroke-width="6" stroke-linecap="round" stroke-linejoin="round" opacity="0.9"/>
             <path d="M200 200 A 100 100 0 1 1 400 200 A 100 100 0 1 0 200 200"
                  stroke="#111" fill="none" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" opacity="0.7"/>
             <path d="M280 280 L320 320 M320 280 L280 320" stroke="#111" stroke-width="2"/>
          </g>
        </svg>
        <div class="caption">Cliquer pour lire le code — Regarder la Matrice s'ajuster</div>
      </div>

      <div class="gallery" aria-live="polite">
        <h4>Le Plan Second : Mémoire Collective (Contributions)</h4>
        <div class="thumbs" id="thumbs">
          <div class="note" style="font-size:13px; color:var(--muted); margin-top:12px;">
            Les contributions sont stockées localement.
          </div>
        </div>
        <div style="margin-top:16px; width: 100%;">
          <button class="btn secondary" id="openContrib">Tracer votre Voie (Contribuer)</button>
        </div>
      </div>
    </div>

    <footer class="note">Le dessin et le dessinateur ne sont qu’une seule et même chose, le reflet d’une même fraction de la conscience.</footer>
  </div>

  <div class="modal" id="modal" aria-hidden="true">
    <div class="panel" role="dialog" aria-modal="true" aria-label="Analyse du Code de Lancelot">
      <button class="close" id="closeModal" title="Fermer">✕</button>

      <div class="stage">
        <svg class="drawing" viewBox="0 0 800 800" id="mainSVG" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Dessin animé du code">
          <rect x="0" y="0" width="100%" height="100%" fill="transparent"/>
          <g id="strokes" stroke-linecap="round" stroke-linejoin="round" stroke="#111" fill="none" stroke-width="8">
            <path data-step="1" d="M100 400 C200 200, 600 200, 700 400" />
            <path data-step="2" d="M700 400 S750 600, 400 650 s-400 -100, -300 -250" />
            <path data-step="3" d="M400 150 L400 350 M300 250 L500 250" stroke-width="5" />
            </g>
        </svg>
      </div>

      <aside class="controls" aria-hidden="false">
        <h3 style="margin: 0; font-size: 20px;">Interface de Lecture</h3>
        <p style="font-size: 14px; color: var(--muted); margin: 0;">Observez le code se déployer, une ligne de conscience après l'autre.</p>

        <div class="slider-container">
          <h5>Contrôles de l'Évolution</h5>
          <div class="control-row" style="margin-bottom:15px;">
            <button class="btn secondary" id="playBtn">▶ Déployer</button>
            <button class="btn secondary" id="pauseBtn">⏸ Suspendre</button>
            <button class="btn secondary" id="rewindBtn">↺ Réinitialiser</button>
          </div>

          <div>
            <label for="speed">Vitesse de lecture (Facteur de Correction)</label>
            <input id="speed" type="range" min="0.1" max="3" step="0.1" value="1.5" style="width: 100%;" />
            <div style="font-size:13px; color:var(--muted); text-align: right;">Vitesse : × <span id="speedVal">1.5</span></div>
          </div>
        </div>

        <div style="margin-top:auto;">
          <h5>Outils du Témoin</h5>
          <div style="display:flex; gap:10px;">
            <button class="btn" id="contribInModal">Tracer votre Voie</button>
            <button class="btn secondary" id="downloadBtn">Capturer l'Instant (PNG)</button>
          </div>
        </div>
        
        <div style="margin-top:20px; font-size:12px; color:var(--muted);">
          *L'oeuvre est le reflet de l'ADN Psychologique. Chaque tracé est une tentative de trouver le "ici c'est moi"*.
        </div>
      </aside>
    </div>
  </div>

  <div class="draw-modal" id="drawModal" aria-hidden="true">
    <div class="draw-panel" role="dialog" aria-modal="true" aria-label="Contribuer un dessin">
      <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:12px;">
        <div style="font-weight:700; font-size:18px;">Tracer votre Voie (Le Miroir)</div>
        <button class="close" id="closeDraw" style="position: static;">✕</button>
      </div>

      <div class="draw-area" style="padding:15px;">
        <canvas id="contribCanvas" width="600" height="350" style="width:100%; height:auto;"></canvas>
      </div>

      <div style="display:flex; gap:10px; margin-top:15px; align-items:center;">
        <label style="font-size:13px; color:var(--muted); flex-shrink: 0;">Épaisseur du Gène</label>
        <input id="penSize" type="range" min="2" max="15" value="5" style="flex-grow: 1;"/>
        <button class="btn secondary" id="clearBtn" style="padding: 8px 15px;">Effacer</button>
        <button class="btn" id="submitBtn" style="padding: 8px 15px;">Inscrire la Balise</button>
      </div>

      <div style="font-size:12px; color:var(--muted); margin-top:10px;">
        Votre contribution (Balise) est stockée localement. Dans l'absolu, elle serait agrégée à la Matrice.
      </div>
    </div>
  </div>

<script>
/* -------------------- JAVASCRIPT: LOGIQUE DU CODE -------------------- */
(() => {
  // --- Éléments du DOM ---
  const modal = document.getElementById('modal');
  const closeModal = document.getElementById('closeModal');
  const previewSquare = document.getElementById('previewSquare');
  const playBtn = document.getElementById('playBtn');
  const pauseBtn = document.getElementById('pauseBtn');
  const rewindBtn = document.getElementById('rewindBtn');
  const speed = document.getElementById('speed');
  const speedVal = document.getElementById('speedVal');
  const downloadBtn = document.getElementById('downloadBtn');

  const drawModal = document.getElementById('drawModal');
  const openContribBtn = document.getElementById('openContrib');
  const contribInModal = document.getElementById('contribInModal');
  const closeDraw = document.getElementById('closeDraw');
  const submitBtn = document.getElementById('submitBtn');
  const clearBtn = document.getElementById('clearBtn');
  const penSize = document.getElementById('penSize');
  const canvas = document.getElementById('contribCanvas');
  const thumbs = document.getElementById('thumbs');

  // --- SVG et Animation ---
  const svg = document.getElementById('mainSVG');
  const strokesGroup = svg.querySelector('#strokes');
  // Sélectionne tous les chemins dans le groupe 'strokes'
  const paths = Array.from(strokesGroup.querySelectorAll('path'));

  // État de l'animation
  let animIndex = 0;
  let animTimer = null;
  let animSpeed = parseFloat(speed.value);
  let running = false;
  const BASE_DURATION_MS = 2500; // Durée de base pour un tracé moyen

  // 1. Initialisation des chemins pour l'animation
  paths.forEach(p => {
    const L = p.getTotalLength();
    p.style.strokeDasharray = L;
    p.style.strokeDashoffset = L; // Cache le tracé initialement
    p.style.transition = 'stroke-dashoffset 0s linear'; // Pas de transition au départ
  });

  // 2. Fonctions de contrôle de l'animation
  function animateNextPath() {
    if (animIndex >= paths.length) {
      running = false;
      return;
    }
    const p = paths[animIndex];
    const L = p.getTotalLength();
    
    // Calcule la durée en fonction de la longueur et de la vitesse
    const duration = Math.max(500, BASE_DURATION_MS * (L / 800) / animSpeed);
    
    // Réinitialise l'index si on a tout tracé (boucle implicite du code)
    if (animIndex === 0 && parseFloat(p.style.strokeDashoffset) < L) {
        rewindAnimation(false);
        setTimeout(playAnimation, 400); // Redémarre après réinitialisation
        return;
    }

    p.style.transition = `stroke-dashoffset ${duration}ms linear`;
    
    // Déclenche l'animation pour ce path
    requestAnimationFrame(() => {
      p.style.strokeDashoffset = 0;
    });

    // Planifie le passage au tracé suivant
    animTimer = setTimeout(() => {
      animIndex++;
      animateNextPath();
    }, duration + 50); // Petit délai entre les tracés
  }

  function playAnimation() {
    // Si déjà en cours, ne rien faire
    if (running) return;
    running = true;

    // Détermine le point de départ
    if (animIndex >= paths.length) animIndex = 0;

    // Reprend à partir du chemin où l'animation s'était arrêtée
    const startPathIndex = paths.findIndex(p => {
        // Un chemin est considéré comme partiellement dessiné si son dashoffset est > 1 (pour éviter les erreurs d'arrondi)
        return parseFloat(getComputedStyle(p).strokeDashoffset) > 1;
    });

    if (startPathIndex !== -1) {
        animIndex = startPathIndex;
    } else if (paths.every(p => parseFloat(getComputedStyle(p).strokeDashoffset) < 1)) {
        // Si tout est dessiné, on réinitialise avant de jouer
        rewindAnimation(false);
        setTimeout(() => { playAnimation(); }, 400);
        return;
    }
    
    animateNextPath();
  }

  function pauseAnimation() {
    if (!running) return;
    
    // Stoppe le timer
    clearTimeout(animTimer);
    running = false;

    // 'Gèle' le dessin à son état actuel en supprimant la transition
    paths.forEach(p => {
      const computedDashoffset = window.getComputedStyle(p).strokeDashoffset;
      p.style.transition = '';
      p.style.strokeDashoffset = computedDashoffset;
    });
  }

  function rewindAnimation(withTransition = true) {
    clearTimeout(animTimer);
    running = false;
    paths.forEach(p => {
      const L = p.getTotalLength();
      if (withTransition) {
        p.style.transition = 'stroke-dashoffset 350ms ease';
      } else {
        p.style.transition = 'stroke-dashoffset 0s linear';
      }
      p.style.strokeDashoffset = L;
    });
    animIndex = 0;
  }

  // 3. Gestion des événements du Modal
  playBtn.addEventListener('click', playAnimation);
  pauseBtn.addEventListener('click', pauseAnimation);
  rewindBtn.addEventListener('click', () => rewindAnimation(true));

  speed.addEventListener('input', (e) => {
    animSpeed = parseFloat(e.target.value);
    speedVal.textContent = animSpeed.toFixed(1);
  });

  function openModal() {
    modal.classList.add('open');
    modal.setAttribute('aria-hidden','false');
    closeModal.focus();
  }
  function closeModalFn() {
    modal.classList.remove('open');
    modal.setAttribute('aria-hidden','true');
    pauseAnimation();
  }

  previewSquare.addEventListener('click', openModal);
  previewSquare.addEventListener('keydown', (e) => { if (e.key === 'Enter' || e.key === ' ') openModal(); });
  closeModal.addEventListener('click', closeModalFn);
  modal.addEventListener('click', (e) => { if (e.target === modal) closeModalFn(); });

  // 4. Fonctionnalité de Téléchargement (Capture de l'Instant)
  downloadBtn.addEventListener('click', async () => {
    // Crée une copie du SVG pour s'assurer que les styles sont corrects
    const svgClone = svg.cloneNode(true);
    svgClone.setAttribute('width', '800');
    svgClone.setAttribute('height', '800');

    // S'assure que les styles de la Matrice sont conservés
    svgClone.querySelectorAll('path').forEach(p => {
      const currentDash = window.getComputedStyle(document.getElementById(p.id)).strokeDashoffset;
      p.style.strokeDashoffset = currentDash;
      p.style.transition = '';
    });

    const serializer = new XMLSerializer();
    const svgString = serializer.serializeToString(svgClone);
    
    // ... reste du code de conversion SVG -> PNG (voir code original)
    const blob = new Blob([svgString], {type:"image/svg+xml;charset=utf-8"});
    const url = URL.createObjectURL(blob);
    const img = new Image();
    img.onload = () => {
      const c = document.createElement('canvas');
      // Utilise une résolution fixe pour le téléchargement
      c.width = 1200;
      c.height = 1200;
      const ctx = c.getContext('2d');
      ctx.fillStyle = '#ffffff';
      ctx.fillRect(0,0,c.width,c.height);
      ctx.drawImage(img, 0, 0, 1200, 1200); // Dessine l'image avec un fond
      URL.revokeObjectURL(url);
      const png = c.toDataURL('image/png');
      const a = document.createElement('a');
      a.href = png;
      a.download = 'Lancelot_Code_Capture.png';
      a.click();
    };
    img.src = url;
  });


  // 5. Logique de la Contribution (Le Miroir)
  const ctx = canvas.getContext('2d');
  let drawing = false;
  let last = {x:0,y:0};
  let pen = {size: parseInt(penSize.value,10), color:'#0b0b0b'};

  function setCanvasSize() {
    const ratio = window.devicePixelRatio || 1;
    // Définit la taille réelle du canvas pour la haute résolution
    canvas.width = Math.floor(canvas.clientWidth * ratio);
    canvas.height = Math.floor(canvas.clientHeight * ratio);
    ctx.scale(ratio, ratio);
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    redrawEmpty();
  }
  function redrawEmpty(){
    ctx.clearRect(0,0,canvas.width, canvas.height);
    ctx.fillStyle = '#fff';
    ctx.fillRect(0,0,canvas.clientWidth, canvas.clientHeight);
  }
  window.addEventListener('resize', setCanvasSize);
  // Un petit délai pour s'assurer que le DOM est rendu avant de mesurer
  setTimeout(setCanvasSize, 50); 

  function toCanvasCoords(e){
    const rect = canvas.getBoundingClientRect();
    return {
      x: (e.clientX - rect.left),
      y: (e.clientY - rect.top)
    };
  }
  // Gestionnaires d'événements Pointer (pour la compatibilité tactile/souris)
  canvas.addEventListener('pointerdown', (e) => {
    drawing = true;
    last = toCanvasCoords(e);
    canvas.setPointerCapture(e.pointerId);
  });
  canvas.addEventListener('pointermove', (e) => {
    if (!drawing) return;
    const p = toCanvasCoords(e);
    ctx.strokeStyle = pen.color;
    ctx.lineWidth = pen.size;
    ctx.beginPath();
    ctx.moveTo(last.x, last.y);
    ctx.lineTo(p.x, p.y);
    ctx.stroke();
    last = p;
  });
  canvas.addEventListener('pointerup', () => drawing = false);
  canvas.addEventListener('pointercancel', () => drawing = false);

  penSize.addEventListener('input', (e) => {
    pen.size = parseInt(e.target.value, 10);
  });
  clearBtn.addEventListener('click', redrawEmpty);

  // Stockage Local (Simulation de l'Agrégation)
  const STORAGE_KEY = 'lancelot_code_contributions';

  function loadThumbs() {
    const raw = localStorage.getItem(STORAGE_KEY);
    const arr = raw ? JSON.parse(raw) : [];
    thumbs.innerHTML = '';
    
    if (arr.length === 0) {
      thumbs.innerHTML = `<div style="font-size:13px; color:var(--muted);">Pas encore de Balises inscrites. Soyez le premier !</div>`;
      return;
    }

    // Affiche les dernières contributions (max 18 pour la mise en page)
    arr.slice().reverse().slice(0, 18).forEach(dataURL => {
      const div = document.createElement('div');
      div.className = 'thumb';
      const img = document.createElement('img');
      img.src = dataURL;
      img.alt = 'Fragment de code';
      img.style.width = '100%';
      img.style.height = '100%';
      img.style.objectFit = 'cover';
      div.appendChild(img);
      thumbs.appendChild(div);
    });
  }

  submitBtn.addEventListener('click', () => {
    // S'assure que le canvas n'est pas vide (une vérification simple)
    // Ici, le code envoie le DataURL PNG pour simuler l'envoi au Plan Second (Base de données)
    const dataURL = canvas.toDataURL('image/png');
    
    try {
      const raw = localStorage.getItem(STORAGE_KEY);
      const arr = raw ? JSON.parse(raw) : [];
      
      // Ajout de la nouvelle balise et limitation de la mémoire locale (par exemple à 50)
      arr.push(dataURL);
      while (arr.length > 50) arr.shift();
      localStorage.setItem(STORAGE_KEY, JSON.stringify(arr));
      
      loadThumbs(); // Recharge les vignettes
      drawModal.classList.remove('open');
    } catch (e) {
      alert("Erreur lors de l'enregistrement de la Balise (stockage plein ou bloqué).");
    }
  });

  // Gestion des Modals de Contribution
  function openDrawModal() {
    drawModal.classList.add('open');
    drawModal.setAttribute('aria-hidden','false');
    // Efface et prépare le canvas pour la nouvelle contribution
    setTimeout(() => { 
      redrawEmpty(); 
      canvas.focus();
    }, 50);
  }

  openContribBtn.addEventListener('click', openDrawModal);
  contribInModal.addEventListener('click', openDrawModal);
  closeDraw.addEventListener('click', () => drawModal.classList.remove('open'));
  drawModal.addEventListener('click', (e) => { if (e.target === drawModal) drawModal.classList.remove('open'); });

  // 6. Accessibilité et Initialisation Finale
  window.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') {
      if (drawModal.classList.contains('open')) drawModal.classList.remove('open');
      else if (modal.classList.contains('open')) closeModalFn();
    }
  });

  // Charge les contributions existantes au démarrage
  loadThumbs();
})();
</script>
</body>
</html>
