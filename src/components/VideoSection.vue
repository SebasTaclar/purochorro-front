<template>
  <!-- ══ VIDEO INSTITUCIONAL ══ -->
  <div class="video-section">
    <div style="text-align:center;margin-bottom:36px">
      <div class="eyebrow eyebrow-dark" style="justify-content:center"><div class="eyebrow-line"></div> Video Institucional</div>
      <h2 class="section-title section-title-white reveal">La fábrica detrás<br>de <span>tu marca</span></h2>
    </div>
    <div class="video-wrap reveal">
      <iframe
        src="https://www.youtube.com/embed/xGjQ-bp3tCo"
        title="Video Institucional ODA Gelato"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
      ></iframe>
    </div>
  </div>
</template>

<script setup lang="ts">
defineOptions({ name: 'VideoSection' });
</script>

<style scoped>
/* ═══════════════════════════════════════════════
   SECTION 2 — VIDEO INSTITUCIONAL
═══════════════════════════════════════════════ */
.video-section{background:var(--secondary);padding:50px 48px}
.video-wrap{
  max-width:900px;margin:0 auto;
  position:relative;border-radius:var(--r-xl);
  overflow:hidden;
  aspect-ratio:16/9;
  background:linear-gradient(135deg,#1A1830,#2A2650);
  box-shadow:var(--shadow-xl);
  display:flex;align-items:center;justify-content:center;
}
.video-wrap iframe{
  width:100%;
  height:100%;
  border:none;
  position:absolute;
  inset:0;
}

.eyebrow{
  display:inline-flex;align-items:center;gap:8px;
  font-family:var(--ff-display);
  font-size:.62rem;font-weight:600;
  letter-spacing:.14em;text-transform:uppercase;
  color:var(--primary-dark);margin-bottom:14px;
}
.eyebrow-dark{color:var(--primary)}
.eyebrow-line{width:28px;height:2px;background:currentColor;border-radius:2px}


.section-title{
  font-family:var(--ff-display);
  font-size:clamp(1.8rem,4vw,3rem);
  font-weight:700;letter-spacing:-.04em;
  color:var(--secondary);line-height:1.05;
  margin-bottom:16px;
}

.section-title-white{color:var(--white)}
.section-title span{color:var(--primary)}
.section-desc{
  font-size:1rem;font-weight:300;
  color:var(--gray);line-height:1.8;
  max-width:520px;
}
.section-desc-white{color:rgba(255,255,255,.6)}


</style>
