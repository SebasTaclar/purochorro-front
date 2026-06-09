<template>
  <main class="home-main">
    <!-- Efecto de brillo de diamantes flotantes -->
    <div class="sparkles" aria-hidden="true">
      <div class="sparkle" v-for="n in 30" :key="n">✨</div>
    </div>

    <!-- Partículas doradas flotantes -->
    <div class="gold-particles" aria-hidden="true">
      <div class="particle" v-for="n in 20" :key="n"></div>
    </div>

    <section class="section">
      <MainBanner @showRules="openRulesModal" />
    </section>

    <section id="categories" class="section">
      <CategoriesSection />
    </section>

    <section id="products" class="section">
      <ProductStore />
    </section>

    <section id="videos" class="section">
      <VideosSection />
    </section>

    <section id="contact" class="section section--visit-footer-divider">
      <ContactSection />
    </section>

    <section class="section offset">
      <Footer_ />
    </section>

    <!-- Enlace flotante para prueba de sincronización -->
    <!-- <router-link to="/test-sync" class="test-sync-link">
      🧪 Probar Sincronización
    </router-link> -->

    <!-- Modal de reglas -->

  </main>
</template>

<script setup lang="ts">
import { ref, defineAsyncComponent } from 'vue'
import MainBanner from '@/components/MainBanner.vue';

// Carga diferida (code splitting) de secciones pesadas
const ProductStore = defineAsyncComponent(() => import('@/components/ProductStore.vue'))
const VideosSection = defineAsyncComponent(() => import('@/components/VideosSection.vue'))
const ContactSection = defineAsyncComponent(() => import('@/components/ContactSection.vue'))
const Footer_ = defineAsyncComponent(() => import('@/components/Footer_.vue'))
const CategoriesSection = defineAsyncComponent(() => import('@/components/CategoriesSection.vue'))

// Estado del modal de reglas
const showRulesModal = ref(false)

const openRulesModal = () => {
  showRulesModal.value = true
}

const closeRulesModal = () => {
  showRulesModal.value = false
}

defineOptions({
  name: 'HomeView',
});

// Expose variables to template
defineExpose({
  showRulesModal,
  openRulesModal,
  closeRulesModal
});

</script>

<style scoped>
/* === EFECTOS VISUALES === */

/* Animación de brillo de diamantes */
@keyframes sparkle {
  0% {
    transform: scale(0) rotate(0deg);
    opacity: 0;
  }
  50% {
    opacity: 1;
    transform: scale(1) rotate(180deg);
  }
  100% {
    transform: scale(0) rotate(360deg);
    opacity: 0;
  }
}

/* Movimiento ondulante dorado */
@keyframes float-gold {
  0%, 100% {
    transform: translateY(0) translateX(0);
  }
  50% {
    transform: translateY(-30px) translateX(20px);
  }
}

/* Partículas doradas flotantes */
@keyframes float-particle {
  0% {
    transform: translateY(0) translateX(0) rotate(0deg);
    opacity: 0.2;
  }
  25% {
    opacity: 0.5;
  }
  50% {
    transform: translateY(-200px) translateX(50px) rotate(180deg);
    opacity: 0.8;
  }
  75% {
    opacity: 0.5;
  }
  100% {
    transform: translateY(-400px) translateX(-50px) rotate(360deg);
    opacity: 0;
  }
}

/* Brillo dorado pulsante */
@keyframes glow {
  0%, 100% {
    box-shadow: 0 0 20px rgba(215, 172, 67, 0.3);
  }
  50% {
    box-shadow: 0 0 40px rgba(215, 172, 67, 0.6);
  }
}

.home-main {
  padding-top: 60px;
  background: linear-gradient(180deg, #0a0a0a 0%, #1a1a1a 50%, #0a0a0a 100%);
  color: var(--white);
  width: 100%;
  max-width: 100vw;
  overflow-x: hidden;
  overflow-y: auto;
  box-sizing: border-box;
  position: relative;
  min-height: 100vh;
}

@media (max-width: 480px) {
  .sparkles,
  .gold-particles {
    display: none;
  }
}

/* === EFECTO DE DIAMANTES BRILLANTES === */
.sparkles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 100;
  overflow: hidden;
}

.sparkle {
  position: absolute;
  top: -10%;
  color: #d7ac43;
  font-size: 1.5em;
  user-select: none;
  animation: sparkle ease-in-out infinite;
  text-shadow: 0 0 10px rgba(215, 172, 67, 0.9), 0 0 20px rgba(215, 172, 67, 0.5);
  filter: brightness(1.2);
}

.sparkle:nth-child(1n) { left: 1%; animation-duration: 4s; animation-delay: 0s; font-size: 1.2em; }
.sparkle:nth-child(2n) { left: 10%; animation-duration: 5s; animation-delay: 0.5s; font-size: 1.5em; }
.sparkle:nth-child(3n) { left: 20%; animation-duration: 4.5s; animation-delay: 1s; font-size: 1.3em; }
.sparkle:nth-child(4n) { left: 30%; animation-duration: 5.5s; animation-delay: 1.5s; font-size: 1.4em; }
.sparkle:nth-child(5n) { left: 40%; animation-duration: 4.2s; animation-delay: 0s; font-size: 1.2em; }
.sparkle:nth-child(6n) { left: 50%; animation-duration: 5.2s; animation-delay: 0.5s; font-size: 1.5em; }
.sparkle:nth-child(7n) { left: 60%; animation-duration: 5.5s; animation-delay: 0.5s; font-size: 1.1em; }
.sparkle:nth-child(8n) { left: 70%; animation-duration: 6s; animation-delay: 1s; font-size: 1.3em; }
.sparkle:nth-child(9n) { left: 80%; animation-duration: 4.8s; animation-delay: 1.5s; font-size: 1.2em; }
.sparkle:nth-child(10n) { left: 90%; animation-duration: 5.5s; animation-delay: 0.3s; font-size: 1.4em; }
.sparkle:nth-child(11n) { left: 5%; animation-duration: 5.2s; animation-delay: 0.8s; font-size: 1.2em; }
.sparkle:nth-child(12n) { left: 15%; animation-duration: 4.5s; animation-delay: 1.2s; font-size: 1.5em; }
.sparkle:nth-child(13n) { left: 25%; animation-duration: 5s; animation-delay: 0.4s; font-size: 1.1em; }
.sparkle:nth-child(14n) { left: 35%; animation-duration: 5.3s; animation-delay: 1.1s; font-size: 1.3em; }
.sparkle:nth-child(15n) { left: 45%; animation-duration: 4.7s; animation-delay: 0.7s; font-size: 1.2em; }
.sparkle:nth-child(16n) { left: 55%; animation-duration: 5.5s; animation-delay: 1.3s; font-size: 1.4em; }
.sparkle:nth-child(17n) { left: 65%; animation-duration: 4.9s; animation-delay: 0.6s; font-size: 1.2em; }
.sparkle:nth-child(18n) { left: 75%; animation-duration: 5.2s; animation-delay: 1.4s; font-size: 1.3em; }
.sparkle:nth-child(19n) { left: 85%; animation-duration: 5.6s; animation-delay: 0.9s; font-size: 1.5em; }
.sparkle:nth-child(20n) { left: 95%; animation-duration: 4.8s; animation-delay: 1.6s; font-size: 1.2em; }
.sparkle:nth-child(21n) { left: 12%; animation-duration: 5.3s; animation-delay: 0.5s; font-size: 1.4em; }
.sparkle:nth-child(22n) { left: 22%; animation-duration: 5.1s; animation-delay: 1s; font-size: 1.2em; }
.sparkle:nth-child(23n) { left: 32%; animation-duration: 4.9s; animation-delay: 1.3s; font-size: 1.3em; }
.sparkle:nth-child(24n) { left: 42%; animation-duration: 5.4s; animation-delay: 0.7s; font-size: 1.5em; }
.sparkle:nth-child(25n) { left: 52%; animation-duration: 5s; animation-delay: 1.2s; font-size: 1.1em; }
.sparkle:nth-child(26n) { left: 62%; animation-duration: 5.5s; animation-delay: 0.4s; font-size: 1.4em; }
.sparkle:nth-child(27n) { left: 72%; animation-duration: 4.7s; animation-delay: 1.5s; font-size: 1.2em; }
.sparkle:nth-child(28n) { left: 82%; animation-duration: 5.2s; animation-delay: 0.8s; font-size: 1.3em; }
.sparkle:nth-child(29n) { left: 92%; animation-duration: 5.1s; animation-delay: 1.1s; font-size: 1.5em; }
.sparkle:nth-child(30n) { left: 7%; animation-duration: 5.3s; animation-delay: 0.6s; font-size: 1.2em; }

/* === PARTÍCULAS DORADAS === */
.gold-particles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 50;
  overflow: hidden;
}

.particle {
  position: absolute;
  bottom: -20px;
  width: 4px;
  height: 4px;
  background: linear-gradient(135deg, #d7ac43, #f4c430);
  border-radius: 50%;
  animation: float-particle linear infinite;
  box-shadow:
    0 0 10px rgba(215, 172, 67, 0.5),
    0 0 20px rgba(215, 172, 67, 0.3),
    0 0 30px rgba(215, 172, 67, 0.2);
}

.particle:nth-child(1) { left: 5%; animation-duration: 8s; animation-delay: 0s; width: 6px; height: 6px; }
.particle:nth-child(2) { left: 15%; animation-duration: 10s; animation-delay: 1s; width: 4px; height: 4px; }
.particle:nth-child(3) { left: 25%; animation-duration: 12s; animation-delay: 2s; width: 5px; height: 5px; }
.particle:nth-child(4) { left: 35%; animation-duration: 9s; animation-delay: 0.5s; width: 7px; height: 7px; }
.particle:nth-child(5) { left: 45%; animation-duration: 11s; animation-delay: 1.5s; width: 4px; height: 4px; }
.particle:nth-child(6) { left: 55%; animation-duration: 13s; animation-delay: 2.5s; width: 6px; height: 6px; }
.particle:nth-child(7) { left: 65%; animation-duration: 10s; animation-delay: 0.8s; width: 5px; height: 5px; }
.particle:nth-child(8) { left: 75%; animation-duration: 12s; animation-delay: 1.8s; width: 4px; height: 4px; }
.particle:nth-child(9) { left: 85%; animation-duration: 9s; animation-delay: 2.8s; width: 6px; height: 6px; }
.particle:nth-child(10) { left: 95%; animation-duration: 11s; animation-delay: 0.3s; width: 5px; height: 5px; }
.particle:nth-child(11) { left: 10%; animation-duration: 14s; animation-delay: 1.2s; width: 4px; height: 4px; }
.particle:nth-child(12) { left: 20%; animation-duration: 8s; animation-delay: 2.2s; width: 7px; height: 7px; }
.particle:nth-child(13) { left: 30%; animation-duration: 10s; animation-delay: 0.7s; width: 5px; height: 5px; }
.particle:nth-child(14) { left: 40%; animation-duration: 13s; animation-delay: 1.7s; width: 6px; height: 6px; }
.particle:nth-child(15) { left: 50%; animation-duration: 9s; animation-delay: 2.7s; width: 4px; height: 4px; }
.particle:nth-child(16) { left: 60%; animation-duration: 11s; animation-delay: 0.4s; width: 5px; height: 5px; }
.particle:nth-child(17) { left: 70%; animation-duration: 12s; animation-delay: 1.4s; width: 6px; height: 6px; }
.particle:nth-child(18) { left: 80%; animation-duration: 10s; animation-delay: 2.4s; width: 4px; height: 4px; }
.particle:nth-child(19) { left: 90%; animation-duration: 14s; animation-delay: 0.6s; width: 7px; height: 7px; }
.particle:nth-child(20) { left: 12%; animation-duration: 9s; animation-delay: 1.6s; width: 5px; height: 5px; }

.section {
  padding: 0.1px 0 0 0;
  border-bottom: 1px solid rgba(215, 172, 67, 0.1);
  width: 100%;
  max-width: 100%;
  box-sizing: border-box;
  z-index: 10;
}

/* Separador fuerte solo entre "Visítanos" y el Footer */
.section--visit-footer-divider {
  border-bottom: 2px solid rgba(201, 168, 89, 0.28);
  position: relative;
}

.section--visit-footer-divider::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  bottom: -1px;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgba(201, 168, 89, 0.55), transparent);
  box-shadow: 0 0 18px rgba(201, 168, 89, 0.25);
  pointer-events: none;
}

.section:last-child {
  border-bottom: none;
}

/* Resplandor en hover */
.section:hover {
  animation: glow 2s ease-in-out infinite;
}

/* Responsive: reducir efectos en móviles */
@media (max-width: 768px) {
  .sparkle:nth-child(n+15) {
    display: none;
  }

  .particle:nth-child(n+10) {
    display: none;
  }
}

/* Enlace flotante para prueba de sincronización */
.test-sync-link {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: linear-gradient(45deg, #667eea, #764ba2);
  color: white;
  padding: 12px 20px;
  border-radius: 25px;
  text-decoration: none;
  font-weight: 600;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
  z-index: 1000;
  transition: all 0.3s ease;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.test-sync-link:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.3);
  background: linear-gradient(45deg, #764ba2, #667eea);
}
</style>
