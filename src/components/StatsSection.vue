<template>
  <!-- ══ STATS ══ -->
<div class="stats-section">
  <div class="stats-grid">
    <div class="stat-item reveal d1">
      <!-- <span class="stat-icon">🏭</span> -->
      <div class="stat-num"><span class="counter" data-target="19">19</span><span class="unit">años</span></div>
      <div class="stat-label">De experiencia en manufactura</div>
    </div>
    <div class="stat-item reveal d2">
      <!-- <span class="stat-icon">❄️</span> -->
      <div class="stat-num"><span class="counter" data-target="30">30</span><span class="unit">M lts</span></div>
      <div class="stat-label">Capacidad de producción anual</div>
    </div>
    <div class="stat-item reveal d3">
      <span class="stat-icon">🇺🇸</span>
      <div class="stat-num" style="font-size:1.4rem">FDA</div>
      <div class="stat-label">Habilitados para exportar a EE.UU.</div>
    </div>
    <div class="stat-item reveal d4">
      <span class="stat-icon">🇮🇹</span>
      <div class="stat-num" style="font-size:1.4rem">Tech</div>
      <div class="stat-label">Tecnología italiana de última generación</div>
    </div>
    <div class="stat-item reveal d5">
      <span class="stat-icon">🔬</span>
      <div class="stat-num" style="font-size:1.4rem">I+D</div>
      <div class="stat-label">Laboratorio propio de desarrollo</div>
    </div>
  </div>
</div>
</template>

<script setup lang="ts">
defineOptions({ name: 'StatsSection' });
</script>

<style scoped>
/* ═══════════════════════════════════════════════
   SECTION 3 — STATS COUNTER
═══════════════════════════════════════════════ */
.stats-section{background:var(--primary);padding:5px 48px}
.stats-grid{
  max-width:1200px;margin:0 auto;
  display:grid;grid-template-columns:repeat(5,1fr);gap:2px;
}
.stat-item{
  padding:36px 24px;text-align:center;
  position:relative;
}
.stat-item::after{
  content:'';position:absolute;right:0;top:20%;bottom:20%;
  width:1px;background:rgba(49,46,87,.12);
}
.stat-item:last-child::after{display:none}
.stat-num{
  font-family:var(--ff-display);
  font-size:clamp(2rem,3.5vw,3rem);
  font-weight:800;color:var(--secondary);
  letter-spacing:-.04em;margin-bottom:8px;
  display:flex;align-items:baseline;justify-content:center;gap:3px;
}
.stat-num .unit{font-size:1rem;font-weight:500;color:var(--secondary-mid)}
.stat-label{
  font-size:.78rem;font-weight:600;
  color:rgba(49,46,87,.7);letter-spacing:.02em;
  line-height:1.4;
}
.stat-icon{
  font-size:1.5rem;margin-bottom:10px;
  display:block;
}

@media (max-width: 768px) {
  .stats-section { padding: 5px 24px; }
  .stats-grid { grid-template-columns: repeat(3, 1fr); }
  .stat-item { padding: 28px 16px; }
  .stat-item:nth-child(3)::after { display: none; }
}

@media (max-width: 480px) {
  .stats-grid { grid-template-columns: repeat(2, 1fr); }
  .stat-item { padding: 24px 12px; }
  .stat-item:nth-child(2)::after,
  .stat-item:nth-child(4)::after { display: none; }
  .stat-item:last-child {
    grid-column: span 2;
    justify-self: center;
    width: 50%;
  }
  .stat-num { font-size: clamp(1.6rem, 8vw, 2.2rem); }
  .stat-label { font-size: 0.7rem; }
  .stat-icon { font-size: 1.2rem; margin-bottom: 6px; }
}

</style>
