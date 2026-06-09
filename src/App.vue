<template>
  <header>
    <nav
      class="nav"
      :class="{
        scrolled: isNavScrolled || isMobileMenuOpen,
        'nav-solid': isSolidNavRoute,
      }"
      id="nav"
    >
      <RouterLink to="/" class="nav-logo" @click="closeMobileMenu">
        <img src="/images/logo.png" alt="ODA Gelato" class="nav-logo-img">
      </RouterLink>

      <div class="nav-links desktop-only">
        <a
          v-for="item in navItems"
          :key="item.id"
          :href="`#${item.target}`"
          @click.prevent="navigateTo(item)"
        >{{ item.label }}</a>
      </div>

      <div class="nav-right desktop-only">
        <div class="nav-lang" aria-label="Idioma">
          <button
            type="button"
            class="lang-btn"
            :class="{ active: locale === 'es' }"
            :aria-pressed="locale === 'es'"
            @click="setLanguage('es')"
          >ES</button>
          <button
            type="button"
            class="lang-btn"
            :class="{ active: locale === 'en' }"
            :aria-pressed="locale === 'en'"
            @click="setLanguage('en')"
          >EN</button>
        </div>

        <template v-if="isLoggedIn">
          <span class="nav-user">{{ username }}</span>
          <RouterLink
            v-if="isAdmin"
            to="/admin/products"
            class="nav-admin-btn"
          >Admin</RouterLink>
          <button type="button" class="nav-logout" @click="logout">Salir</button>
        </template>
        <RouterLink v-else-if="currentRoute.path !== '/login'" to="/login" class="nav-login">
          Ingresar
        </RouterLink>
      </div>

      <button
        type="button"
        class="nav-hamburger"
        :class="{ active: isMobileMenuOpen }"
        :aria-expanded="isMobileMenuOpen"
        aria-label="Menú"
        @click="toggleMobileMenu"
      >
        <span></span>
        <span></span>
        <span></span>
      </button>

      <div class="nav-mobile-panel" :class="{ active: isMobileMenuOpen }">
        <div class="nav-mobile-links">
          <a
            v-for="item in navItems"
            :key="item.id"
            :href="`#${item.target}`"
            @click.prevent="navigateTo(item)"
          >{{ item.label }}</a>
        </div>
        <div class="nav-mobile-footer">
          <div class="nav-lang" aria-label="Idioma">
            <button
              type="button"
              class="lang-btn"
              :class="{ active: locale === 'es' }"
              @click="setLanguage('es')"
            >ES</button>
            <button
              type="button"
              class="lang-btn"
              :class="{ active: locale === 'en' }"
              @click="setLanguage('en')"
            >EN</button>
          </div>

          <template v-if="isLoggedIn">
            <span class="nav-user nav-user--mobile">{{ username }}</span>
            <RouterLink
              v-if="isAdmin"
              class="nav-admin-link nav-admin-link--mobile"
              to="/admin/products"
              @click="closeMobileMenu"
            >Panel Admin</RouterLink>
            <button type="button" class="nav-cta nav-cta--block nav-cta--logout" @click="logout">
              Cerrar Sesión
            </button>
          </template>

          <template v-else>
            <RouterLink
              to="/login"
              class="nav-login nav-login--mobile"
              @click="closeMobileMenu"
            >Ingresar</RouterLink>
            <button type="button" class="nav-cta nav-cta--block" @click="scrollToContact">
              Solicitar Cotización
            </button>
          </template>
        </div>
      </div>
    </nav>
  </header>

  <RouterView />

  <ProductQuickViewModal
    :open="quickViewOpen"
    :product="quickViewProduct"
    @close="closeQuickView"
  />

  <!-- BotÃ³n flotante WhatsApp (solo) -->
  <SocialFloating />
</template>

<script setup lang="ts">
import { RouterLink, RouterView, useRoute } from 'vue-router';
import { authService } from '@/services/api';
import { onMounted, onUnmounted, ref, watch, computed } from 'vue';
import router from './router';
import SocialFloating from '@/components/SocialFloating.vue';
import ProductQuickViewModal from '@/components/ProductQuickViewModal.vue'
import { useProductQuickView } from '@/composables/useProductQuickView'
import { useI18n } from 'vue-i18n';
import type { SupportedLocale } from './i18n';
import { persistLocale } from './i18n';

const isLoggedIn = ref(false);
const username = ref('');
const isMobileMenuOpen = ref(false);
const isNavScrolled = ref(false);

const { t, locale } = useI18n();

type NavItem = {
  id: string;
  label: string;
  target: string;
};

const navItems = computed<NavItem[]>(() => [
  { id: 'nosotros', label: t('nav.about'), target: 'about' },
  { id: 'portafolio', label: t('nav.portfolio'), target: 'portafolio' },
  { id: 'propuestas', label: t('nav.valueProposition'), target: 'propuestas' },
  { id: 'contacto', label: t('nav.contact'), target: 'contact' },
  { id: 'tienda', label: t('nav.store'), target: 'portafolio' },
]);

const { isOpen: quickViewOpen, product: quickViewProduct, close: closeQuickView } = useProductQuickView()

const setLanguage = (next: SupportedLocale) => {
  locale.value = next;
  persistLocale(next);
};

// Router hooks
const currentRoute = useRoute();

// Verificar si el usuario es administrador
const isAdmin = computed(() => authService.isAdmin());

const isSolidNavRoute = computed(() => {
  const path = currentRoute.path;
  return path === '/login' || path.startsWith('/admin');
});

const navigateTo = async (item: NavItem) => {
  closeMobileMenu();

  if (currentRoute.path !== '/') {
    await router.push({ path: '/', hash: `#${item.target}` });
    return;
  }

  const el = document.getElementById(item.target);
  if (el) {
    el.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
};

const scrollToContact = () => {
  const contact = navItems.value.find((item) => item.id === 'contacto');
  if (contact) navigateTo(contact);
};

// Funciones para el menÃº hamburguesa
const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false;
};

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
  // Usar replace para no dejar historial que permita volver a la pÃ¡gina autenticada
  router.replace({ name: 'home' });
};

const onWindowScroll = () => {
  isNavScrolled.value = window.scrollY > 0;
};

onMounted(() => {
  checkAuthStatus();
  onWindowScroll();
  window.addEventListener('scroll', onWindowScroll, { passive: true });
});

onUnmounted(() => {
  window.removeEventListener('scroll', onWindowScroll);
});

const route = useRoute();
watch(route, () => {
  checkAuthStatus();
  closeMobileMenu();
});

watch(isMobileMenuOpen, (open) => {
  document.body.style.overflow = open ? 'hidden' : '';
});
</script>

<style scoped>

/* â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
   NAVIGATION â€” ODA GELATO
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â• */
header {
  position: relative;
  z-index: 500;
}

.nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 500;
  height: 72px;
  padding: 0 clamp(20px, 4vw, 48px);
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto minmax(0, 1fr);
  align-items: center;
  gap: 24px;
  background: transparent;
  box-shadow: none;
  transition:
    background-color 0.4s var(--ease-out),
    box-shadow 0.35s var(--ease-out),
    backdrop-filter 0.4s var(--ease-out);
}

.nav.scrolled {
  background: rgba(49, 46, 87, 0.97);
  backdrop-filter: blur(20px);
  box-shadow: 0 6px 32px rgba(26, 24, 48, 0.35);
}

.nav.nav-solid {
  background: rgba(49, 46, 87, 0.97);
  backdrop-filter: blur(20px);
  box-shadow: 0 6px 32px rgba(26, 24, 48, 0.35);
}

.nav-logo {
  justify-self: start;
  text-decoration: none;
}

.nav-logo-img {
  height: 40px;
  width: auto;
  display: block;
  background: var(--white);
  border-radius: var(--r-sm);
  padding: 4px 8px;
  object-fit: contain;
}

.nav-links {
  justify-self: center;
  display: flex;
  align-items: center;
  gap: clamp(20px, 2.5vw, 36px);
  padding-right: 150px;
}

.nav-links a {
  font-family: var(--ff-body);
  font-size: 0.72rem;
  font-weight: 500;
  color: var(--white);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  text-decoration: none;
  white-space: nowrap;
  transition: color 0.2s, opacity 0.2s;
}

.nav-links a:hover {
  color: var(--primary-pale);
}

.nav-right {
  justify-self: end;
  display: flex;
  align-items: center;
  gap: 14px;
  padding-left: 24px;
}

.nav-lang {
  display: flex;
  align-items: center;
  gap: 2px;
  background: rgba(0, 0, 0, 0.22);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 100px;
  padding: 4px;
}

.lang-btn {
  padding: 5px 12px;
  border-radius: 100px;
  font-family: var(--ff-body);
  font-size: 0.68rem;
  font-weight: 600;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.55);
  border: none;
  background: transparent;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}

.lang-btn.active {
  background: var(--primary);
  color: var(--secondary);
}

.nav-cta {
  padding: 10px 22px;
  border-radius: 100px;
  background: var(--accent);
  color: var(--white);
  font-family: var(--ff-display);
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.03em;
  border: none;
  cursor: pointer;
  white-space: nowrap;
  transition: transform 0.25s var(--ease-out), box-shadow 0.25s;
}
.nav-cta:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 28px rgba(208, 79, 109, 0.45);
}

.nav-login {
  padding: 8px 20px;
  border-radius: 100px;
  background: transparent;
  color: var(--white);
  font-family: var(--ff-display);
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  text-decoration: none;
  text-transform: uppercase;
  white-space: nowrap;
  border: 1.5px solid rgba(255, 255, 255, 0.25);
  transition: background 0.25s, border-color 0.25s, transform 0.25s var(--ease-out);
}
.nav-login:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.5);
  transform: translateY(-2px);
}

.nav-user {
  font-family: var(--ff-body);
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--primary);
  white-space: nowrap;
  max-width: 120px;
  overflow: hidden;
  text-overflow: ellipsis;
}

.nav-user--mobile {
  display: block;
  text-align: center;
  color: var(--white);
  margin-bottom: 4px;
}

.nav-admin-btn {
  padding: 8px 20px;
  border-radius: 100px;
  background: var(--primary);
  color: var(--white);
  font-family: var(--ff-display);
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  white-space: nowrap;
  border: 1.5px solid var(--primary);
  text-decoration: none;
  cursor: pointer;
  transition: background 0.25s, border-color 0.25s, transform 0.25s var(--ease-out);
}

.nav-admin-btn:hover {
  background: var(--primary-dark);
  border-color: var(--primary-dark);
  transform: translateY(-2px);
}

.nav-logout {
  padding: 8px 20px;
  border-radius: 100px;
  background: transparent;
  color: var(--white);
  font-family: var(--ff-display);
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  white-space: nowrap;
  border: 1.5px solid rgba(255, 255, 255, 0.25);
  cursor: pointer;
  transition: background 0.25s, border-color 0.25s, transform 0.25s var(--ease-out);
}

.nav-logout:hover {
  background: rgba(208, 79, 109, 0.2);
  border-color: var(--accent);
  transform: translateY(-2px);
}

.nav-cta--logout {
  background: transparent !important;
  border: 1.5px solid rgba(255, 255, 255, 0.25) !important;
  box-shadow: none !important;
}

.nav-cta--logout:hover {
  background: rgba(208, 79, 109, 0.2) !important;
  border-color: var(--accent) !important;
  box-shadow: none !important;
}

.nav-cta:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 28px rgba(208, 79, 109, 0.45);
}

.nav-cta--block {
  width: 100%;
}

.nav-hamburger {
  display: none;
  justify-self: end;
  flex-direction: column;
  justify-content: center;
  gap: 5px;
  width: 40px;
  height: 40px;
  padding: 8px;
  border: none;
  background: transparent;
  cursor: pointer;
}

.nav-hamburger span {
  display: block;
  width: 100%;
  height: 2px;
  background: var(--white);
  border-radius: 2px;
  transition: transform 0.3s, opacity 0.3s;
}

.nav-hamburger.active span:nth-child(1) {
  transform: translateY(7px) rotate(45deg);
}

.nav-hamburger.active span:nth-child(2) {
  opacity: 0;
}

.nav-hamburger.active span:nth-child(3) {
  transform: translateY(-7px) rotate(-45deg);
}

.nav-mobile-panel {
  display: none;
}

.nav-admin-link {
  display: block;
  text-align: center;
  margin-top: 8px;
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.65);
  text-decoration: none;
}

.nav-admin-link:hover {
  color: var(--primary);
}

.nav-login--mobile,
.nav-admin-link--mobile {
  display: block;
  text-align: center;
  padding: 12px;
  border-radius: 100px;
  font-family: var(--ff-display);
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  text-decoration: none;
  transition: background 0.25s, transform 0.25s var(--ease-out);
}

.nav-login--mobile {
  background: transparent;
  color: var(--white);
  border: 1.5px solid rgba(255, 255, 255, 0.25);
}

.nav-login--mobile:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.5);
}

.nav-admin-link--mobile {
  background: var(--primary);
  color: var(--white);
  border: 1.5px solid var(--primary);
}

.nav-admin-link--mobile:hover {
  background: var(--primary-dark);
  border-color: var(--primary-dark);
}

@media (max-width: 1024px) {
  .nav {
    grid-template-columns: 1fr auto;
    gap: 12px;
  }

  .desktop-only {
    display: none !important;
  }

  .nav-hamburger {
    display: flex;
    grid-column: 2;
    grid-row: 1;
  }

  .nav-logo {
    grid-column: 1;
    grid-row: 1;
  }

  .nav-mobile-panel {
    display: block;
    position: fixed;
    top: 72px;
    left: 0;
    right: 0;
    bottom: 0;
    padding: 28px clamp(20px, 5vw, 32px) 500px;
    background: var(--secondary);
    border-top: 1px solid rgba(255, 255, 255, 0.08);
    opacity: 0;
    visibility: hidden;
    transform: translateY(-8px);
    transition: opacity 0.3s, visibility 0.3s, transform 0.3s var(--ease-out);
    overflow-y: auto;
  }

  .nav-mobile-panel.active {
    opacity: 1;
    visibility: visible;
    transform: translateY(0);
  }

  .nav-mobile-links {
    display: flex;
    flex-direction: column;
    gap: 4px;
    margin-bottom: 32px;
  }

  .nav-mobile-links a {
    font-family: var(--ff-display);
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--white);
    text-transform: uppercase;
    letter-spacing: 0.04em;
    text-decoration: none;
    padding: 14px 0;
    border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  }

  .nav-mobile-footer {
    display: flex;
    flex-direction: column;
    gap: 16px;
    align-items: stretch;
  }

  .nav-mobile-footer .nav-lang {
    align-self: flex-start;
  }
}
</style>
