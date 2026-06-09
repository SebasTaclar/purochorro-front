<template>
  <section class="visit-section contact-section">
    <div class="container">
      <!-- Encabezado -->
      <div class="section-header">
        <h2 class="section-title">✦ {{ $t('visit.title') }} ✦</h2>
        <p class="section-description">{{ $t('visit.subtitle') }}</p>
      </div>

      <!-- Contenido: Mapa + Info -->
      <div class="content-grid">
        <!-- Mapa + Dirección -->
        <div class="left-column">
          <div ref="mapHostEl" class="map-wrapper">
            <iframe
              v-if="shouldLoadMap"
              title="Mapa: Joyería Angelie"
              src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3976.9729106924015!2d-74.07661092622172!3d4.598875342534367!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x8e3f9957a2e86765%3A0x92bd4b62b7506c09!2sJOYERIA%20ANGELIE!5e0!3m2!1ses!2sus!4v1765479420647!5m2!1ses!2sus"
              allowfullscreen
              loading="lazy"
              referrerpolicy="no-referrer-when-downgrade"
              class="map-iframe"
            ></iframe>
            <div v-else class="map-placeholder" aria-hidden="true"></div>
          </div>

          <div class="info-item">
            <div class="info-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                <circle cx="12" cy="10" r="3"/>
              </svg>
            </div>
            <div class="info-content">
              <h3 class="info-label">{{ $t('visit.addressLabel') }}</h3>
              <p class="info-value">Edificio Víctor Melo · Cl. 12 #6-10 Local 07 · La Candelaria, Bogotá</p>
            </div>
          </div>
        </div>

        <!-- Información -->
        <div class="info-wrapper">
          <div class="info-item">
            <div class="info-icon phone-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
              </svg>
            </div>
            <div class="info-content">
              <h3 class="info-label">{{ $t('visit.phoneLabel') }}</h3>
              <p class="info-value phone-number"><a href="tel:3213727442">321 3727442</a></p>
            </div>
          </div>

          <div class="info-item">
            <div class="info-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                <path d="m22 6-10 7L2 6"/>
              </svg>
            </div>
            <div class="info-content">
              <h3 class="info-label">{{ $t('visit.emailLabel') }}</h3>
              <p class="info-value"><a href="mailto:info@joyeriaange.com">info@joyeriaange.com</a></p>
            </div>
          </div>

          <div class="info-item">
            <div class="info-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <circle cx="12" cy="12" r="10"/>
                <polyline points="12 6 12 12 16 14"/>
              </svg>
            </div>
            <div class="info-content">
              <h3 class="info-label">{{ $t('visit.hoursLabel') }}</h3>
              <p class="info-value">Lunes a Viernes<br>9:00 AM - 6:00 PM<br>Sábados: 9:00 AM - 2:00 PM</p>
            </div>
          </div>
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
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;800&display=swap');

/* === SECCIÓN COMPACTA === */
.visit-section {
  padding: 1.5rem 0;
  background: linear-gradient(180deg, #071e25 0%, #0a1f26 100%);
  border-top: 1px solid rgba(201, 168, 89, 0.15);
  position: relative;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* === ENCABEZADO === */
.section-header {
  text-align: center;
  margin-bottom: 0.9rem;
}

.section-title {
  font-family: 'Playfair Display', serif;
  font-size: 1.9rem;
  font-weight: 800;
  color: rgb(201, 168, 89);
  margin: 0 0 0.5rem 0;
  letter-spacing: 1.5px;
}

.section-description {
  font-size: 0.9rem;
  color: rgba(246, 245, 241, 0.75);
  margin: 0 auto;
  max-width: 52ch;
  line-height: 1.6;
}

/* === GRID DE CONTENIDO === */
.content-grid {
  display: grid;
  grid-template-columns: 1.3fr 1fr;
  gap: 1rem;
  align-items: start;
}

.left-column {
  display: flex;
  flex-direction: column;
  gap: 0.65rem;
}

/* === MAPA === */
.map-wrapper {
  border-radius: 16px;
  overflow: hidden;
  border: 2px solid rgba(201, 168, 89, 0.3);
  box-shadow: 0 8px 28px rgba(0, 0, 0, 0.28);
}

.map-iframe {
  width: 100%;
  height: 240px;
  border: none;
  display: block;
}

.map-placeholder {
  width: 100%;
  height: 240px;
  background: rgba(255, 255, 255, 0.04);
}

/* === INFORMACIÓN === */
.info-wrapper {
  display: flex;
  flex-direction: column;
  gap: 0.65rem;
}

.info-item {
  display: flex;
  gap: 0.85rem;
  padding: 0.7rem;
  background: linear-gradient(135deg, rgba(201, 168, 89, 0.08) 0%, rgba(201, 168, 89, 0.03) 100%);
  border: 1px solid rgba(201, 168, 89, 0.25);
  border-radius: 12px;
  transition: background 0.2s ease, border-color 0.2s ease;
}

.info-item:hover {
  border-color: rgba(201, 168, 89, 0.5);
  background: linear-gradient(135deg, rgba(201, 168, 89, 0.12) 0%, rgba(201, 168, 89, 0.05) 100%);
}

.info-icon {
  width: 38px;
  height: 38px;
  min-width: 38px;
  border-radius: 12px;
  background: rgba(201, 168, 89, 0.15);
  border: 1px solid rgba(201, 168, 89, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgb(201, 168, 89);
}

.info-icon svg {
  width: 24px;
  height: 24px;
}

.phone-icon {
  background: rgba(37, 211, 102, 0.15);
  border-color: rgba(37, 211, 102, 0.3);
  color: #25D366;
}

.info-content {
  flex: 1;
}

.info-label {
  font-family: 'Playfair Display', serif;
  font-size: 0.95rem;
  font-weight: 800;
  color: rgb(201, 168, 89);
  margin: 0 0 0.35rem 0;
  letter-spacing: 0.5px;
}

.info-value {
  font-size: 0.9rem;
  color: rgba(246, 245, 241, 0.85);
  margin: 0;
  line-height: 1.45;
}

.info-value a {
  color: inherit;
  text-decoration: none;
}

.info-value a:hover {
  text-decoration: underline;
  text-decoration-color: rgba(201, 168, 89, 0.7);
  text-underline-offset: 3px;
}

.phone-number {
  font-size: 1.05rem;
  font-weight: 700;
  color: #25D366;
  letter-spacing: 1px;
}

.phone-number a {
  color: inherit;
}

/* === RESPONSIVE === */
@media (max-width: 1024px) {
  .content-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .map-iframe {
    height: 240px;
  }
}

@media (max-width: 768px) {
  .visit-section {
    padding: 1.25rem 0;
  }

  /* En móvil/tablet: centrar columnas y textos */
  .left-column,
  .info-wrapper {
    align-items: center;
  }

  .info-item {
    justify-content: center;
  }

  .info-content {
    text-align: center;
  }

  .section-title {
    font-size: 1.7rem;
  }

  .section-description {
    font-size: 0.85rem;
  }

  .map-iframe {
    height: 220px;
  }

  .info-item {
    padding: 0.7rem;
  }
}

@media (max-width: 480px) {
  .container {
    padding: 0 1rem;
  }

  .section-header {
    margin-bottom: 1rem;
  }

  .section-title {
    font-size: 1.4rem;
    letter-spacing: 1px;
  }

  .section-description {
    font-size: 0.82rem;
  }

  .map-iframe {
    height: 200px;
  }

  /* Teléfono + correo en la misma línea; horarios debajo a ancho completo */
  .info-wrapper {
    width: 100%;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.65rem;
    align-items: stretch;
  }

  /* El card de horarios (último) ocupa toda la fila */
  .info-wrapper .info-item:last-child {
    grid-column: 1 / -1;
  }

  .info-item {
    flex-direction: column;
    gap: 0.6rem;
    padding: 0.8rem;
    align-items: center;
    text-align: center;
  }

  .info-icon {
    width: 38px;
    height: 38px;
    min-width: 38px;
  }

  .info-label {
    font-size: 0.95rem;
  }

  .info-value {
    font-size: 0.85rem;
  }

  .phone-number {
    font-size: 1rem;
  }

}


</style>
