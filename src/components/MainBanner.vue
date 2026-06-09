<template>
    <section class="hero" id="inicio">
  <div class="hero-media" aria-hidden="true">
    <video
      ref="heroVideoEl"
      class="hero-video"
      autoplay
      muted
      loop
      playsinline
      preload="auto"
    >
      <source
        src="https://res.cloudinary.com/dlwzazojt/video/upload/q_auto/f_auto/v1780522791/4fba5c84-0159-4925-bdfc-89c2e860907e_bqhqub.mp4"
        type="video/mp4"
      />
    </video>
    <div class="hero-video-bg"></div>
  </div>
  <div class="hero-overlay"></div>

  <div class="hero-content">
    <!-- LEFT -->
    <div class="hero-text">
      <div class="hero-eyebrow">
        <div class="hero-eyebrow-dot"></div>
        <span>Fabricante Industrial · Helados de alta calidad</span>
      </div>

      <div class="hero-rotating-wrap">
        <div class="hero-rotating" id="heroRotating">
          <span class="line active" id="line0">Expertos en desarrollar<br>tu marca de helado</span>
          <span class="line" id="line1">Expertos en helados<br>de marca propia</span>
          <span class="line" id="line2">Expertos maquiladores<br>de helado</span>
          <span class="line" id="line3">Creamos la marca de helado<br>de tus sueños</span>
          <span class="line" id="line4">
            <span class="cert-line">Certificados FDA ✓</span>
          </span>
        </div>
      </div>

      <p class="hero-sub">
        19 años fabricando helados premium para supermercados, restaurantes y distribuidores que quieren su propia marca sin asumir la complejidad de una planta industrial.
      </p>

      <div class="hero-btns">
        <button class="btn-primary-hero" onclick="document.getElementById('contacto').scrollIntoView({behavior:'smooth'})">
          <svg viewBox="0 0 24 24"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
          Solicitar Cotización
        </button>

      </div>
    </div>

    <!-- RIGHT stats -->
    <div class="hero-right">
      <div class="hero-stat-card reveal reveal-right d1">
        <div class="hsc-label">Capacidad de Producción</div>
        <div class="hsc-val"><em>30M</em> Litros</div>
        <div class="hsc-desc">de helado producidos al año</div>
      </div>
      <div class="hero-stat-card reveal reveal-right d2">
        <div class="hsc-label">Trayectoria</div>
        <div class="hsc-val"><em>19</em> Años</div>
        <div class="hsc-desc">de experiencia industrial</div>
      </div>
      <div class="hero-stat-card reveal reveal-right d3">
        <div class="hsc-label">Certificación</div>
        <div class="hsc-val">FDA <em>✓</em></div>
        <div class="hsc-desc">Habilitados para exportar a USA</div>
      </div>
      <div class="hero-stat-card reveal reveal-right d4">
        <div class="hsc-label">Tecnología</div>
        <div class="hsc-val">🇮🇹 <em>Italiana</em></div>
        <div class="hsc-desc">Maquinaria de última generación</div>
      </div>
    </div>
  </div>

  <div class="hero-scroll">
    Scroll
    <div class="hero-scroll-line"></div>
  </div>
</section>
</template>


<script setup lang="ts">
import { onMounted, ref } from 'vue';

const heroVideoEl = ref<HTMLVideoElement | null>(null);
/** Reproducción más lenta (~72 % de velocidad normal) */
const HERO_VIDEO_PLAYBACK_RATE = 0.72;

onMounted(() => {
  const video = heroVideoEl.value;
  if (!video) return;
  video.playbackRate = HERO_VIDEO_PLAYBACK_RATE;
  const onVideoMeta = () => {
    video.playbackRate = HERO_VIDEO_PLAYBACK_RATE;
  };
  video.addEventListener('loadedmetadata', onVideoMeta);
  if (video.readyState >= 1) onVideoMeta();
  void video.play().catch(() => {});
});

/* ═══ SCROLL REVEAL ═══ */
const io=new IntersectionObserver(es=>es.forEach(e=>{
  if(e.isIntersecting){e.target.classList.add('in');io.unobserve(e.target)}
}),{threshold:.1});
document.querySelectorAll('.reveal,.reveal-left,.reveal-right').forEach(el=>io.observe(el));

/* ═══ HERO ROTATING TEXT ═══ */
const lines=['line0','line1','line2','line3','line4'];
let cur=0;
setInterval(()=>{
  const prev = document.getElementById(lines[cur]);
  if (prev) prev.classList.remove('active');
  cur=(cur+1)%lines.length;
  const next = document.getElementById(lines[cur]);
  if (next) next.classList.add('active');
},3200);

/* ═══ COUNTER ANIMATION ═══ */
const counters=document.querySelectorAll('.counter');
const cio=new IntersectionObserver(es=>es.forEach(e=>{
  if(e.isIntersecting){
    const rawTarget = (e.target as HTMLElement).dataset?.target;
    const target = rawTarget ? Number(rawTarget) : 0;
    const start=Date.now();const dur=1800;
    const tick=()=>{
      const prog=Math.min((Date.now()-start)/dur,1);
      const ease=1-Math.pow(1-prog,3);
      (e.target as HTMLElement).textContent=String(Math.round(ease*target));
      if(prog<1)requestAnimationFrame(tick);
    };tick();
    cio.unobserve(e.target);
  }
}),{threshold:.5});
counters.forEach(c=>cio.observe(c));

</script>

<style scoped>

/* ═══════════════════════════════════════════════
   HERO
═══════════════════════════════════════════════ */
.hero{
  min-height:100vh;
  min-height:100svh;
  position:relative;
  display:flex;align-items:center;
  overflow:hidden;
  background:var(--secondary);
  margin-top:0;
}

/*
  Extender a todo el ancho sin recortar alto:
  width 100% + height auto → el alto se calcula solo (proporción del video).
  No uses width/height fijos en vh/vw: deforman o recortan.
*/
.hero-media{
  position:absolute;
  left:0;
  right:0;
  top:50%;
  transform:translateY(-50%);
  width:100%;
  z-index:0;
  line-height:0;
  pointer-events:none;
}
.hero-video-bg{
  position:absolute;inset:0;z-index:0;
  border-radius:var(--r-lg);
  background:linear-gradient(135deg,#1A1830 0%,#312E57 40%,#1A3A6B 100%);
  opacity:.35;
}
.hero-video-bg::before{
  content:'';position:absolute;inset:0;
  border-radius:inherit;
  background-image:
    linear-gradient(rgba(118,180,242,.04) 1px,transparent 1px),
    linear-gradient(90deg,rgba(118,180,242,.04) 1px,transparent 1px);
  background-size:48px 48px;
}
.hero-video-bg::after{
  content:'';position:absolute;inset:0;
  border-radius:inherit;
  background:radial-gradient(ellipse 80% 80% at 70% 50%,rgba(118,180,242,.12) 0%,transparent 70%);
}
.hero-video{
  position:relative;
  display:block;
  width:100%;
  height:auto;
  max-width:100%;
  object-fit:contain;
  object-position:center center;
  opacity:0.6;
  z-index:1;
}
.hero-overlay{
  position:absolute;inset:0;z-index:2;
  background:
    linear-gradient(180deg,rgba(26,24,48,.25) 0%,rgba(49,46,87,.45) 42%,rgba(49,46,87,.72) 100%),
    linear-gradient(90deg,rgba(49,46,87,.58) 0%,rgba(49,46,87,.32) 48%,rgba(49,46,87,.08) 100%);
}

.hero-content{
  position:relative;z-index:3;
  max-width:1280px;margin:0 auto;width:100%;
  padding:calc(72px + 28px) 48px 80px;
  display:grid;grid-template-columns:1fr 1fr;
  gap:60px;align-items:center;
}

/* LEFT — text */
.hero-text{}
.hero-eyebrow{
  display:inline-flex;align-items:center;gap:9px;
  background:rgba(118,180,242,.12);
  border:1px solid rgba(118,180,242,.25);
  border-radius:100px;padding:6px 16px;
  margin-bottom:28px;
}
.hero-eyebrow-dot{
  width:7px;height:7px;background:var(--primary);
  border-radius:50%;animation:pulse 2s ease-in-out infinite;
}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.5;transform:scale(.7)}}
.hero-eyebrow span{
  font-family:var(--ff-display);
  font-size:.65rem;font-weight:500;
  letter-spacing:.12em;text-transform:uppercase;
  color:var(--primary);
}

/* Rotating text hero */
.hero-rotating-wrap{margin-bottom:20px}
.hero-rotating{
  font-family:var(--ff-display) !important;
  font-size:clamp(2rem,4.5vw,3.8rem);
  font-weight:800;letter-spacing:-.04em;
  line-height:1.0;color:var(--white);
  min-height:1.2em;position:relative;
}
.hero-rotating .line{
  position:absolute;top:0;left:0;
  opacity:0;transform:translateY(24px);
  transition:opacity .7s var(--ease-out),transform .7s var(--ease-out);
}
.hero-rotating .line.active{
  position:relative;opacity:1;transform:translateY(0);
}
.hero-rotating .cert-line{
  color:var(--primary);display:block;
  font-size:clamp(1.5rem,3.5vw,2.8rem);
  margin-top:8px;
}

.hero-sub{
  font-size:1rem;font-weight:300;
  color:rgba(255,255,255,.6);
  line-height:1.8;max-width:460px;
  margin-bottom:40px;
}
.hero-btns{display:flex;gap:14px;flex-wrap:wrap}
.btn-primary-hero{
  display:inline-flex;align-items:center;gap:10px;
  padding:15px 32px;border-radius:100px;
  background:var(--accent);color:var(--white);
  font-family:var(--ff-display);
  font-size:.72rem;font-weight:700;letter-spacing:.04em;
  border:none;
  transition:transform .3s var(--ease-out),box-shadow .3s;
  box-shadow:0 8px 28px rgba(208,79,109,.35);
}
.btn-primary-hero:hover{transform:translateY(-3px);box-shadow:0 16px 40px rgba(208,79,109,.5)}
.btn-primary-hero svg{width:16px;height:16px;fill:white}
.btn-wa{
  display:inline-flex;align-items:center;gap:10px;
  padding:15px 32px;border-radius:100px;
  background:rgba(255,255,255,.1);
  border:1.5px solid rgba(255,255,255,.2);
  color:var(--white);
  font-family:var(--ff-display);
  font-size:.72rem;font-weight:600;letter-spacing:.04em;
  transition:background .25s,border-color .25s;
  backdrop-filter:blur(10px);
}
.btn-wa:hover{background:rgba(255,255,255,.18);border-color:rgba(255,255,255,.4)}
.btn-wa svg{width:18px;height:18px;fill:var(--white)}

/* RIGHT — stats card cluster */
.hero-right{
  display:flex;flex-direction:column;gap:14px;
  align-items:flex-end;
}
.hero-stat-card{
  background:rgb(26 115 232 / 8%);
  backdrop-filter:blur(20px);
  border:1px solid rgba(118,180,242,.15);
  border-radius:var(--r-lg);
  padding:17px 24px;
  width:100%;max-width:320px;
  transition:transform .3s var(--ease-out),border-color .3s;
}
.hero-stat-card:hover{transform:translateY(-4px);border-color:rgba(118,180,242,.4)}
.hsc-label{
  font-size:.65rem;font-weight:500;
  letter-spacing:.1em;text-transform:uppercase;
  color:rgba(255,255,255,.45);
}
.hsc-val{
  font-family:var(--ff-display);
  font-size:1.7rem;font-weight:800;
  color:var(--white);letter-spacing:-.03em;
}
.hsc-val em{color:var(--primary);font-style:normal}
.hsc-desc{font-size:.75rem;color:rgba(255,255,255,.5)}

.hero-scroll{
  position:absolute;bottom:32px;left:50%;transform:translateX(-50%);
  z-index:3;display:flex;flex-direction:column;align-items:center;gap:8px;
  color:rgba(255,255,255,.35);
  font-size:.62rem;font-weight:500;letter-spacing:.12em;text-transform:uppercase;
  animation:scrollBob 2.5s ease-in-out infinite;
}
@keyframes scrollBob{0%,100%{transform:translateX(-50%) translateY(0)}50%{transform:translateX(-50%) translateY(-6px)}}
.hero-scroll-line{
  width:1px;height:48px;
  background:linear-gradient(to bottom,rgba(255,255,255,.4),transparent);
}

/* Responsive */
@media (max-width: 768px) {
  .hero-content {
    display: flex;
    flex-direction: column;
    padding: calc(72px + 20px) 24px 60px;
    gap: 0;
  }

  .hero-text {
    display: contents;
  }

  .hero-eyebrow,
  .hero-rotating-wrap,
  .hero-sub {
    order: 1;
  }

  .hero-right {
    order: 2;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    align-items: stretch;
    width: 100%;
    margin: 24px 0;
  }

  .hero-stat-card {
    max-width: 100%;
  }

  .hero-eyebrow {
    margin-bottom: 20px;
  }

  .hero-sub {
    font-size: 0.92rem;
    line-height: 1.65;
    max-width: 100%;
    margin-bottom: 0;
  }

  .hero-btns {
    order: 3;
    justify-content: center;
    margin-top: 8px;
  }

  .btn-primary-hero,
  .btn-wa {
    padding: 13px 24px;
  }

  .hero-media {
    top: 0;
    transform: none;
  }

  .hero-video {
    min-height: 85vh;
    object-fit: cover;
  }
}

@media (max-width: 480px) {
  .hero-content {
    padding: calc(72px + 16px) 20px 48px;
  }

  .hero-right {
    gap: 8px;
    margin: 20px 0;
  }

  .hero-rotating {
    font-size: clamp(1.6rem, 9vw, 2.4rem);
  }

  .hero-rotating .cert-line {
    font-size: clamp(1.2rem, 7vw, 1.8rem);
  }

  .hero-sub {
    font-size: 0.85rem;
  }

  .hero-stat-card {
    padding: 12px 14px;
  }

  .hsc-val {
    font-size: 1.25rem;
  }

  .hsc-label {
    font-size: 0.58rem;
  }

  .hsc-desc {
    font-size: 0.7rem;
  }

  .hero-video {
    min-height: 150vh;
  }

  .hero-scroll {
    display: none;
  }
}

</style>
