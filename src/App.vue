<template>
  <header>
    <nav class="navbar">
      <!-- Logo y marca -->
      <RouterLink class="link-navbar home" to="/" @click="closeMobileMenu">
        <div class="brand-container">

          <div class="brand-info">
            <div class="brand-title"><span class="highlight">JOYERÍA ANGELIE</span></div>
            <div class="brand-tagline">{{ t('brand.tagline') }}</div>
          </div>
        </div>
      </RouterLink>

      <!-- Navegación principal -->
      <div class="nav-menu desktop-nav">
        <RouterLink to="/anillos" class="nav-link" :class="{ active: isCurrentRoute('/anillos') }" @click="closeMobileMenu">{{ t('nav.rings') }}</RouterLink>
        <RouterLink to="/collares" class="nav-link" :class="{ active: isCurrentRoute('/collares') }" @click="closeMobileMenu">{{ t('nav.necklaces') }}</RouterLink>
        <RouterLink to="/aretes" class="nav-link" :class="{ active: isCurrentRoute('/aretes') }" @click="closeMobileMenu">{{ t('nav.earings') }}</RouterLink>
        <RouterLink to="/pulseras" class="nav-link" :class="{ active: isCurrentRoute('/pulseras') }" @click="closeMobileMenu">{{ t('nav.bracelets') }}</RouterLink>
        <RouterLink to="/esmeraldas" class="nav-link" :class="{ active: isCurrentRoute('/esmeraldas') }" @click="closeMobileMenu">{{ t('nav.emeralds') }}</RouterLink>
        <RouterLink to="/ofertas" class="nav-link" :class="{ active: isCurrentRoute('/ofertas') }" @click="closeMobileMenu">{{ t('nav.offers') }}</RouterLink>
      </div>

      <!-- Controles de usuario -->
      <div class="nav-controls desktop-nav">
        <button
          v-if="showHeaderSearch"
          type="button"
          class="search-toggle"
          aria-label="Abrir buscador"
          title="Buscar"
          @click="openSearch"
        >
          <svg class="search-toggle-icon" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true">
            <path
              fill="currentColor"
              d="M10 2a8 8 0 1 1 0 16 8 8 0 0 1 0-16zm8.707 17.293-4.387-4.387a9 9 0 1 0-1.414 1.414l4.387 4.387a1 1 0 0 0 1.414-1.414z"
            />
          </svg>
        </button>

        <div class="lang-switch" aria-label="Language switch">
          <button
            class="lang-btn"
            :class="{ active: locale === 'en' }"
            type="button"
            :aria-label="locale === 'en' ? 'Language: English (selected)' : 'Switch to English'"
            title="English"
            @click="setLanguage('en')"
          >
            <img class="flag-icon" src="/icons/flags/us.svg" alt="" aria-hidden="true" />
            <span class="sr-only">English</span>
          </button>
          <button
            class="lang-btn"
            :class="{ active: locale === 'es' }"
            type="button"
            :aria-label="locale === 'es' ? 'Idioma: Español (seleccionado)' : 'Cambiar a Español'"
            title="Español"
            @click="setLanguage('es')"
          >
            <img class="flag-icon" src="/icons/flags/co.svg" alt="" aria-hidden="true" />
            <span class="sr-only">Español</span>
          </button>
        </div>

        <RouterLink v-if="!isLoggedIn" class="btn access-btn" to="/login">{{ t('auth.access') }}</RouterLink>
        <RouterLink v-if="isLoggedIn && isAdmin" class="btn admin-btn" to="/admin/products">{{ t('auth.adminPanel') }}</RouterLink>
        <RouterLink v-if="isLoggedIn" @click="logout" class="btn logout-btn" to="/">{{ t('auth.logout') }}</RouterLink>
        <div v-if="isLoggedIn" class="user-greeting">
          <span>{{ username }}</span>
        </div>
      </div>

      <!-- Menu hamburguesa para mobile -->
      <div class="mobile-header-controls">
        <button
          v-if="showHeaderSearch"
          type="button"
          class="search-toggle mobile-only"
          aria-label="Abrir buscador"
          title="Buscar"
          @click="openSearch"
        >
          <svg class="search-toggle-icon" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true">
            <path
              fill="currentColor"
              d="M10 2a8 8 0 1 1 0 16 8 8 0 0 1 0-16zm8.707 17.293-4.387-4.387a9 9 0 1 0-1.414 1.414l4.387 4.387a1 1 0 0 0 1.414-1.414z"
            />
          </svg>
        </button>

        <div class="lang-switch mobile-header" aria-label="Language switch">
          <button
            class="lang-btn"
            :class="{ active: locale === 'en' }"
            type="button"
            :aria-label="locale === 'en' ? 'Language: English (selected)' : 'Switch to English'"
            title="English"
            @click="setLanguage('en')"
          >
            <img class="flag-icon" src="/icons/flags/us.svg" alt="" aria-hidden="true" />
            <span class="sr-only">English</span>
          </button>
          <button
            class="lang-btn"
            :class="{ active: locale === 'es' }"
            type="button"
            :aria-label="locale === 'es' ? 'Idioma: Español (seleccionado)' : 'Cambiar a Español'"
            title="Español"
            @click="setLanguage('es')"
          >
            <img class="flag-icon" src="/icons/flags/co.svg" alt="" aria-hidden="true" />
            <span class="sr-only">Español</span>
          </button>
        </div>
      </div>

      <button class="hamburger-menu" @click="toggleMobileMenu" :class="{ 'active': isMobileMenuOpen }">
        <span></span>
        <span></span>
        <span></span>
      </button>

      <!-- Menu mobile desplegable -->
      <div class="mobile-menu" :class="{ 'active': isMobileMenuOpen }">
        <div class="mobile-menu-content">
          <div class="mobile-nav-links">
            <RouterLink to="/anillos" class="mobile-link" :class="{ active: isCurrentRoute('/anillos') }" @click="closeMobileMenu">{{ t('nav.rings') }}</RouterLink>
            <RouterLink to="/collares" class="mobile-link" :class="{ active: isCurrentRoute('/collares') }" @click="closeMobileMenu">{{ t('nav.necklaces') }}</RouterLink>
            <RouterLink to="/aretes" class="mobile-link" :class="{ active: isCurrentRoute('/aretes') }" @click="closeMobileMenu">{{ t('nav.earings') }}</RouterLink>
            <RouterLink to="/pulseras" class="mobile-link" :class="{ active: isCurrentRoute('/pulseras') }" @click="closeMobileMenu">{{ t('nav.bracelets') }}</RouterLink>
            <RouterLink to="/esmeraldas" class="mobile-link" :class="{ active: isCurrentRoute('/esmeraldas') }" @click="closeMobileMenu">{{ t('nav.emeralds') }}</RouterLink>
            <RouterLink to="/ofertas" class="mobile-link" :class="{ active: isCurrentRoute('/ofertas') }" @click="closeMobileMenu">{{ t('nav.offers') }}</RouterLink>
          </div>

          <div class="mobile-controls">
            <RouterLink v-if="!isLoggedIn" class="mobile-btn access-btn" to="/login" @click="closeMobileMenu">
              {{ t('auth.access') }}
            </RouterLink>
            <div v-if="isLoggedIn" class="mobile-user-greeting">
              <span>{{ t('auth.hello', { name: username }) }}</span>
            </div>
            <RouterLink v-if="isLoggedIn && isAdmin" class="mobile-btn admin-btn" to="/admin/products" @click="closeMobileMenu">
              {{ t('auth.adminPanel') }}
            </RouterLink>
            <button v-if="isLoggedIn" @click="handleMobileLogout" class="mobile-btn logout-btn">
              {{ t('auth.logout') }}
            </button>
          </div>
        </div>
      </div>
    </nav>

    <GlobalSearchOverlay v-model:open="isSearchOpen" />
  </header>

  <RouterView />

  <ProductQuickViewModal
    :open="quickViewOpen"
    :product="quickViewProduct"
    @close="closeQuickView"
  />

  <GlobalCart />

  <!-- Botones flotantes de redes sociales -->
  <SocialFloating />
</template>

<script setup lang="ts">
import { RouterLink, RouterView, useRoute } from 'vue-router';
import { authService } from '@/services/api';
import { onMounted, ref, watch, computed } from 'vue';
import router from './router';
import SocialFloating from '@/components/SocialFloating.vue';
import GlobalCart from '@/components/GlobalCart.vue';
import GlobalSearchOverlay from '@/components/GlobalSearchOverlay.vue';
import ProductQuickViewModal from '@/components/ProductQuickViewModal.vue'
import { useProductQuickView } from '@/composables/useProductQuickView'
import { useI18n } from 'vue-i18n';
import type { SupportedLocale } from './i18n';
import { persistLocale } from './i18n';

const isLoggedIn = ref(false);
const username = ref('');
const isMobileMenuOpen = ref(false);
const isSearchOpen = ref(false);

const { isOpen: quickViewOpen, product: quickViewProduct, close: closeQuickView } = useProductQuickView()

const { t, locale } = useI18n();

const setLanguage = (next: SupportedLocale) => {
  locale.value = next;
  persistLocale(next);
};

// Router hooks
const currentRoute = useRoute();

// Verificar si el usuario es administrador
const isAdmin = computed(() => authService.isAdmin());

// Función para verificar la ruta actual
const isCurrentRoute = (path: string): boolean => {
  return currentRoute.path === path;
};

const showHeaderSearch = computed(() => !currentRoute.path.startsWith('/admin') && currentRoute.path !== '/login');

// Funciones para el menú hamburguesa
const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false;
};

const openSearch = async () => {
  isSearchOpen.value = true;
};




// Función para hacer scroll a la sección de productos
/* const scrollToProductStore = () => {
  const productStoreSection = document.querySelector('.product-store');
  if (productStoreSection) {
    productStoreSection.scrollIntoView({
      behavior: 'smooth',
      block: 'start'
    });
  }
}; */

// Función para hacer scroll a la sección de contacto
/* const scrollToContact = () => {
  const contactSection = document.querySelector('.contact-section');
  if (contactSection) {
    contactSection.scrollIntoView({
      behavior: 'smooth',
      block: 'start'
    });
  }
}; */

const checkAuthStatus = () => {
  isLoggedIn.value = authService.isAuthenticated();
  if (isLoggedIn.value) {
    const currentUser = authService.getCurrentUser();
    username.value = currentUser?.name || '';
  } else {
    username.value = '';
  }
};

const logout = () => {
  authService.logout();
  isLoggedIn.value = false;
  username.value = '';
  // Usar replace para no dejar historial que permita volver a la página autenticada
  router.replace({ name: 'home' });
};

const handleMobileLogout = () => {
  closeMobileMenu();
  logout();
};

onMounted(() => {
  checkAuthStatus();
});

const route = useRoute();
watch(route, () => {
  checkAuthStatus();
});
</script>

<style scoped>
.navbar {
  background: #ffffff;
  margin: 0;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  height: 75px;
  padding: 0 clamp(20px, 5vw, 60px);
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
}

.search-toggle {
  width: 42px;
  height: 42px;
  border-radius: 999px;
  border: 1px solid rgba(26, 58, 82, 0.2);
  background: rgba(26, 58, 82, 0.05);
  color: #1a3a52;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  backdrop-filter: blur(12px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.search-toggle:hover {
  background: rgba(201, 168, 89, 0.15);
  border-color: rgb(201, 168, 89);
  color: rgb(201, 168, 89);
}

.search-toggle:active {
  transform: scale(0.98);
}

.search-toggle.mobile-only {
  display: none;
}

.mobile-header-controls {
  display: none;
  align-items: center;
  gap: 10px;
}

.lang-switch.mobile-header {
  display: none;
  flex-direction: row;
  gap: 4px;
  padding: 4px;
  border-radius: 10px;
  background: rgba(26, 58, 82, 0.05);
  border: 1px solid rgba(26, 58, 82, 0.15);
}



/* Logo y marca */
.brand-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

/* Logo creativo */
.creative-logo {
  position: relative;
  width: 50px;
  height: 50px;
}

.logo-letter {
  position: relative;
  z-index: 2;
  font-weight: 900;
  font-size: 14px;
  color: var(--white);
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.5),
               0 0 20px rgba(255, 255, 255, 0.3);
  letter-spacing: -1px;
}

.logo-letter:first-child::after {
  content: '•';
  margin: 0 1px;
  font-size: 8px;
  opacity: 0.6;
}

.logo-glow {
  position: absolute;
  inset: -10px;
  background: radial-gradient(circle, rgba(201, 168, 89, 0.4) 0%, transparent 70%);
  border-radius: 50%;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
}

.creative-logo:hover .logo-circle {
  transform: scale(1.08);
  box-shadow: 0 8px 24px rgba(201, 168, 89, 0.6),
              0 0 40px rgba(201, 168, 89, 0.4),
              inset 0 2px 12px rgba(255, 255, 255, 0.3);
}

.creative-logo:hover .logo-glow {
  opacity: 1;
}

@keyframes logoFloat {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-4px) rotate(2deg); }
}



.brand-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 2px;
}

.brand-title {
  font-size: 22px;
  font-weight: 900;
  line-height: 1;
  margin: 0;
  letter-spacing: 1.5px;
  text-transform: uppercase;
}

.brand-title .highlight {
  background: linear-gradient(135deg, #1a3a52 0%, rgb(201, 168, 89) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  filter: drop-shadow(0 1px 2px rgba(201, 168, 89, 0.2));
}

.brand-tagline {
  font-size: 10px;
  color: rgba(26, 58, 82, 0.6);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.brand-subtitle {
  font-size: 12px;
  color: #94a3b8;
  font-weight: 500;
  line-height: 1;
  margin: 0;
}

/* Navegación principal */
.nav-menu {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-left: auto;
  margin-right: 30px;
}

.nav-link {
  color: #1a3a52;
  text-decoration: none;
  font-weight: 600;
  font-size: 15px;
  padding: 10px 18px;
  border-radius: 12px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  letter-spacing: 0.3px;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}

.nav-link:focus {
  outline: none;
}

.nav-link:focus-visible {
  box-shadow: 0 0 0 3px rgba(26, 58, 82, 0.15);
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: 5px;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 2px;
  background: #1a3a52;
  transition: width 0.3s ease;
}

.nav-link:hover {
  color: rgb(201, 168, 89);
  background-color: rgba(201, 168, 89, 0.1);
  transform: translateY(-2px);
}

.nav-link:hover::after {
  width: 70%;
  background: rgb(201, 168, 89);
}

.nav-link.active {
  color: rgb(201, 168, 89);
  background: rgba(201, 168, 89, 0.12);
}

.nav-link.active::after {
  width: 70%;
  background: rgb(201, 168, 89);
}

.share-btn {
  background: linear-gradient(135deg, #22d3ee 0%, #0891b2 100%);
  color: #ffffff !important;
  font-weight: 600;
  box-shadow: 0 2px 10px rgba(34, 211, 238, 0.3);
}

.share-btn:hover {
  background: linear-gradient(135deg, #0891b2 0%, #0e7490 100%);
  box-shadow: 0 4px 15px rgba(34, 211, 238, 0.5);
  transform: translateY(-2px);
}

/* Controles de usuario */
.nav-controls {
  display: flex;
  align-items: center;
  gap: 15px;
}

.lang-switch {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  padding: 6px;
  border-radius: 12p6, 58, 82, 0.05;
  border: 1px solid rgba(26, 58, 82, 0.15);
}

.lang-switch.mobile {
  width: 100%;
  justify-content: center;
}

.lang-btn {
  appearance: none;
  border: 1px solid rgba(26, 58, 82, 0.15);
  background: rgba(26, 58, 82, 0.05);
  color: #1a3a52;
  border-radius: 10px;
  padding: 6px 10px;
  font-weight: 800;
  letter-spacing: 0.6px;
  cursor: pointer;
  transition: background 0.2s ease, border-color 0.2s ease;
  line-height: 1;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.flag-icon {
  width: 18px;
  height: 12px;
  display: block;
  border-radius: 2px;
  box-shadow: 0 0 0 1px rgba(26, 58, 82, 0.1);
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.lang-btn:hover {
  background: rgba(201, 168, 89, 0.15);
  color: rgb(201, 168, 89.3);
  background: rgba(26, 58, 82, 0.1);
}

.lang-btn.active {
  border-color: rgb(201, 168, 89);
  background: rgba(201, 168, 89, 0.15.85);
  background: rgba(201, 168, 89, 0.2);
  color: rgb(201, 168, 89);
}

.btn {
  padding: 10px 20px;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
  font-size: 14px;
  transition: all 0.3s ease;
  border: none;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.access-btn {
  background: linear-gradient(135deg, rgb(201, 168, 89) 0%, rgb(180, 145, 65) 100%);
  color: #0a3a2e;
  box-shadow: 0 4px 16px rgba(201, 168, 89, 0.35);
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.access-btn:hover {
  background: linear-gradient(135deg, rgb(180, 145, 65) 0%, rgb(160, 125, 45) 100%);
  box-shadow: 0 6px 24px rgba(201, 168, 89, 0.5);
  transform: translateY(-3px);
}

.logout-btn {
  background: rgba(201, 168, 89, 0.1);
  color: rgb(201, 168, 89);
  border: 1px solid rgba(201, 168, 89, 0.3);
}

.logout-btn:hover {
  background: rgba(201, 168, 89, 0.2);
  border-color: rgba(201, 168, 89, 0.5);
  transform: translateY(-1px);
}

.admin-btn {
  background: linear-gradient(135deg, var(--black) 0%, #1a1a1a 100%);
  color: rgb(201, 168, 89);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.4);
  border: 1px solid rgba(201, 168, 89, 0.3);
  font-weight: 700;
  padding: 6px 12px;
  font-size: 12px;
}

.admin-btn:hover {
  background: linear-gradient(135deg, #1a1a1a 0%, var(--black) 100%);
  box-shadow: 0 6px 24px rgba(201, 168, 89, 0.4);
  transform: translateY(-3px);
  border-color: rgb(201, 168, 89);
}

.purchases-btn {
  background: var(--brand-accent-gradient);
  color: #ffffff;
  box-shadow: 0 2px 10px var(--brand-accent-glow);
}

.purchases-btn:hover {
  background: var(--brand-accent-gradient);
  filter: brightness(1.1);
  box-shadow: 0 4px 15px var(--brand-accent-glow);
  transform: translateY(-2px);
}

.user-greeting {
  color: #1a3a52;
  font-weight: 600;
  font-size: 14px;
  margin-right: 10px;
}

/* Menu hamburguesa */
.hamburger-menu {
  display: none;
  flex-direction: column;
  width: 30px;
  height: 30px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  justify-content: space-around;
  align-items: center;
  z-index: 1001;
}

.hamburger-menu span {
  display: block;
  height: 3px;
  width: 100%;
  background-color: #1a3a52;
  border-radius: 3px;
  transition: all 0.3s ease;
}

.hamburger-menu.active span:nth-child(1) {
  transform: rotate(45deg) translate(8px, 8px);
}

.hamburger-menu.active span:nth-child(2) {
  opacity: 0;
}

.hamburger-menu.active span:nth-child(3) {
  transform: rotate(-45deg) translate(7px, -6px);
}

/* Menu mobile */
.mobile-menu {
  display: none;
  position: fixed;
  top: 70px;
  left: 0;
  width: 100%;
  height: calc(100vh - 70px);
  background: var(--brand-gradient);
  transform: translateX(-100%);
  transition: transform 0.3s ease;
  z-index: 999;
  overflow-y: auto;
}

.mobile-menu.active {
  transform: translateX(0);
}

.mobile-menu-content {
  padding: 30px 20px;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.mobile-nav-links {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.mobile-link {
  color: #e2e8f0;
  text-decoration: none;
  padding: 15px 20px;
  font-size: 18px;
  font-weight: 500;
  border-radius: 12px;
  transition: all 0.3s ease;
  text-align: center;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}

.mobile-link:focus {
  outline: none;
}

.mobile-link:focus-visible {
  box-shadow: 0 0 0 3px rgba(201, 168, 89, 0.35);
}

.mobile-link:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #ffffff;
  transform: translateY(-2px);
}

.mobile-controls {
  display: flex;
  flex-direction: column;
  gap: 15px;
  padding-top: 20px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.mobile-btn {
  padding: 15px 20px;
  border-radius: 12px;
  text-decoration: none;
  font-weight: 600;
  font-size: 16px;
  text-align: center;
  transition: all 0.3s ease;
  border: none;
  cursor: pointer;
  width: 100%;
  font-family: inherit;
}

.mobile-btn.access-btn {
  background: linear-gradient(135deg, rgb(201, 168, 89) 0%, rgb(180, 145, 65) 100%);
  color: #0a3a2e;
  box-shadow: 0 4px 15px rgba(201, 168, 89, 0.3);
}

.mobile-btn.logout-btn {
  background: rgba(201, 168, 89, 0.1);
  color: rgb(201, 168, 89);
  border: 1px solid rgba(201, 168, 89, 0.3);
}

.mobile-btn.admin-btn {
  background: linear-gradient(135deg, #06b6d4 0%, #0891b2 100%);
  color: #ffffff;
  box-shadow: 0 4px 15px rgba(6, 182, 212, 0.3);
}

.mobile-btn.purchases-btn {
  background: linear-gradient(135deg, #60a5fa 0%, #3b82f6 100%);
  color: #ffffff;
  box-shadow: 0 4px 15px rgba(96, 165, 250, 0.3);
}

.mobile-user-greeting {
  color: #e2e8f0;
  text-align: center;
  padding: 15px 20px;
  font-weight: 600;
  font-size: 16px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  backdrop-filter: blur(10px);
}

/* Responsive */
@media (max-width: 768px) {
  .navbar {
    height: 70px;
    padding: 0 20px;
  }

  .desktop-nav {
    display: none;
  }

  .hamburger-menu {
    display: flex;
  }

  .mobile-header-controls {
    display: flex;
  }

  .search-toggle.mobile-only {
    display: inline-flex;
  }

  .lang-switch.mobile-header {
    display: inline-flex;
  }

  .mobile-menu {
    display: block;
  }

  .brand-title {
    font-size: 18px;
  }

  .brand-logo {
    width: 42px;
    height: 42px;
  }

  .brand-subtitle {
    font-size: 11px;
  }

  .logo-circle {
    width: 45px;
    height: 45px;
    font-size: 20px;
  }
}

@media (max-width: 480px) {
  .navbar {
    padding: 0 15px;
  }

  .brand-container {
    gap: 10px;
  }

  .brand-title {
    font-size: 16px;
  }

  .brand-logo {
    width: 38px;
    height: 38px;
  }

  .logo-circle {
    width: 40px;
    height: 40px;
    font-size: 18px;
  }
}

/* Quitar subrayado del link principal */
.link-navbar {
  text-decoration: none !important;
}

/* Estilos para enlaces activos */
/* Nota: el subrayado del link activo en desktop ya lo maneja
   `.nav-link::after` + `.nav-link.active::after` (centrado).
   Aquí sólo damos estado activo al menú mobile sin subrayado extra. */
.mobile-link.active {
  color: rgb(201, 168, 89);
  border-color: rgba(201, 168, 89, 0.35);
  background: rgba(201, 168, 89, 0.12);
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scaleX(0.5);
  }
  to {
    opacity: 1;
    transform: scaleX(1);
  }
}

.link-navbar:hover {
  text-decoration: none !important;
}
</style>
