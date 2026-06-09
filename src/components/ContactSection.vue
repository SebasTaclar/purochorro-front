<template>
  <!-- ══════════════════════════════════════════
   CONTACTO
══════════════════════════════════════════ -->
<section class="section section-alt" id="contacto">
  <div class="section-inner">
    <div class="contact-grid">
      <div class="reveal reveal-left">
        <div class="eyebrow"><div class="eyebrow-line"></div> Contacto Comercial</div>
        <h2 class="section-title">Hablemos de<br><span>tu proyecto</span></h2>
        <p class="section-desc" style="margin-bottom:36px">
          Cuéntanos tu idea y nuestro equipo comercial te acompañará en todo el proceso, desde el desarrollo del sabor hasta la entrega final.
        </p>

        <div class="contact-detail">
          <div class="cd-icon"><svg viewBox="0 0 24 24"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M22 7l-10 7L2 7"/></svg></div>
          <div>
            <div class="cd-label">Correo Comercial</div>
            <div class="cd-val"><a href="mailto:servicioalcliente@odagelato.com.co">servicioalcliente@odagelato.com.co</a></div>
          </div>
        </div>
        <div class="contact-detail">
          <div class="cd-icon"><svg viewBox="0 0 24 24"><path d="M22 16.92v3a2 2 0 01-2.18 2 19.79 19.79 0 01-8.63-3.07A19.5 19.5 0 014.69 13.6a19.79 19.79 0 01-3.07-8.67A2 2 0 013.56 3h3a2 2 0 012 1.72c.127.96.361 1.903.7 2.81a2 2 0 01-.45 2.11L8.09 10.17a16 16 0 005.74 5.74l1.27-1.27a2 2 0 012.11-.45 12.84 12.84 0 002.81.7A2 2 0 0122 16.92z"/></svg></div>
          <div>
            <div class="cd-label">WhatsApp</div>
            <div class="cd-val"><a href="https://wa.me/573202002489">+57 320 200 2489</a></div>
          </div>
        </div>
        <div class="contact-detail">
          <div class="cd-icon"><svg viewBox="0 0 24 24"><path d="M21 10c0 7-9 13-9 13S3 17 3 10a9 9 0 0118 0z"/><circle cx="12" cy="10" r="3"/></svg></div>
          <div>
            <div class="cd-label">Ubicación</div>
            <div class="cd-val"><a href="https://maps.google.com/?q=Zona+Franca+Intexzona,+Km.+1+via+Siberia+-+Funza,+Bodega+10,+Cota,+Cundinamarca,+Colombia" target="_blank" rel="noopener">Zona Franca Intexzona, Km. 1 via Siberia — Funza, Bodega 10, Cota, Cundinamarca, Colombia</a></div>
          </div>
        </div>

        <!-- Google Maps -->
        <div class="contact-map">
          <iframe
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3976.1221385190693!2d-74.16313611139063!3d4.7488138010352134!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x8e3f837abd23220f%3A0x9af3b3fa2991c9c4!2sOda%20Gelato!5e0!3m2!1ses!2sus!4v1780621351453!5m2!1ses!2sus"
            width="100%"
            height="180"
            style="border:0;"
            :allowfullscreen="true"
            loading="lazy"
            referrerpolicy="no-referrer-when-downgrade"
            title="Ubicación ODA Gelato"
          ></iframe>
        </div>
      </div>

      <div class="contact-form reveal reveal-right">
        <div class="form-title">Solicitar Cotización</div>
        <div class="form-row">
          <div class="form-group">
            <label class="form-label">Nombre *</label>
            <input class="form-input" type="text" placeholder="Tu nombre completo" required>
          </div>
          <div class="form-group">
            <label class="form-label">Empresa *</label>
            <input class="form-input" type="text" placeholder="Nombre de tu empresa">
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label class="form-label">Correo *</label>
            <input class="form-input" type="email" placeholder="correo@empresa.com" required>
          </div>
          <div class="form-group">
            <label class="form-label">Teléfono</label>
            <input class="form-input" type="tel" placeholder="+57 300 000 0000">
          </div>
        </div>
        <div class="form-group full">
          <label class="form-label">¿Qué necesitas?</label>
          <select class="form-select">
            <option value="">Selecciona una opción</option>
            <option>Desarrollar mi marca de helado</option>
            <option>Maquila de producción</option>
            <option>Exportación / Distribución</option>
            <option>Desarrollo de nuevos sabores</option>
            <option>Información general</option>
          </select>
        </div>
        <div class="form-group full">
          <label class="form-label">Mensaje</label>
          <textarea class="form-textarea" placeholder="Cuéntanos sobre tu proyecto, volumen estimado, mercado objetivo..."></textarea>
        </div>
        <button class="btn-submit" onclick="handleContactSubmit(event)">
          Enviar Solicitud →
        </button>
      </div>
    </div>
  </div>
</section>
</template>

<script setup lang="ts">
defineOptions({
  name: 'ContactSection'
})

import { onMounted, onBeforeUnmount, ref } from 'vue'

const mapHostEl = ref<HTMLElement | null>(null)
const shouldLoadMap = ref(false)
let onDomReady: (() => void) | null = null

onMounted(() => {
  if (typeof window === 'undefined') return
  if (shouldLoadMap.value) return

  const load = () => {
    shouldLoadMap.value = true
  }

  const schedule = () => {
    const w = window as unknown as {
      requestIdleCallback?: (cb: () => void, opts?: { timeout: number }) => void
    }

    // Carga pronto, pero evitando bloquear el render inicial.
    if (typeof w.requestIdleCallback === 'function') {
      w.requestIdleCallback(load, { timeout: 1500 })
    } else {
      window.setTimeout(load, 0)
    }
  }

  // Más rápido que esperar window.load.
  if (document.readyState === 'interactive' || document.readyState === 'complete') {
    schedule()
    return
  }

  onDomReady = () => schedule()
  window.addEventListener('DOMContentLoaded', onDomReady, { once: true })
})

onBeforeUnmount(() => {
  if (onDomReady) {
    window.removeEventListener('DOMContentLoaded', onDomReady)
    onDomReady = null
  }
})
</script>

<style scoped>

/* ═══════════════════════════════════════════════
   SECTION 10 — CONTACTO
═══════════════════════════════════════════════ */
.section{padding:100px 48px;position:relative}
.section-inner{max-width:1280px;margin:0 auto}

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

.contact-grid{
  display:grid;grid-template-columns:1fr 1.2fr;
  gap:72px;align-items:start;
}
@media (max-width:768px){
  .contact-grid{grid-template-columns:1fr;gap:32px}
  .contact-form{padding:24px}
  .form-row{grid-template-columns:1fr}
  .form-input,.form-select,.form-textarea{width:100%;box-sizing:border-box}
}



.contact-info{}
.contact-info .section-desc{margin-bottom:32px}
.contact-detail{
  display:flex;gap:14px;align-items:flex-start;
  margin-bottom:20px;
}
.cd-icon{
  width:40px;height:40px;flex-shrink:0;
  background:var(--primary-wash);border-radius:var(--r-sm);
  display:flex;align-items:center;justify-content:center;
  border:1px solid rgba(118,180,242,.2);
}
.cd-icon svg{width:18px;height:18px;stroke:var(--primary-dark);stroke-width:2;fill:none;stroke-linecap:round;stroke-linejoin:round}
.cd-label{font-size:.65rem;font-weight:600;color:var(--gray);letter-spacing:.06em;text-transform:uppercase;margin-bottom:3px}
.cd-val{font-size:.85rem;color:var(--secondary);font-weight:500}
.cd-val a{text-decoration:none;color:inherit}
.cd-val a:hover{color:var(--primary)}

.contact-form{
  background:var(--white);
  border:1.5px solid rgba(118,180,242,.12);
  border-radius:var(--r-xl);
  padding:40px;
  box-shadow:var(--shadow-md);
}
.form-title{
  font-family:var(--ff-display);
  font-size:1.1rem;font-weight:700;
  color:var(--secondary);margin-bottom:24px;
  letter-spacing:-.02em;
}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:14px}
@media (max-width:768px){.form-row{grid-template-columns:1fr}}
.form-group{display:flex;flex-direction:column;gap:6px;margin-bottom:14px}
.form-group.full{grid-column:1/-1}
.form-label{
  font-size:.7rem;font-weight:600;
  color:var(--secondary);letter-spacing:.04em;text-transform:uppercase;
}
.form-input,.form-select,.form-textarea{
  padding:12px 16px;
  border:1.5px solid var(--gray-light);
  border-radius:var(--r);
  font-family:var(--ff-body);font-size:.88rem;
  color:var(--secondary);background:var(--gray-bg);
  transition:border-color .2s,background .2s,box-shadow .2s;
  outline:none;
}

.contact-form{
  padding:20px;
}

.form-input::placeholder,.form-select::placeholder,.form-textarea::placeholder{color:var(--gray)}
.form-input:focus,.form-select:focus,.form-textarea:focus{
  border-color:var(--primary);
  background:var(--white);
  box-shadow:0 0 0 4px rgba(118,180,242,.12);
}
.form-textarea{resize:vertical;min-height:100px}
.btn-submit{
  width:100%;padding:14px;
  background:var(--accent);color:var(--white);
  border:none;border-radius:100px;
  font-family:var(--ff-display);
  font-size:.72rem;font-weight:700;letter-spacing:.06em;
  text-transform:uppercase;
  transition:transform .3s var(--ease-out),box-shadow .3s;
  box-shadow:0 8px 24px rgba(208,79,109,.3);
}
.btn-submit:hover{transform:translateY(-2px);box-shadow:0 14px 36px rgba(208,79,109,.45)}

.contact-map{
  margin-top:24px;
  border-radius:var(--r-lg);
  overflow:hidden;
  border:1px solid var(--gray-light);
  background:var(--gray-bg);
}
.contact-map iframe{
  display:block;
  width:100%;
  height:180px;
  filter: grayscale(20%) contrast(105%);
}

</style>
