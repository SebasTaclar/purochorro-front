<template>
  <!-- ══════════════════════════════════════════
   CTA PRINCIPAL
══════════════════════════════════════════ -->
<section class="section cta-section">
  <div class="section-inner">
    <div class="cta-inner reveal">
      <div class="eyebrow eyebrow-dark">
        <div class="eyebrow-line"></div>
        <span>¿LISTO PARA COMENZAR?</span>
      </div>
      <h2 class="cta-title">Desarrolla tu marca de helado<br>con <span>ODA GELATO</span></h2>
      <p class="cta-sub">
        Sin inversión en planta. Sin complejidad operativa.<br>
        Solo tu marca, nuestro expertise y el helado de tus sueños.
      </p>
      <div class="cta-btns">
        <button class="btn-primary-hero" onclick="document.getElementById('contacto').scrollIntoView({behavior:'smooth'})">
          <svg viewBox="0 0 24 24"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
          Solicitar Cotización Ahora
        </button>
        <a class="btn-wa" href="https://wa.me/573202002489?text=Hola%2C%20quiero%20desarrollar%20mi%20marca%20de%20helado%20con%20ODA%20GELATO" target="_blank" rel="noopener">
          <svg viewBox="0 0 24 24"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg>
          WhatsApp Directo
        </a>
      </div>
    </div>
  </div>
</section>
</template>

<script setup lang="ts">
</script>

<style scoped>
.cta-section {
  padding: 100px 24px;
  text-align: center;
  background: var(--gray-bg, #f4f6fb);
}
.container {
  max-width: 720px;
  margin: 0 auto;
}
.cta-pre {
  font-size: .85rem;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: var(--primary);
  font-weight: 600;
  margin-bottom: 16px;
}
.cta-title {
  font-family: var(--ff-display);
  font-size: 2.4rem;
  line-height: 1.2;
  color: var(--secondary);
  margin-bottom: 20px;
}
.cta-desc {
  font-size: 1.05rem;
  line-height: 1.7;
  color: rgba(26, 24, 48, .6);
}
@media (max-width: 768px) {
  .cta-section { padding: 64px 20px; }
  .cta-title { font-size: 1.6rem; }
}

/* ═══════════════════════════════════════════════
   SECTION 9 — CTA PRINCIPAL
═══════════════════════════════════════════════ */
.cta-section{
  background:linear-gradient(135deg,var(--secondary) 0%,#1A1830 100%);
  position:relative;overflow:hidden;
}
.cta-section::before{
  content:'';position:absolute;inset:0;
  background:radial-gradient(ellipse 70% 70% at 80% 50%,rgba(118,180,242,.12) 0%,transparent 70%);
}
.cta-inner{
  max-width:900px;margin:0 auto;text-align:center;
  position:relative;z-index:1;
}
.cta-title{
  font-family:var(--ff-display);
  font-size:clamp(2rem,5vw,4rem);
  font-weight:800;letter-spacing:-.04em;
  color:var(--white);line-height:1.0;
  margin-bottom:20px;
}
.cta-title span{color:var(--primary)}
.cta-sub{
  font-size:1rem;font-weight:300;
  color:rgba(255,255,255,.55);
  line-height:1.8;margin-bottom:44px;
}
.cta-btns{display:flex;gap:14px;justify-content:center;flex-wrap:wrap}

.eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 20px;
  font-family: var(--ff-display);
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--primary);
}
.eyebrow-line {
  width: 32px;
  height: 2px;
  background: var(--primary);
  border-radius: 1px;
}

.btn-primary-hero {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 16px 36px;
  border-radius: 100px;
  background: var(--accent);
  color: var(--white);
  font-family: var(--ff-display);
  font-size: .8rem;
  font-weight: 700;
  letter-spacing: .04em;
  border: none;
  cursor: pointer;
  text-decoration: none;
  transition: transform .3s var(--ease-out), box-shadow .3s;
  box-shadow: 0 8px 28px rgba(208,79,109,.35);
}
.btn-primary-hero:hover {
  transform: translateY(-3px);
  box-shadow: 0 16px 40px rgba(208,79,109,.5);
}
.btn-primary-hero svg {
  width: 16px;
  height: 16px;
  fill: currentColor;
}

.btn-wa {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 16px 36px;
  border-radius: 100px;
  background: rgba(255,255,255,.08);
  border: 1.5px solid rgba(255,255,255,.2);
  color: var(--white);
  font-family: var(--ff-display);
  font-size: .8rem;
  font-weight: 600;
  letter-spacing: .04em;
  text-decoration: none;
  transition: background .25s, border-color .25s, transform .3s var(--ease-out);
  backdrop-filter: blur(10px);
}
.btn-wa:hover {
  background: rgba(255,255,255,.18);
  border-color: rgba(255,255,255,.4);
  transform: translateY(-3px);
}
.btn-wa svg {
  width: 18px;
  height: 18px;
  fill: currentColor;
}

@media (max-width: 520px) {
  .btn-primary-hero, .btn-wa {
    width: 100%;
    justify-content: center;
  }
}
</style>
