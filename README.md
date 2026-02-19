# facundoesteche-lab.github.io
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>La Batalla de San Antonio — Salto, 1846</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=Cinzel:wght@400;600;900&display=swap');

  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
  :root {
    --black: #060608; --red: #8B1A1A; --red-bright: #C0392B;
    --gold: #B8912A; --gold-light: #D4AA4A;
    --white: #F0EDE8; --grey: #888880; --text: #C8C4BC;
  }
  html { scroll-behavior: smooth; }
  body {
    background: var(--black); color: var(--white);
    font-family: 'Cormorant Garamond', Georgia, serif;
    overflow-x: hidden; transition: background-color 1.2s ease;
  }
  #particles-canvas {
    position: fixed; top:0; left:0; width:100%; height:100%;
    pointer-events:none; z-index:0; opacity:0.4;
  }
  .progress-bar {
    position:fixed; top:0; left:0; height:2px;
    background: linear-gradient(to right,var(--red),var(--gold));
    z-index:200; width:0%; transition:width .1s linear;
  }

  /* ── NAVBAR ── */
  .navbar {
    position:fixed; top:0; left:0; right:0; z-index:150;
    padding:0 3vw; height:56px;
    display:flex; align-items:center; justify-content:space-between;
    background:rgba(6,6,8,0); backdrop-filter:blur(0px);
    border-bottom:1px solid rgba(184,145,42,0);
    transition:background .5s,backdrop-filter .5s,border-color .5s;
  }
  .navbar.scrolled { background:rgba(6,6,8,.88); backdrop-filter:blur(14px); border-bottom:1px solid rgba(184,145,42,.15); }
  .nav-brand { font-family:'Cinzel',serif; font-size:.7rem; letter-spacing:.3em; color:var(--gold); text-decoration:none; opacity:0; transition:opacity .5s; }
  .navbar.scrolled .nav-brand { opacity:1; }
  .nav-links { display:flex; gap:2rem; list-style:none; opacity:0; transition:opacity .5s; }
  .navbar.scrolled .nav-links { opacity:1; }
  .nav-links a { font-family:'Cinzel',serif; font-size:.58rem; letter-spacing:.25em; color:var(--grey); text-transform:uppercase; text-decoration:none; transition:color .3s; white-space:nowrap; }
  .nav-links a:hover { color:var(--gold); }
  .nav-lang { display:flex; gap:.6rem; align-items:center; opacity:0; transition:opacity .5s; }
  .navbar.scrolled .nav-lang { opacity:1; }
  .nav-lang a { font-family:'Cinzel',serif; font-size:.55rem; letter-spacing:.2em; color:var(--grey); text-decoration:none; padding:.2rem .5rem; border:1px solid transparent; transition:all .3s; }
  .nav-lang a:hover { color:var(--gold); border-color:rgba(184,145,42,.3); }
  .nav-lang a.active { color:var(--gold); border-color:rgba(184,145,42,.5); }
  .nav-sep { color:rgba(184,145,42,.3); font-size:.6rem; }
  @media(max-width:768px){ .nav-links{display:none;} }

  /* ── HERO ── */
  .hero {
    position:relative; height:100vh;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    text-align:center; z-index:1; overflow:hidden;
  }
  .hero::before {
    content:''; position:absolute; inset:0;
    background:radial-gradient(ellipse at 50% 60%,rgba(139,26,26,.2) 0%,transparent 70%);
    pointer-events:none;
  }
  .hero-date { font-family:'Cinzel',serif; font-size:clamp(.65rem,1.4vw,.9rem); letter-spacing:.4em; color:var(--gold); text-transform:uppercase; margin-bottom:2.5rem; opacity:0; animation:fadeUp 1.2s ease .3s forwards; }
  .hero-title { font-family:'Cinzel',serif; font-size:clamp(3rem,10vw,9rem); font-weight:900; line-height:.9; color:var(--white); opacity:0; animation:fadeUp 1.2s ease .7s forwards; }
  .hero-title span { display:block; color:var(--red-bright); font-style:italic; font-weight:600; font-family:'Cormorant Garamond',serif; font-size:1.1em; }
  .hero-subtitle { font-size:clamp(.9rem,2vw,1.25rem); font-weight:300; font-style:italic; color:var(--grey); margin-top:2rem; letter-spacing:.05em; opacity:0; animation:fadeUp 1.2s ease 1.1s forwards; max-width:700px; padding:0 2rem; overflow:hidden; border-right:2px solid var(--gold); white-space:nowrap; width:0; animation:fadeUp 0s ease 1.1s forwards, typing 2.5s steps(60,end) 1.4s forwards, blink .75s step-end 3.9s 4; }
  .scroll-cue { position:absolute; bottom:2.5rem; left:50%; transform:translateX(-50%); display:flex; flex-direction:column; align-items:center; gap:.5rem; opacity:0; animation:fadeUp 1s ease 2.2s forwards; color:var(--grey); font-family:'Cinzel',serif; font-size:.6rem; letter-spacing:.3em; text-transform:uppercase; }
  .scroll-line { width:1px; height:50px; background:linear-gradient(to bottom,var(--gold),transparent); animation:scrollPulse 2s ease-in-out infinite; }

  /* ── SECTIONS ── */
  .story-section { position:relative; min-height:100vh; display:flex; align-items:center; justify-content:center; z-index:1; padding:12vh 6vw; }
  .story-inner { max-width:800px; text-align:center; width:100%; }
  .story-inner.wide { max-width:1050px; }

  /* ── REVEAL BASE ── */
  .reveal { opacity:0; transform:translateY(55px); transition:opacity 1s ease,transform 1s ease; }
  .reveal.visible { opacity:1; transform:translateY(0); }
  .reveal-delay-1{transition-delay:.15s} .reveal-delay-2{transition-delay:.35s}
  .reveal-delay-3{transition-delay:.55s} .reveal-delay-4{transition-delay:.75s}
  .reveal-delay-5{transition-delay:.95s}

  /* ── SENTENCE-BY-SENTENCE (Apple style) ── */
  .sentence-block { margin:3rem 0; }
  .sentence {
    display:block; font-size:clamp(1.1rem,2.2vw,1.5rem); font-weight:300;
    line-height:1.7; color:rgba(200,196,188,0.25);
    margin-bottom:.8rem; transition:color 1s ease;
  }
  .sentence.lit { color:var(--text); }
  .sentence.dim { color:rgba(200,196,188,0.18); }

  /* ── KEY PHRASE (grande) ── */
  .key-phrase {
    font-family:'Cinzel',serif; font-size:clamp(1.8rem,5vw,4rem);
    font-weight:900; color:var(--white); line-height:1.05;
    margin:3rem 0 1.5rem; letter-spacing:-.01em;
  }
  .key-phrase em { color:var(--red-bright); font-style:italic; font-family:'Cormorant Garamond',serif; font-size:1.1em; font-weight:300; }
  .secondary-text {
    font-size:clamp(.95rem,1.6vw,1.15rem); font-weight:300; line-height:1.85;
    color:rgba(200,196,188,0.6); max-width:600px; margin:0 auto;
  }

  /* ── TROOP VIZ ── */
  .troop-viz { margin:4rem 0; }
  .troop-side { margin-bottom:2.5rem; }
  .troop-label { font-family:'Cinzel',serif; font-size:.65rem; letter-spacing:.3em; color:var(--gold); text-transform:uppercase; margin-bottom:.8rem; display:block; }
  .troop-bar-wrap { background:rgba(255,255,255,.04); border:1px solid rgba(184,145,42,.1); height:56px; position:relative; overflow:hidden; }
  .troop-bar {
    height:100%; width:0%; position:absolute; left:0; top:0;
    display:flex; align-items:center; justify-content:flex-end; padding-right:1rem;
    transition:width 2s cubic-bezier(.16,1,.3,1);
  }
  .troop-bar.garibaldi { background:linear-gradient(to right,rgba(139,26,26,.3),rgba(192,57,43,.7)); }
  .troop-bar.gómez    { background:linear-gradient(to right,rgba(80,80,100,.3),rgba(100,100,130,.6)); }
  .troop-bar-num { font-family:'Cinzel',serif; font-size:1.4rem; font-weight:900; color:var(--white); white-space:nowrap; }
  .troop-dots { display:flex; flex-wrap:wrap; gap:3px; margin-top:.6rem; }
  .troop-dot { width:7px; height:7px; border-radius:50%; opacity:0; transition:opacity .05s; }
  .troop-dot.garibaldi-dot { background:var(--red-bright); }
  .troop-dot.gómez-dot    { background:rgba(140,140,170,.7); }
  .troop-ratio {
    font-family:'Cormorant Garamond',serif; font-size:clamp(1.2rem,3vw,2rem);
    font-style:italic; color:var(--text); margin-top:2rem; text-align:center;
  }

  /* ── HORIZONTAL TIMELINE ── */
  .htimeline-section { min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:12vh 0; z-index:1; position:relative; }
  .htimeline-header { text-align:center; padding:0 6vw; margin-bottom:3rem; }
  .htimeline-outer { width:100%; overflow:hidden; position:relative; }
  .htimeline-track {
    display:flex; gap:0; cursor:grab; user-select:none;
    transition:transform .4s cubic-bezier(.16,1,.3,1);
    will-change:transform;
  }
  .htimeline-track:active { cursor:grabbing; }
  .htimeline-card {
    min-width:min(380px,85vw); padding:3rem 3rem 3rem 4rem;
    border-right:1px solid rgba(184,145,42,.15);
    position:relative; flex-shrink:0;
  }
  .htimeline-card::before {
    content:''; position:absolute; left:0; top:0; bottom:0; width:3px;
    background:linear-gradient(to bottom,transparent,var(--gold),transparent);
    opacity:0; transition:opacity .4s;
  }
  .htimeline-card.active::before { opacity:1; }
  .htimeline-dot {
    position:absolute; left:-5px; top:3rem;
    width:10px; height:10px; border-radius:50%;
    background:rgba(184,145,42,.3); border:1px solid rgba(184,145,42,.4);
    transition:background .4s, box-shadow .4s;
  }
  .htimeline-card.active .htimeline-dot { background:var(--gold); box-shadow:0 0 12px var(--gold); }
  .htimeline-time { font-family:'Cinzel',serif; font-size:.6rem; letter-spacing:.3em; color:var(--gold); text-transform:uppercase; display:block; margin-bottom:.8rem; }
  .htimeline-text { font-size:1.05rem; font-weight:300; line-height:1.75; color:var(--text); }
  .htimeline-nav { display:flex; gap:1rem; justify-content:center; margin-top:2.5rem; padding:0 6vw; }
  .htimeline-btn {
    font-family:'Cinzel',serif; font-size:.6rem; letter-spacing:.25em;
    color:var(--grey); background:none; border:1px solid rgba(184,145,42,.2);
    padding:.6rem 1.4rem; cursor:pointer; text-transform:uppercase;
    transition:all .3s;
  }
  .htimeline-btn:hover { color:var(--gold); border-color:rgba(184,145,42,.5); }
  .htimeline-progress { display:flex; gap:6px; justify-content:center; margin-top:1.5rem; }
  .htimeline-pip { width:20px; height:2px; background:rgba(184,145,42,.2); transition:background .3s; cursor:pointer; }
  .htimeline-pip.active { background:var(--gold); }
  .htimeline-hint { font-family:'Cinzel',serif; font-size:.55rem; letter-spacing:.25em; color:rgba(136,136,128,.5); text-transform:uppercase; margin-top:1rem; text-align:center; }

  /* ── PERSPECTIVA ── */
  .perspective-section { min-height:100vh; display:flex; align-items:center; justify-content:center; z-index:1; position:relative; padding:12vh 6vw; }
  .perspective-inner { max-width:800px; width:100%; text-align:center; }
  .perspective-tabs { display:flex; gap:2px; justify-content:center; margin:2.5rem 0; flex-wrap:wrap; }
  .perspective-tab {
    font-family:'Cinzel',serif; font-size:.62rem; letter-spacing:.25em;
    color:var(--grey); background:rgba(255,255,255,.02);
    border:1px solid rgba(184,145,42,.15); padding:.9rem 1.8rem;
    cursor:pointer; text-transform:uppercase; transition:all .35s;
    position:relative; overflow:hidden;
  }
  .perspective-tab::before {
    content:''; position:absolute; inset:0;
    background:linear-gradient(135deg,rgba(139,26,26,.15),transparent);
    opacity:0; transition:opacity .35s;
  }
  .perspective-tab:hover { color:var(--gold); border-color:rgba(184,145,42,.4); }
  .perspective-tab.active { color:var(--white); border-color:var(--gold); }
  .perspective-tab.active::before { opacity:1; }
  .perspective-icon { display:block; font-size:1.5rem; margin-bottom:.4rem; }
  .perspective-panels { position:relative; min-height:280px; }
  .perspective-panel {
    position:absolute; inset:0;
    opacity:0; transform:translateY(20px);
    transition:opacity .6s ease, transform .6s ease;
    pointer-events:none;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    padding:2.5rem; border:1px solid rgba(184,145,42,.12);
    background:rgba(255,255,255,.015);
  }
  .perspective-panel.active { opacity:1; transform:translateY(0); pointer-events:auto; position:relative; }
  .panel-eyebrow { font-family:'Cinzel',serif; font-size:.6rem; letter-spacing:.35em; color:var(--gold); text-transform:uppercase; margin-bottom:1.2rem; display:block; }
  .panel-text { font-size:clamp(1rem,1.8vw,1.25rem); font-weight:300; line-height:1.9; color:var(--text); font-style:italic; }

  /* ── SECTION TYPOGRAPHY ── */
  .section-label { font-family:'Cinzel',serif; font-size:.65rem; letter-spacing:.4em; color:var(--gold); text-transform:uppercase; margin-bottom:2rem; display:block; }
  .headline { font-family:'Cinzel',serif; font-size:clamp(2rem,6vw,5rem); font-weight:900; line-height:1.05; color:var(--white); margin-bottom:2rem; }
  .headline em { color:var(--red-bright); font-style:italic; font-family:'Cormorant Garamond',serif; font-size:1.15em; font-weight:300; }
  .body-text { font-size:clamp(1.1rem,2vw,1.35rem); font-weight:300; line-height:1.9; color:var(--text); margin-bottom:1.5rem; }
  .divider { width:60px; height:1px; background:var(--gold); margin:2rem auto; opacity:.5; }

  /* ── PERSONAS ── */
  .personas-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(250px,1fr)); gap:2px; margin:3rem 0; }
  .persona-card { background:rgba(255,255,255,.02); border:1px solid rgba(184,145,42,.15); padding:2.5rem 2rem; text-align:center; position:relative; overflow:hidden; }
  .persona-card::before { content:''; position:absolute; top:0; left:0; right:0; height:2px; background:linear-gradient(to right,transparent,var(--gold),transparent); }
  .persona-icon { font-size:2.5rem; margin-bottom:1rem; display:block; }
  .persona-name { font-family:'Cinzel',serif; font-size:1rem; font-weight:600; color:var(--white); letter-spacing:.1em; margin-bottom:.3rem; display:block; }
  .persona-role { font-family:'Cinzel',serif; font-size:.6rem; letter-spacing:.3em; color:var(--gold); text-transform:uppercase; margin-bottom:1rem; display:block; }
  .persona-desc { font-size:.95rem; font-weight:300; line-height:1.7; color:var(--text); }

  /* ── QUOTE ── */
  .quote-section { background:linear-gradient(135deg,rgba(139,26,26,.08) 0%,transparent 60%); }
  .quote-mark { font-family:'Cormorant Garamond',serif; font-size:8rem; line-height:.5; color:var(--red); opacity:.4; display:block; margin-bottom:-1rem; }
  blockquote { font-family:'Cormorant Garamond',serif; font-size:clamp(1.5rem,3.5vw,2.8rem); font-weight:300; font-style:italic; line-height:1.4; color:var(--white); margin-bottom:1.5rem; }
  .quote-attr { font-family:'Cinzel',serif; font-size:.75rem; letter-spacing:.25em; color:var(--gold); text-transform:uppercase; }

  /* ── WORLD GRID ── */
  .world-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(210px,1fr)); gap:2px; margin:3rem 0; }
  .world-item { padding:2rem 1.5rem; border:1px solid rgba(184,145,42,.1); background:rgba(255,255,255,.015); }
  .world-year { font-family:'Cinzel',serif; font-size:.6rem; letter-spacing:.3em; color:var(--gold); text-transform:uppercase; margin-bottom:.5rem; display:block; }
  .world-place { font-family:'Cinzel',serif; font-size:1rem; font-weight:600; color:var(--white); margin-bottom:.5rem; display:block; }
  .world-desc { font-size:.9rem; font-weight:300; line-height:1.6; color:var(--text); }

  /* ── MONUMENT ── */
  .monument-block { display:grid; grid-template-columns:1fr 1fr; gap:4rem; align-items:center; max-width:900px; margin:3rem auto 0; }
  .monument-photo-wrap { position:relative; border:1px solid rgba(184,145,42,.2); background:rgba(6,6,8,.7); overflow:hidden; }
  .monument-photo { width:100%; display:block; filter:grayscale(20%) contrast(1.05) brightness(.92); transition:filter .6s,transform .6s; }
  .monument-photo:hover { filter:grayscale(0%) brightness(1); transform:scale(1.02); }
  .monument-photo-caption { position:absolute; bottom:0; left:0; right:0; padding:.6rem 1rem; background:linear-gradient(to top,rgba(6,6,8,.85),transparent); font-family:'Cinzel',serif; font-size:.5rem; letter-spacing:.25em; color:var(--gold); text-transform:uppercase; text-align:center; }
  .monument-fact { font-family:'Cinzel',serif; font-size:clamp(1.3rem,2.5vw,2rem); font-weight:900; color:var(--gold); line-height:1.2; margin-bottom:1.5rem; text-align:left; }

  /* ── CTA ── */
  .cta-box { max-width:700px; margin:3rem auto 0; border:1px solid rgba(184,145,42,.25); padding:4rem 3rem; background:rgba(6,6,8,.8); backdrop-filter:blur(10px); position:relative; }
  .cta-box::before,.cta-box::after { content:''; position:absolute; width:20px; height:20px; border-color:var(--gold); border-style:solid; opacity:.5; }
  .cta-box::before { top:-1px; left:-1px; border-width:2px 0 0 2px; }
  .cta-box::after  { bottom:-1px; right:-1px; border-width:0 2px 2px 0; }
  .cta-title { font-family:'Cinzel',serif; font-size:clamp(1.3rem,3vw,2.2rem); font-weight:900; color:var(--white); margin-bottom:1.5rem; line-height:1.1; }
  .cta-points { list-style:none; text-align:left; margin:2rem 0; display:grid; grid-template-columns:1fr 1fr; gap:.8rem 2rem; }
  .cta-points li { font-size:.95rem; font-weight:300; color:var(--text); padding-left:1.2rem; position:relative; line-height:1.5; }
  .cta-points li::before { content:'—'; position:absolute; left:0; color:var(--gold); }

  /* ── FINALE ── */
  .finale { min-height:80vh; background:radial-gradient(ellipse at center bottom,rgba(139,26,26,.25) 0%,transparent 70%); display:flex; flex-direction:column; align-items:center; justify-content:center; text-align:center; padding:10vh 5vw; z-index:1; position:relative; }
  .finale-year { font-family:'Cinzel',serif; font-size:clamp(5rem,20vw,16rem); font-weight:900; color:transparent; -webkit-text-stroke:1px rgba(184,145,42,.25); line-height:1; user-select:none; }
  .finale-text { font-family:'Cormorant Garamond',serif; font-size:clamp(1.2rem,3vw,2rem); font-style:italic; font-weight:300; color:var(--white); max-width:600px; line-height:1.6; margin-top:-1rem; }
  .finale-sub { font-family:'Cinzel',serif; font-size:.65rem; letter-spacing:.4em; color:var(--gold); text-transform:uppercase; margin-top:2.5rem; }

  /* ── FOOTER ── */
  .site-footer { border-top:1px solid rgba(184,145,42,.15); padding:3rem 6vw; text-align:center; position:relative; z-index:1; }
  .footer-lang-links { display:flex; justify-content:center; gap:1.5rem; margin-bottom:1.5rem; }
  .footer-lang-links a { font-family:'Cinzel',serif; font-size:.65rem; letter-spacing:.25em; color:var(--grey); text-decoration:none; transition:color .3s; }
  .footer-lang-links a:hover,.footer-lang-links a.active { color:var(--gold); }
  .footer-text { font-size:.8rem; color:rgba(136,136,128,.5); font-style:italic; }

  @keyframes fadeUp  { from{opacity:0;transform:translateY(30px)} to{opacity:1;transform:translateY(0)} }
  @keyframes typing  { from{width:0} to{width:100%} }
  @keyframes blink   { 50%{border-color:transparent} }
  @keyframes scrollPulse { 0%,100%{opacity:.4} 50%{opacity:1} }

  @media(max-width:700px){
    .monument-block{grid-template-columns:1fr;gap:2rem;}
    .cta-points{grid-template-columns:1fr;}
    .monument-fact{text-align:center;}
    .htimeline-card{min-width:88vw;}
    .perspective-tabs{flex-direction:column;align-items:center;}
  }
</style>
</head>
<body>
<div class="progress-bar" id="progress"></div>
<canvas id="particles-canvas"></canvas>
<nav class="navbar" id="navbar">
  <a class="nav-brand" href="#">San Antonio · 1846</a>
  <ul class="nav-links">    <li><a href="#contexto">El escenario</a></li>
    <li><a href="#fuerzas">La batalla</a></li>
    <li><a href="#protagonistas">Protagonistas</a></li>
    <li><a href="#perspectiva">Perspectivas</a></li>
    <li><a href="#mundo">Legado</a></li>
    <li><a href="#monumento">Monumento</a></li>
    <li><a href="#cta">Visitar Salto</a></li></ul>
  <div class="nav-lang"><a href="batalla-san-antonio-es.html" class="active">ES</a><span class="nav-sep">·</span><a href="batalla-san-antonio-it.html">IT</a><span class="nav-sep">·</span><a href="batalla-san-antonio-en.html">EN</a></div>
</nav>

<!-- HERO -->
<section class="hero">
  <p class="hero-date">8 de Febrero · 1846 · Salto, Uruguay</p>
  <h1 class="hero-title">San Antonio<span>Resistirá.</span></h1>
  <p class="hero-subtitle">La historia del día en que 186 hombres detuvieron a un ejército — y cambiaron el mundo</p>
  <div class="scroll-cue"><div class="scroll-line"></div><span>Descender</span></div>
</section>

<!-- CONTEXTO -->
<section class="story-section" id="contexto" data-section="contexto">
  <div class="story-inner">
    <span class="section-label reveal">El escenario</span>
    <p class="key-phrase reveal reveal-delay-1">Uruguay ardía. <em>Garibaldi llegó.</em></p>
    <p class="secondary-text reveal reveal-delay-2">Era la Guerra Grande. Dos bandos desgarraban al país mientras Montevideo resistía el sitio de Manuel Oribe, apoyado por Juan Manuel de Rosas desde Argentina.</p>
    <div class="sentence-block"><span class="sentence">En 1842, Garibaldi formó en Montevideo la Legión Italiana.</span><span class="sentence">Los célebres camisas rojas, al servicio de la libertad.</span><span class="sentence">En noviembre de 1845, recuperó Salto del dominio sitiador.</span><span class="sentence">Y fue allí donde la historia lo esperaba.</span></div>
  </div>
</section>

<!-- FUERZAS -->
<section class="story-section" id="fuerzas" data-section="fuerzas">
  <div class="story-inner wide">
    <span class="section-label reveal">Los números</span>
    <p class="key-phrase reveal reveal-delay-1">Una minoría <em>que no retrocedió.</em></p>
    <p class="secondary-text reveal reveal-delay-2">La desproporción era aplastante. Pero Garibaldi eligió salir a campo abierto para evitar que la batalla destruyera la ciudad.</p>
    <div class="troop-viz reveal reveal-delay-3">
      <div class="troop-side">
        <span class="troop-label">Legión de Garibaldi · Infantería italiana y caballería de Báez</span>
        <div class="troop-bar-wrap"><div class="troop-bar garibaldi"><span class="troop-bar-num">0</span></div></div>
        <div class="troop-dots garibaldi-dots"></div>
      </div>
      <div class="troop-side">
        <span class="troop-label">Fuerzas de Servando Gómez · Infantería, caballería y lanzas</span>
        <div class="troop-bar-wrap"><div class="troop-bar gómez"><span class="troop-bar-num">0</span></div></div>
        <div class="troop-dots gómez-dots"></div>
      </div>
      <p class="troop-ratio">Un hombre de Garibaldi por cada tres del enemigo. Trece horas de combate.</p>
    </div>
  </div>
</section>

<!-- PROTAGONISTAS -->
<section class="story-section" id="protagonistas" data-section="protagonistas">
  <div class="story-inner wide">
    <span class="section-label reveal">Los protagonistas</span>
    <p class="key-phrase reveal reveal-delay-1">No fue solo <em>Garibaldi.</em></p>
    <p class="secondary-text reveal reveal-delay-2">Junto al jefe italiano, dos figuras fueron esenciales para que la resistencia fuera posible.</p>
    <div class="personas-grid" style="margin-top:3rem;">
      <div class="persona-card reveal reveal-delay-2">
        <span class="persona-icon">⚔️</span>
        <span class="persona-name">Giuseppe Garibaldi</span>
        <span class="persona-role">Coronel · Legión Italiana</span>
        <p class="persona-desc">Comandante en primera fila. Combatió las trece horas sin retroceder. Había llegado a América buscando causas de libertad donde luchar.</p>
      </div>
      <div class="persona-card reveal reveal-delay-3">
        <span class="persona-icon">🐎</span>
        <span class="persona-name">Bernardino Báez</span>
        <span class="persona-role">Coronel · Caballería Oriental</span>
        <p class="persona-desc">Jefe de la caballería uruguaya. Su actuación en los primeros minutos fue decisiva para frenar el avance federal.</p>
      </div>
      <div class="persona-card reveal reveal-delay-4">
        <span class="persona-icon">🏴</span>
        <span class="persona-name">Francisco Anzani</span>
        <span class="persona-role">Mayor · Segundo al mando</span>
        <p class="persona-desc">Lugarteniente de confianza de Garibaldi. Regresaría a Italia en 1848 para participar en el Risorgimento.</p>
      </div>
    </div>
  </div>
</section>

<!-- PERSPECTIVA -->
<section class="perspective-section" id="perspectiva" data-section="perspectiva">
  <div class="perspective-inner">
    <span class="section-label reveal">La batalla</span>
    <p class="key-phrase reveal reveal-delay-1">El mismo día, <em>tres miradas.</em></p>
    <p class="secondary-text reveal reveal-delay-2">Elige desde dónde quieres vivir la batalla del 8 de febrero.</p>
    <div class="perspective-tabs reveal reveal-delay-3"><button class="perspective-tab" data-tab="garibaldi"><span class="perspective-icon">⚔️</span>Garibaldi</button><button class="perspective-tab" data-tab="legionario"><span class="perspective-icon">🔴</span>Un legionario</button><button class="perspective-tab" data-tab="habitante"><span class="perspective-icon">🏘️</span>Un habitante</button></div>
    <div class="perspective-panels reveal reveal-delay-4"><div class="perspective-panel active" data-panel="garibaldi"><span class="panel-eyebrow">Desde los ojos de Garibaldi</span><p class="panel-text">Salgo de Salto antes del amanecer sabiendo que Gómez nos supera en número. No tengo opción de retroceder — atrás está la ciudad, están los civiles. Formo el cuadro. Les digo a mis hombres: aquí nos quedamos. Carga tras carga, los veo resistir. En doce horas no hemos cedido un palmo. Al retirarme ordenadamente hacia Salto, siento que hemos ganado algo más grande que una batalla.</p></div><div class="perspective-panel" data-panel="legionario"><span class="panel-eyebrow">Desde los ojos de un legionario</span><p class="panel-text">Soy italiano. Vine aquí buscando libertad. El calor del río es sofocante. Cuando llega la primera carga de caballería, mi corazón se detiene. Pero el cuadro aguanta. Recojo el fusil de un compañero caído. No hay tiempo para pensar. Solo disparar, recargar, aguantar. Cuando al final caminamos de regreso a Salto, no sé si he ganado o perdido. Solo sé que sigo vivo.</p></div><div class="perspective-panel" data-panel="habitante"><span class="panel-eyebrow">Desde los ojos de un habitante de Salto</span><p class="panel-text">Desde el pueblo escuchamos los disparos toda la mañana. Los niños preguntan qué pasa. Las mujeres rezan. Al mediodía el sonido no para. Por la tarde, tampoco. Al caer el sol, vemos regresar a los legionarios — heridos, agotados, pero caminando. Garibaldi llega el último. Salto sigue en pie.</p></div></div>
  </div>
</section>

<!-- TIMELINE HORIZONTAL -->
<section class="htimeline-section" id="batalla" data-section="batalla">
  <div class="htimeline-header">
    <span class="section-label reveal">8 de Febrero de 1846</span>
    <p class="key-phrase reveal reveal-delay-1">Hora a hora, <em>la batalla.</em></p>
  </div>
  <div class="htimeline-outer reveal reveal-delay-2">
    <div class="htimeline-track">
  <div class="htimeline-card">
    <div class="htimeline-dot"></div>
    <span class="htimeline-time">Amanecer</span>
    <p class="htimeline-text">Garibaldi sale de Salto hacia el norte. Sabe que Servando Gómez avanza con más de mil hombres. Elige enfrentarlo lejos de la ciudad.</p>
  </div>
  <div class="htimeline-card">
    <div class="htimeline-dot"></div>
    <span class="htimeline-time">Primer choque</span>
    <p class="htimeline-text">La vanguardia federal aparece antes de lo esperado. Garibaldi forma el cuadro de infantería y ordena aguantar. Báez cubre los flancos.</p>
  </div>
  <div class="htimeline-card">
    <div class="htimeline-dot"></div>
    <span class="htimeline-time">Media mañana</span>
    <p class="htimeline-text">Oleadas de caballería e infantería. Cada carga es rechazada. Los legionarios recogen armas de los caídos enemigos para seguir combatiendo.</p>
  </div>
  <div class="htimeline-card">
    <div class="htimeline-dot"></div>
    <span class="htimeline-time">Mediodía</span>
    <p class="htimeline-text">El cuadro resiste. Gómez no puede romperlo. Sus bajas se acumulan. La disciplina de la Legión Italiana es su mayor arma.</p>
  </div>
  <div class="htimeline-card">
    <div class="htimeline-dot"></div>
    <span class="htimeline-time">El giro</span>
    <p class="htimeline-text">El ímpetu federal se agota. Gómez, indeciso según testigos, no ordena el asalto final. La iniciativa pasa a Garibaldi.</p>
  </div>
  <div class="htimeline-card">
    <div class="htimeline-dot"></div>
    <span class="htimeline-time">Al caer la noche</span>
    <p class="htimeline-text">Tras trece horas, Garibaldi ordena retirada ordenada hacia Salto. La misión está cumplida: la ciudad, defendida.</p>
  </div>
    </div>
  </div>
  <div class="htimeline-nav reveal reveal-delay-3">
    <button class="htimeline-btn htl-prev">←</button>
    <button class="htimeline-btn htl-next">→</button>
  </div>
  <div class="htimeline-progress reveal reveal-delay-3"><div class="htimeline-pip"></div><div class="htimeline-pip"></div><div class="htimeline-pip"></div><div class="htimeline-pip"></div><div class="htimeline-pip"></div><div class="htimeline-pip"></div></div>
  <p class="htimeline-hint reveal reveal-delay-4">Arrastrá para navegar</p>
</section>

<!-- DECRETO -->
<section class="story-section quote-section" id="decreto" data-section="decreto">
  <div class="story-inner" style="max-width:750px;">
    <span class="quote-mark reveal">"</span>
    <blockquote class="reveal reveal-delay-1">Invencibles combatieron el 8 de febrero de 1846.</blockquote>
    <div class="divider reveal reveal-delay-2"></div>
    <p class="quote-attr reveal reveal-delay-2">Decreto del Gobierno de la Defensa · 25 de febrero de 1846</p>
    <p class="secondary-text reveal reveal-delay-3" style="margin-top:2rem;font-style:italic;">Con estas palabras, bordadas en laureles de oro sobre la bandera de la Legión Italiana, el gobierno uruguayo reconoció la hazaña. Los nombres de todos los combatientes fueron inscriptos en la Casa de Gobierno.</p>
  </div>
</section>

<!-- MUNDO -->
<section class="story-section" id="mundo" data-section="mundo">
  <div class="story-inner wide">
    <span class="section-label reveal">El legado global</span>
    <p class="key-phrase reveal reveal-delay-1">San Antonio lo <em>lanzó al mundo.</em></p>
    <div class="sentence-block"><span class="sentence">La batalla resonó en Europa.</span><span class="sentence">Italia soñaba con la unidad.</span><span class="sentence">El nombre de Garibaldi cruzó el Atlántico.</span><span class="sentence">Salto fue el trampolín.</span></div>
    <div class="world-grid" style="margin-top:3rem;">
<div class="world-item reveal reveal-delay-2">
  <span class="world-year">Sept. 1846</span><span class="world-place">Montevideo</span>
  <p class="world-desc">Ascendido a General. Su fama en Europa comienza a crecer.</p></div>
<div class="world-item reveal reveal-delay-3">
  <span class="world-year">1848</span><span class="world-place">Roma, Italia</span>
  <p class="world-desc">Defiende la República Romana. El Risorgimento lo convoca.</p></div>
<div class="world-item reveal reveal-delay-4">
  <span class="world-year">1860</span><span class="world-place">Sicilia y Nápoles</span>
  <p class="world-desc">Lidera los Mil. Hace posible la unificación de Italia.</p></div>
<div class="world-item reveal reveal-delay-5">
  <span class="world-year">1864</span><span class="world-place">Londres</span>
  <p class="world-desc">Recibido como héroe mundial. El Héroe de Dos Mundos.</p></div>
    </div>
  </div>
</section>

<!-- MONUMENTO -->
<section class="story-section" id="monumento" data-section="monumento">
  <div class="story-inner wide">
    <span class="section-label reveal">Patrimonio de Salto</span>
    <p class="key-phrase reveal reveal-delay-1">El monumento <em>más grande del mundo.</em></p>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:4rem;align-items:center;max-width:900px;margin:3rem auto 0;" class="monument-block">
      <div class="monument-photo-wrap reveal reveal-delay-2">
        <img src="data:image/jpeg;base64,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" alt="Monumento a Garibaldi" class="monument-photo"/>
        <p class="monument-photo-caption">Monumento · Arq. Juan Veltroni · Salto, Uruguay</p>
      </div>
      <div class="reveal reveal-delay-3">
        <p class="monument-fact">El mayor monumento a Garibaldi fuera de Italia está aquí, en Salto.</p>
        <div class="divider" style="margin:1.5rem 0;"></div>
        <div class="sentence-block"><span class="sentence">Obra del arquitecto Juan Veltroni.</span><span class="sentence">En el sitio exacto de la batalla.</span><span class="sentence">Con el decreto de Suárez grabado en piedra.</span><span class="sentence">Ninguna ciudad fuera de Italia rinde más homenaje al Héroe de Dos Mundos.</span></div>
        <p class="secondary-text" style="margin-top:1.5rem;">Salto guarda además un busto público, un óleo del siglo XIX, una carta manuscrita del propio Garibaldi y una colonia agrícola con su nombre.</p>
      </div>
    </div>
  </div>
</section>

<!-- CTA -->
<section class="story-section" id="cta" data-section="cta">
  <div class="story-inner">
    <span class="section-label reveal">Salto, hoy</span>
    <p class="key-phrase reveal reveal-delay-1">Una epopeya que <em>merece ser vivida.</em></p>
    <p class="secondary-text reveal reveal-delay-2">Salto tiene su propia hazaña fundacional — con todos los elementos para convertirla en un destino de turismo histórico de alcance internacional. La batalla de San Antonio es reconocida en Italia, en Brasil, en todo el mundo garibaldino.</p>
    <div class="cta-box reveal reveal-delay-3">
      <p class="cta-title">La Ruta Garibaldina de Salto</p>
      <p class="body-text" style="margin-bottom:.5rem;">Un circuito histórico que une los puntos clave de la gesta, en la ciudad que guarda el legado más completo del Héroe de Dos Mundos en América del Sur.</p>
      <ul class="cta-points"><li>El Monumento de Veltroni en Avenida Garibaldi</li>
<li>El sitio del combate, arroyo San Antonio</li>
<li>El óleo histórico del siglo XIX</li>
<li>La carta manuscrita de Garibaldi</li>
<li>La fortaleza en Plaza de los Treinta y Tres</li>
<li>Los legionarios que se quedaron en Salto</li></ul>
      <div class="divider" style="margin:2rem auto 1.5rem;"></div>
      <p class="quote-attr">Una historia que el mundo ya reconoce — Salto merece apropiársela.</p>
    </div>
  </div>
</section>

<!-- FINALE -->
<section class="finale">
  <p class="finale-year reveal">1846</p>
  <p class="finale-text reveal reveal-delay-2">Ciento ochenta años después, las orillas del San Antonio siguen guardando el eco de aquellos hombres que le dijeron <em>no</em> a lo imposible.</p>
  <p class="finale-sub reveal reveal-delay-3">Salto · Uruguay · 8 de Febrero · La hazaña que cambió el mundo</p>
</section>

<footer class="site-footer">
  <div class="footer-lang-links"><a href="batalla-san-antonio-es.html" class="active">ES</a> · <a href="batalla-san-antonio-it.html">IT</a> · <a href="batalla-san-antonio-en.html">EN</a></div>
  <p class="footer-text">© Salto, Uruguay — Campaña de revalorización histórica</p>
</footer>

<script>
// PROGRESS
const progressBar = document.getElementById('progress');
window.addEventListener('scroll',()=>{
  const t=document.body.scrollHeight-window.innerHeight;
  progressBar.style.width=(window.scrollY/t*100)+'%';
},{passive:true});

// NAVBAR
const navbar=document.querySelector('.navbar');
window.addEventListener('scroll',()=>{ navbar.classList.toggle('scrolled',window.scrollY>80); },{passive:true});

// REVEAL
const revObs=new IntersectionObserver(es=>es.forEach(e=>{if(e.isIntersecting)e.target.classList.add('visible');}),{threshold:.12});
document.querySelectorAll('.reveal').forEach(el=>revObs.observe(el));

// PARALLAX HERO
const hero=document.querySelector('.hero');
window.addEventListener('scroll',()=>{
  const y=window.scrollY;
  if(y<window.innerHeight){hero.style.transform=`translateY(${y*.28}px)`;hero.style.opacity=1-y/(window.innerHeight*.9);}
},{passive:true});

// SENTENCE LIGHTER — highlights each sentence as you scroll through it
document.querySelectorAll('.sentence-block').forEach(block=>{
  const sentences=block.querySelectorAll('.sentence');
  const obs=new IntersectionObserver(es=>{
    es.forEach(e=>{
      if(e.isIntersecting){
        sentences.forEach((s,i)=>{
          setTimeout(()=>{ s.classList.add('lit'); },i*220);
        });
      }
    });
  },{threshold:.2});
  obs.observe(block);
});

// BG COLOR SHIFT
const sectionBgs={contexto:'#060608',fuerzas:'#0c0608',protagonistas:'#07080a',batalla:'#0d0605',perspectiva:'#0a0506',decreto:'#0a0608',mundo:'#060a0a',monumento:'#090808',cta:'#0c0a04'};
const bgObs=new IntersectionObserver(es=>{
  es.forEach(e=>{if(e.isIntersecting){const c=sectionBgs[e.target.dataset.section]||'#060608';document.body.style.backgroundColor=c;}});
},{threshold:.35});
document.querySelectorAll('[data-section]').forEach(s=>bgObs.observe(s));

// TROOP VISUALIZATION
function animateTroops(){
  const viz=document.querySelector('.troop-viz');
  if(!viz||viz.dataset.animated) return;
  viz.dataset.animated=true;

  // Bars
  const bG=document.querySelector('.troop-bar.garibaldi');
  const bE=document.querySelector('.troop-bar.gómez');
  setTimeout(()=>{ bG.style.width='23%'; bG.querySelector('.troop-bar-num').textContent='~300'; },200);
  setTimeout(()=>{ bE.style.width='100%'; bE.querySelector('.troop-bar-num').textContent='+1.000'; },600);

  // Dots — 30 garibaldi, 100 enemy (scaled)
  const dotsG=document.querySelector('.troop-dots.garibaldi-dots');
  const dotsE=document.querySelector('.troop-dots.gómez-dots');
  for(let i=0;i<30;i++){
    const d=document.createElement('span');
    d.className='troop-dot garibaldi-dot';
    dotsG.appendChild(d);
    setTimeout(()=>d.style.opacity='1', 400+i*40);
  }
  for(let i=0;i<100;i++){
    const d=document.createElement('span');
    d.className='troop-dot gómez-dot';
    dotsE.appendChild(d);
    setTimeout(()=>d.style.opacity='1', 800+i*18);
  }
}
const troopObs=new IntersectionObserver(es=>{es.forEach(e=>{if(e.isIntersecting)animateTroops();});},{threshold:.3});
const tv=document.querySelector('.troop-viz');
if(tv) troopObs.observe(tv);

// HORIZONTAL TIMELINE DRAG
(function(){
  const track=document.querySelector('.htimeline-track');
  if(!track) return;
  const cards=track.querySelectorAll('.htimeline-card');
  const pips=document.querySelectorAll('.htimeline-pip');
  let current=0, startX=0, isDragging=false, startTranslate=0, currentTranslate=0;

  function goTo(idx){
    current=Math.max(0,Math.min(cards.length-1,idx));
    const cardW=cards[0].offsetWidth;
    currentTranslate=-current*cardW;
    track.style.transform=`translateX(${currentTranslate}px)`;
    cards.forEach((c,i)=>c.classList.toggle('active',i===current));
    pips.forEach((p,i)=>p.classList.toggle('active',i===current));
  }

  track.addEventListener('mousedown',e=>{isDragging=true;startX=e.clientX;startTranslate=currentTranslate;track.style.transition='none';});
  window.addEventListener('mouseup',e=>{
    if(!isDragging) return; isDragging=false;
    track.style.transition='';
    const diff=e.clientX-startX;
    if(diff<-50) goTo(current+1);
    else if(diff>50) goTo(current-1);
    else goTo(current);
  });
  window.addEventListener('mousemove',e=>{
    if(!isDragging) return;
    track.style.transform=`translateX(${startTranslate+(e.clientX-startX)}px)`;
  });
  track.addEventListener('touchstart',e=>{startX=e.touches[0].clientX;startTranslate=currentTranslate;track.style.transition='none';},{passive:true});
  track.addEventListener('touchend',e=>{
    track.style.transition='';
    const diff=e.changedTouches[0].clientX-startX;
    if(diff<-40) goTo(current+1); else if(diff>40) goTo(current-1); else goTo(current);
  });
  document.querySelector('.htl-prev')?.addEventListener('click',()=>goTo(current-1));
  document.querySelector('.htl-next')?.addEventListener('click',()=>goTo(current+1));
  pips.forEach((p,i)=>p.addEventListener('click',()=>goTo(i)));
  goTo(0);
})();

// PERSPECTIVE TABS
document.querySelectorAll('.perspective-tab').forEach(tab=>{
  tab.addEventListener('click',()=>{
    const key=tab.dataset.tab;
    document.querySelectorAll('.perspective-tab').forEach(t=>t.classList.remove('active'));
    document.querySelectorAll('.perspective-panel').forEach(p=>p.classList.remove('active'));
    tab.classList.add('active');
    document.querySelector(`.perspective-panel[data-panel="${key}"]`)?.classList.add('active');
  });
});

// PARTICLES
const canvas=document.getElementById('particles-canvas');
const ctx=canvas.getContext('2d');
function resize(){canvas.width=window.innerWidth;canvas.height=window.innerHeight;}
resize(); window.addEventListener('resize',resize);
class Particle{
  constructor(){this.reset(true);}
  reset(init){
    this.x=Math.random()*canvas.width;
    this.y=init?Math.random()*canvas.height:canvas.height+10;
    this.size=Math.random()*2+.4; this.speedY=-(Math.random()*.55+.15);
    this.speedX=(Math.random()-.5)*.25; this.life=Math.random();
    this.maxLife=Math.random()*.4+.3;
    this.color=Math.random()>.5?'139,26,26':'184,145,42';
  }
  update(){this.x+=this.speedX;this.y+=this.speedY;this.life-=.0025;if(this.y<-10||this.life<=0)this.reset(false);}
  draw(){const a=Math.max(0,this.life/this.maxLife)*.65;ctx.beginPath();ctx.arc(this.x,this.y,this.size,0,Math.PI*2);ctx.fillStyle=`rgba(${this.color},${a})`;ctx.fill();}
}
const particles=Array.from({length:60},()=>new Particle());
function animate(){ctx.clearRect(0,0,canvas.width,canvas.height);particles.forEach(p=>{p.update();p.draw();});requestAnimationFrame(animate);}
animate();
</script>

</body>
</html>
