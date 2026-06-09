<template>
  <section class="hero-banner" :style="{ backgroundImage: 'url(/images/banner1.jpg)' }">
    <!-- Fondo con esmeraldas animadas -->
    <div class="emerald-background">
      <div class="emerald emerald-1"></div>
      <div class="emerald emerald-2"></div>
      <div class="emerald emerald-3"></div>
      <div class="emerald emerald-4"></div>
      <div class="emerald emerald-5"></div>
      <div class="emerald emerald-6"></div>
      <div class="emerald emerald-7"></div>
      <div class="emerald emerald-8"></div>
    </div>

    <!-- Banner center logo -->
    <div class="hero-logo-container animate-fade-in">
      <div class="logo-glow"></div>
      <img src="/images/logo.jpeg" :alt="$t('banner.logoAlt')" class="hero-logo">
      <div class="logo-shine"></div>
    </div>

    <!-- Contenedor principal -->
    <div class="banner-content">
      <!-- Textos principales -->
      <div class="text-section">
        <h1 class="brand-name">
          <span class="brand-joyeria">JOYERÍA</span>
          <span class="brand-angelie">ANGELIE</span>
        </h1>
        <div class="hero-divider animate-fade-in-delay-2">
          <span class="divider-line"></span>
          <i class="fas fa-gem"></i>
          <span class="divider-line"></span>
        </div>
        <p class="brand-tagline">{{ $t('banner.tagline') }}</p>
      </div>

      <!-- Botones de acción -->
      <div class="action-buttons">
        <button class="btn btn-primary" @click="scrollToProducts">
          <span>{{ $t('banner.explore') }}</span>
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="m9 18 6-6-6-6"/>
          </svg>
        </button>
        <button class="btn btn-secondary" @click="toggleInfo">
          <span>{{ $t('banner.learnMore') }}</span>
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="6 9 12 15 18 9"/>
          </svg>
        </button>
      </div>

      <!-- Línea decorativa -->

    </div>

    <!-- Indicador de scroll -->
    <div
      class="scroll-indicator"
      role="button"
      tabindex="0"
      @click="scrollDownIndicator"
      @keydown.enter.prevent="scrollDownIndicator"
      @keydown.space.prevent="scrollDownIndicator"
    >
      <p>{{ $t('banner.scrollDown') }}</p>
      <div class="scroll-arrow" aria-hidden="true"></div>
    </div>
  </section>
</template>

<script setup lang="ts">
// Botones del banner: navegación por scroll dentro del Home.

// Funciones
const scrollToProducts = () => {
  const productsSection = document.querySelector('#products') || document.querySelector('.product-store')
  if (productsSection) {
    productsSection.scrollIntoView({ behavior: 'smooth' })
  }
}

const toggleInfo = () => {
  const categoriesSection = document.querySelector('#categories') || document.querySelector('.categories-section')
  if (categoriesSection) {
    categoriesSection.scrollIntoView({ behavior: 'smooth' })
  }
}

const scrollDownIndicator = () => {
  const target =
    document.querySelector('#categories') ||
    document.querySelector('.categories-section') ||
    document.querySelector('#products') ||
    document.querySelector('.product-store')

  if (target) {
    target.scrollIntoView({ behavior: 'smooth' })
    return
  }

  window.scrollBy({ top: window.innerHeight * 0.9, behavior: 'smooth' })
}

defineOptions({
  name: 'MainBanner'
})
</script>

<style scoped>
/* === BANNER ELEGANTE JOYERÍA ANGELIE === */

/* Animaciones */
@keyframes float {
  0%, 100% {
    transform: translateY(0px) rotate(0deg);
  }
  50% {
    transform: translateY(-20px) rotate(5deg);
  }
}

@keyframes rotate-slow {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes pulse-glow {
  0%, 100% {
    box-shadow: 0 0 20px rgba(201, 168, 89, 0.4), inset 0 0 20px rgba(201, 168, 89, 0.15);
  }
  50% {
    box-shadow: 0 0 40px rgba(201, 168, 89, 0.7), inset 0 0 30px rgba(201, 168, 89, 0.25);
  }
}

@keyframes fade-in-down {
  from {
    opacity: 0;
    transform: translateY(-30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slide-up {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes bounce-down {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(10px);
  }
}

@keyframes float-logo {
  0%, 100% {
    transform: translateY(0) scale(1);
  }
  50% {
    transform: translateY(-15px) scale(1.05);
  }
}

@keyframes glow-logo {
  0%, 100% {
    filter: drop-shadow(0 0 15px rgba(201, 168, 89, 0.5));
  }
  50% {
    filter: drop-shadow(0 0 25px rgba(201, 168, 89, 0.9));
  }
}

@keyframes logoFloat {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-15px) scale(1.05); }
}

@keyframes rotate-logo {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes gemRotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes pulse {
  0%, 100% { opacity: 0.5; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.1); }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.animate-fade-in-delay-2 {
  animation: fade-in-down 1s ease-out 0.5s both;
}

.animate-fade-in {
  animation: fadeIn 1.2s ease-out;
}

/* Banner principal */
.hero-banner {
  position: relative;
  width: 100%;
  height: 90vh;
  min-height: 700px;
  overflow: hidden;
  background: linear-gradient(135deg, #0a3a2e 0%, #0d5546 50%, #1a4d47 100%);
  background-size: cover;
  background-position: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.hero-banner::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(rgba(10, 58, 46, 0.15), rgba(10, 58, 46, 0.15)),
              radial-gradient(ellipse at 50% 30%, rgba(212, 175, 55, 0.05), transparent 80%);
  z-index: 2;
  pointer-events: none;
}

/* Fondo con esmeraldas */
.emerald-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: 1;
  opacity: 0.4;
}

.emerald {
  position: absolute;
  background: linear-gradient(135deg, #10b981 0%, #059669 50%, #047857 100%);
  border-radius: 50%;
  filter: blur(40px);
  animation: float 6s ease-in-out infinite;
}

.emerald-1 {
  width: 300px;
  height: 300px;
  top: -10%;
  left: -5%;
  animation-delay: 0s;
}

.emerald-2 {
  width: 250px;
  height: 250px;
  top: 20%;
  right: -10%;
  animation-delay: 1s;
}

.emerald-3 {
  width: 280px;
  height: 280px;
  bottom: -15%;
  left: 15%;
  animation-delay: 2s;
}

.emerald-4 {
  width: 200px;
  height: 200px;
  bottom: 10%;
  right: 5%;
  animation-delay: 1.5s;
}

.emerald-5 {
  width: 220px;
  height: 220px;
  top: 40%;
  left: 10%;
  animation-delay: 2.5s;
}

.emerald-6 {
  width: 260px;
  height: 260px;
  top: 60%;
  right: 20%;
  animation-delay: 0.5s;
}

.emerald-7 {
  width: 180px;
  height: 180px;
  bottom: 30%;
  left: 50%;
  animation-delay: 3s;
}

.emerald-8 {
  width: 240px;
  height: 240px;
  top: 10%;
  right: 40%;
  animation-delay: 1.8s;
}

/* Contenedor principal */
.banner-content {
  position: relative;
  z-index: 10;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1.3rem;
  animation: fade-in-down 1s ease-out;
}

/* Asegura centrado del bloque de texto (incluye tagline) */
.text-section {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Hero logo (center) */
.hero-logo-container {
  position: relative;
  margin: 0 auto 1.5rem;
  z-index: 20;
  display: block;
  text-align: center;
}

.hero-logo {
  height: 180px;
  width: 180px;
  border-radius: 50%;
  box-shadow: 0 0 40px rgba(212, 175, 55, 0.6), 0 0 80px rgba(212, 175, 55, 0.3), 0 20px 60px rgba(0, 0, 0, 0.5);
  border: 4px solid rgba(212, 175, 55, 0.5);
  animation: logoFloat 3s ease-in-out infinite;
  display: block;
  object-fit: contain;
}

.logo-glow {
  position: absolute;
  inset: -20px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(212, 175, 55, 0.4), transparent 70%);
  animation: pulse 2s ease-in-out infinite;
  z-index: -1;
}

@keyframes shine {
  0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
  100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
}

.logo-shine {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(45deg, transparent 30%, rgba(255,255,255,0.3) 50%, transparent 70%);
  animation: shine 3s infinite;
  pointer-events: none;
  z-index: 5;
}

/* (Logo styles consolidated above for hero-logo-container) */

/* Textos */
.text-section {
  animation: slide-up 1s ease-out 0.4s both;
}

.brand-name {
  font-size: clamp(3rem, 12vw, 5.5rem);
  font-weight: 700;
  letter-spacing: 4px;
  margin: 0;
  line-height: 1;
  text-transform: uppercase;
  font-family: 'Playfair Display', 'Georgia', 'Garamond', serif;
}

.brand-joyeria {
  display: block;
  font-size: 48px;
  color: rgba(255, 255, 255, 0.95);
  font-weight: 300;
  letter-spacing: 8px;
  text-transform: uppercase;
  margin-bottom: 10px;
  text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.7);
  font-family: 'Playfair Display', 'Georgia', 'Garamond', serif;
}

.brand-angelie {
  display: block;
  font-size: 96px;
  color: #c9a859;
  letter-spacing: 12px;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.8), 0 0 10px rgba(201, 168, 89, 0.3);
  font-family: 'Playfair Display', 'Georgia', 'Garamond', serif;
  line-height: 1;
}

.hero-divider {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;
  margin-top: 1.5rem;
  margin-bottom: 1rem;
}

.divider-line {
  width: 60px;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgb(201, 168, 89), transparent);
  display: block;
}

.hero-divider i {
  color: rgb(201, 168, 89);
  font-size: 1.2rem;
  animation: gemRotate 4s linear infinite;
}

.brand-tagline {
  font-size: 24px;
  color: rgba(255, 255, 255, 0.95);
  margin-top: 1.5rem;
  margin-bottom: 0;
  margin-left: auto;
  margin-right: auto;
  letter-spacing: 0.5px;
  font-weight: 300;
  max-width: 700px;
  line-height: 1.8;
  font-family: 'Segoe UI', 'Trebuchet MS', sans-serif;
  text-align: center;
}

/* Botones de acción */
.action-buttons {
  display: flex;
  gap: 1.5rem;
  margin-top: 2rem;
  flex-wrap: wrap;
  justify-content: center;
  animation: slide-up 1s ease-out 0.6s both;
}

.btn {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem 2rem;
  font-size: 1rem;
  font-weight: 600;
  border: 2px solid;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 1px;
  position: relative;
  overflow: hidden;
}

.btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.1);
  transition: left 0.3s ease;
  z-index: -1;
}

.btn:hover::before {
  left: 100%;
}

.btn-primary {
  background: linear-gradient(135deg, rgb(201, 168, 89) 0%, rgb(220, 190, 120) 100%);
  color: #0a3a2e;
  border-color: rgb(201, 168, 89);
  box-shadow: 0 8px 25px rgba(201, 168, 89, 0.4);
}

.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 35px rgba(201, 168, 89, 0.6);
}

.btn-secondary {
  background: linear-gradient(135deg, rgba(15, 36, 55, 0.8) 0%, rgba(10, 58, 46, 0.9) 100%);
  color: #ffffff;
  border-color: rgb(201, 168, 89);
  box-shadow: 0 8px 25px rgba(201, 168, 89, 0.2);
}

.btn-secondary:hover {
  background: linear-gradient(135deg, rgba(15, 36, 55, 0.95) 0%, rgba(10, 58, 46, 1) 100%);
  border-color: rgb(201, 168, 89);
  box-shadow: 0 8px 25px rgba(201, 168, 89, 0.4);
}

/*
  Override global styles (e.g. `button.btn-secondary:hover:not(:disabled)` in `src/assets/main.css`)
  that can win on hover due to higher specificity and turn the button red.
*/
.action-buttons button.btn.btn-secondary {
  background: linear-gradient(
    135deg,
    rgba(15, 36, 55, 0.85) 0%,
    rgba(10, 58, 46, 0.95) 55%,
    rgba(4, 120, 87, 0.85) 100%
  );
  box-shadow: 0 8px 25px rgba(201, 168, 89, 0.2);
}

.action-buttons button.btn.btn-secondary:hover:not(:disabled) {
  /* Keep the same look (no red), just a subtle lift */
  background: linear-gradient(
    135deg,
    rgba(15, 36, 55, 0.95) 0%,
    rgba(10, 58, 46, 1) 100%
  );
  border-color: rgb(201, 168, 89);
  box-shadow: 0 12px 35px rgba(201, 168, 89, 0.35);
  transform: translateY(-3px);
}

.action-buttons button.btn.btn-secondary:focus-visible:not(:disabled) {
  /* No color change on focus, only emphasis */
  background: linear-gradient(
    135deg,
    rgba(15, 36, 55, 0.85) 0%,
    rgba(10, 58, 46, 0.95) 55%,
    rgba(4, 120, 87, 0.85) 100%
  );
  box-shadow: 0 0 0 3px rgba(201, 168, 89, 0.35), 0 8px 25px rgba(201, 168, 89, 0.2);
}

.action-buttons button.btn.btn-secondary:active:not(:disabled) {
  /* Press effect without changing color */
  background: linear-gradient(
    135deg,
    rgba(15, 36, 55, 0.85) 0%,
    rgba(10, 58, 46, 0.95) 55%,
    rgba(4, 120, 87, 0.85) 100%
  );
  transform: translateY(1px) scale(0.98);
  box-shadow: 0 6px 18px rgba(201, 168, 89, 0.22);
}

.action-buttons button.btn.btn-primary {
  /* Prevent global `button.btn-primary` from changing the banner look */
  background: linear-gradient(135deg, rgb(201, 168, 89) 0%, rgb(220, 190, 120) 100%);
  color: #0a3a2e;
  border-color: rgb(201, 168, 89);
  box-shadow: 0 8px 25px rgba(201, 168, 89, 0.4);
}

.action-buttons button.btn.btn-primary:hover:not(:disabled) {
  background: linear-gradient(135deg, rgb(201, 168, 89) 0%, rgb(220, 190, 120) 100%);
  transform: translateY(-3px);
  box-shadow: 0 12px 35px rgba(201, 168, 89, 0.6);
}

.action-buttons button.btn.btn-primary:focus-visible:not(:disabled) {
  background: linear-gradient(135deg, rgb(201, 168, 89) 0%, rgb(220, 190, 120) 100%);
  box-shadow: 0 0 0 3px rgba(201, 168, 89, 0.35), 0 8px 25px rgba(201, 168, 89, 0.4);
}

.action-buttons button.btn.btn-primary:active:not(:disabled) {
  /* Press effect without changing color (primary button) */
  background: linear-gradient(135deg, rgb(201, 168, 89) 0%, rgb(220, 190, 120) 100%);
  transform: translateY(1px) scale(0.98);
  box-shadow: 0 6px 18px rgba(201, 168, 89, 0.35);
}

.btn svg {
  width: 20px;
  height: 20px;
  transition: transform 0.3s ease;
}

.btn-primary:hover svg,
.btn-secondary:hover svg {
  transform: translateX(3px);
}

/* Línea decorativa */
.decorative-line {
  width: 80px;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgb(201, 168, 89), transparent);
  margin-top: 1rem;
  animation: slide-up 1s ease-out 0.8s both;
}

/* Indicador de scroll */
.scroll-indicator {
  position: absolute;
  bottom: 18px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  animation: slide-up 1s ease-out 1s both;
  z-index: 10;
  cursor: pointer;
  user-select: none;
}

.scroll-indicator:focus-visible {
  outline: 2px solid rgba(201, 168, 89, 0.5);
  outline-offset: 6px;
  border-radius: 10px;
}

.scroll-indicator p {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.75);
  letter-spacing: 2px;
  text-transform: uppercase;
  text-align: center;
  margin: 0;
}

.scroll-arrow {
  width: 26px;
  height: 14px;
  position: relative;
  animation: bounce-down 2s ease-in-out infinite;
}

.scroll-arrow::before,
.scroll-arrow::after {
  content: '';
  position: absolute;
  top: 6px;
  width: 14px;
  height: 2px;
  background: rgb(201, 168, 89);
  opacity: 0.95;
}

.scroll-arrow::before {
  left: 0;
  transform: rotate(45deg);
  transform-origin: left center;
}

.scroll-arrow::after {
  right: 0;
  transform: rotate(-45deg);
  transform-origin: right center;
}

/* Responsive */
@media (max-width: 768px) {
  .hero-banner {
    height: 80vh;
    min-height: 600px;
  }

  .banner-content {
    gap: 1.5rem;
  }

  .brand-name {
    font-size: 2rem;
  }

  .brand-joyeria {
    font-size: 1.6rem;
    letter-spacing: 5px;
    margin-bottom: 8px;
  }

  .brand-angelie {
    font-size: 4.2rem;
    letter-spacing: 8px;
  }

  .brand-tagline {
    font-size: 0.95rem;
    max-width: 90%;
    margin-top: 0.75rem;
    line-height: 1.8;
  }

  .action-buttons {
    gap: 1rem;
  }

  .btn {
    padding: 0.8rem 1.5rem;
    font-size: 0.9rem;
  }

  .logo-circle-banner {
    width: 100px;
    height: 100px;
  }

  .hero-logo {
    height: 140px;
    width: 140px;
  }

  .logo-inner {
    font-size: 3rem;
  }

  .emerald {
    filter: blur(30px);
  }
}

@media (max-width: 480px) {
  .hero-banner {
    height: 95vh;
    min-height: 500px;
  }

  .hero-logo-container {
    margin: 0 auto 1rem;
  }

  .brand-name {
    font-size: 1.5rem;
  }

  .brand-joyeria {
    font-size: 1.35rem;
    letter-spacing: 4px;
    margin-bottom: 6px;
  }

  .brand-angelie {
    font-size: 3.15rem;
    letter-spacing: 6px;
  }

  .brand-tagline {
    font-size: 0.9rem;
    line-height: 1.65;
    margin-top: 0.85rem;
  }

  .hero-divider {
    gap: 1rem;
    margin-top: 1rem;
    margin-bottom: 0.75rem;
  }

  .divider-line {
    width: 44px;
  }

  .action-buttons {
    flex-direction: column;
    width: 100%;
    padding: 0 1rem;
    gap: 0.9rem;
    margin-top: 1.25rem;
  }

  .btn {
    width: 100%;
    justify-content: center;
    padding: 0.9rem 1.5rem;
  }

  /* En móvil, el secundario se ve como "outline" limpio (como la captura) */
  .action-buttons button.btn.btn-secondary,
  .action-buttons button.btn.btn-secondary:hover:not(:disabled),
  .action-buttons button.btn.btn-secondary:focus-visible:not(:disabled) {
    background: rgba(255, 255, 255, 0.08);
    border-color: rgba(255, 255, 255, 0.45);
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.22);
  }

  .hero-logo {
    height: 100px;
    width: 100px;
  }

  /* En celular: indicador debajo de los botones (en flujo normal) */
  .scroll-indicator {
    position: relative;
    left: auto;
    right: auto;
    bottom: auto;
    transform: none;
    width: 100%;
    margin-top: 0.85rem;
    padding-bottom: env(safe-area-inset-bottom);
    gap: 0.35rem;
  }

  .scroll-arrow {
    width: 30px;
    height: 16px;
  }
}
</style>
