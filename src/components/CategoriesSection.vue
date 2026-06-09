<template>
  <section class="categories-section">
    <div class="container">
      <header class="header">
        <h2 class="title">Nuestras Categorías</h2>
        <p class="subtitle">Explora cada pieza de nuestra exclusiva selección diseñada para realzar tu elegancia</p>
      </header>

      <div v-if="loading" class="state">Cargando categorías...</div>
      <div v-else-if="error" class="state error">{{ error }}</div>

      <div v-else class="grid" role="list">
        <button
          v-for="category in categories"
          :key="category.id"
          type="button"
          class="category-card"
          role="listitem"
          @click="goToCategory(category)"
        >
          <img
            class="bg-image"
            :src="getImageSrc(category)"
            alt=""
            aria-hidden="true"
            loading="lazy"
            decoding="async"
            @error="onImageError(category)"
          />

          <span class="icon-badge" aria-hidden="true">
            <svg v-if="iconName(category) === 'ring'" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12 2.5l2.2 3.9 4.3.9-3 3.2.5 4.4L12 12.9 8 15.9l.5-4.4-3-3.2 4.3-.9L12 2.5zm0 9.2l1.2-.9-1.2-.9-1.2.9 1.2.9z"
              />
            </svg>
            <svg v-else-if="iconName(category) === 'necklace'" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12 2a1 1 0 0 1 .9.55l1.9 3.8 4.2.6a1 1 0 0 1 .56 1.69l-3 2.9.7 4.1a1 1 0 0 1-1.45 1.05L12 16.9l-3.8 2a1 1 0 0 1-1.45-1.05l.7-4.1-3-2.9A1 1 0 0 1 5 6.35l4.2-.6 1.9-3.8A1 1 0 0 1 12 2z"
              />
            </svg>
            <svg v-else-if="iconName(category) === 'earrings'" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12 2.5a3.5 3.5 0 0 1 3.5 3.5c0 1.2-.6 2.2-1.5 2.9v2.1c0 1.7-1.3 3-3 3s-3-1.3-3-3V8.9C7.1 8.2 6.5 7.2 6.5 6A3.5 3.5 0 0 1 12 2.5zm0 2a1.5 1.5 0 0 0-1.5 1.5c0 .6.3 1.1.8 1.3l.7.4v4.3a1 1 0 1 0 2 0V7.7l.7-.4c.5-.2.8-.7.8-1.3A1.5 1.5 0 0 0 12 4.5z"
              />
            </svg>
            <svg v-else-if="iconName(category) === 'bracelet'" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12 2a8 8 0 1 1 0 16A8 8 0 0 1 12 2zm0 2a6 6 0 1 0 0 12A6 6 0 0 0 12 4zm7.2 12.3a1 1 0 0 1 1.4.1l1.2 1.4a1 1 0 0 1-1.5 1.3l-1.2-1.4a1 1 0 0 1 .1-1.4z"
              />
            </svg>
            <svg v-else-if="iconName(category) === 'emerald'" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M8 3h8l3 4-7 14H12L5 7l3-4zm2 3-1.4 2.2L12 20l3.4-11.8L14 6h-4z"
              />
            </svg>
            <svg v-else-if="iconName(category) === 'perfume'" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M10 2h4v2h-1v2.2c.9.2 1.6 1 1.6 2v.8H17a2 2 0 0 1 2 2v7a3 3 0 0 1-3 3H8a3 3 0 0 1-3-3v-7a2 2 0 0 1 2-2h2.4v-.8c0-1 .7-1.8 1.6-2V4h-1V2zm-3 11v7a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1v-7H7zm4.4-2h1.2v-.8a.6.6 0 0 0-.6-.6.6.6 0 0 0-.6.6v.8z"
              />
            </svg>
            <svg v-else viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12 2l6 6-6 14L6 8l6-6zm0 4.3L8.8 9 12 19.2 15.2 9 12 6.3z"
              />
            </svg>
          </span>

          <div class="card-gradient" aria-hidden="true"></div>

          <div class="card-body">
            <h3 class="card-title">{{ category.name }}</h3>
            <div class="title-underline" aria-hidden="true"></div>
            <p class="card-description">{{ briefDescription(category.description) }}</p>
            <div class="pill">{{ designsLabel(category) }}</div>
          </div>
        </button>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'
import { useCategories } from '@/composables/useCategories'
import type { Category } from '@/types/CategoryType'

const router = useRouter()
const { categories, loading, error, loadCategories } = useCategories()

const imageIndexByCategoryId = ref<Record<string, number>>({})

onMounted(async () => {
  if (categories.value.length === 0) {
    await loadCategories()
  }
})

function slugify(value: string): string {
  return value
    .trim()
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .replace(/[^a-z0-9\s-]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-')
}

function iconName(category: Category): 'ring' | 'necklace' | 'earrings' | 'bracelet' | 'emerald' | 'perfume' | 'gem' {
  const slug = slugify(category.name)
  if (slug.includes('anillo')) return 'ring'
  if (slug.includes('collar')) return 'necklace'
  if (slug.includes('arete')) return 'earrings'
  if (slug.includes('pulsera')) return 'bracelet'
  if (slug.includes('esmerald')) return 'emerald'
  if (slug.includes('perfume')) return 'perfume'
  return 'gem'
}

function fallbackInternetImageUrl(category: Category): string {
  // Placeholder de internet por categoría (según el nombre). No depende de Unsplash.
  // Formato: /800x600/<bg>/<fg>?text=<Nombre>
  const text = encodeURIComponent(category.name || 'Categoría')
  return `https://placehold.co/800x600/0A3A2E/D7AC43?text=${text}`
}

function imageCandidates(category: Category): string[] {
  // En este proyecto las imágenes públicas viven en `public/images/`.
  const slug = slugify(category.name)
  const base = `/images/${slug}`
  return [
    `${base}.jpg`,
    `${base}.jpeg`,
    `${base}.png`,
    `${base}.webp`,
    fallbackInternetImageUrl(category)
  ]
}

function getImageSrc(category: Category): string {
  const idx = imageIndexByCategoryId.value[category.id] ?? 0
  const list = imageCandidates(category)
  return list[Math.min(idx, list.length - 1)]
}

function onImageError(category: Category) {
  const current = imageIndexByCategoryId.value[category.id] ?? 0
  const next = Math.min(current + 1, imageCandidates(category).length - 1)
  imageIndexByCategoryId.value = { ...imageIndexByCategoryId.value, [category.id]: next }
}

function briefDescription(description?: string): string {
  const base = (description || '').trim()
  return base || 'Explora esta categoría'
}

function goToCategory(category: Category) {
  const slug = slugify(category.name)

  // En este proyecto las vistas están registradas con nombres iguales al path: anillos, collares, etc.
  if (router.hasRoute(slug)) {
    router.push({ name: slug })
    return
  }

  // Fallback seguro si no existe una vista específica
  router.push('/')
}

function designsLabel(category: Category): string {
  // No hay conteo de productos en el tipo del backend.
  // Si la descripción trae un número (ej. "15+"), lo reutilizamos; si no, mostramos un CTA.
  const desc = (category.description || '').trim()
  const match = desc.match(/(\d+\s*\+?)/)
  if (match?.[1]) {
    const n = match[1].replace(/\s+/g, '')
    return `${n.endsWith('+') ? n : `${n}+`} DISEÑOS`
  }
  return 'VER COLECCIÓN'
}
</script>

<style scoped>
.categories-section {
  width: 100%;
  padding: 4rem 0;
  background: linear-gradient(180deg, rgba(244, 233, 214, 0.96) 0%, rgba(236, 223, 200, 0.92) 100%);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.header {
  text-align: center;
  margin-bottom: 2.25rem;
}

.title {
  margin: 0;
  font-size: 2.6rem;
  font-weight: 800;
  color: #0a0a0a;
}

.subtitle {
  margin: 0.75rem auto 0;
  max-width: 720px;
  color: rgba(10, 10, 10, 0.7);
  font-size: 1rem;
  line-height: 1.5;
}

.state {
  text-align: center;
  padding: 2rem 0;
  color: rgba(10, 10, 10, 0.75);
}

.state.error {
  color: rgba(220, 38, 38, 0.9);
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1.25rem;
}

@media (max-width: 1024px) {
  .grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 640px) {
  /* Tipografía más cómoda en celular */
  .header {
    margin-bottom: 1.5rem;
  }

  .title {
    font-size: 1.7rem;
    line-height: 1.15;
  }

  .subtitle {
    font-size: 0.9rem;
    line-height: 1.45;
  }

  .state {
    font-size: 0.95rem;
  }

  .grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 0.9rem;
  }

  .container {
    padding: 0 1rem;
  }

  .category-card {
    min-height: 220px;
    border-radius: 16px;
  }

  .icon-badge {
    top: 12px;
    right: 12px;
    width: 36px;
    height: 36px;
  }

  .icon-badge svg {
    width: 18px;
    height: 18px;
  }

  .card-body {
    padding: 1rem 1rem 0.9rem;
  }

  .card-title {
    font-size: 1.45rem !important;
    line-height: 1.15;
  }

  .title-underline {
    margin-top: 0.6rem;
  }

  .card-description {
    font-size: 0.78rem;
    margin-top: 0.6rem;
    -webkit-line-clamp: 3;
    line-clamp: 3;
  }

  .pill {
    margin-top: 0.75rem;
    height: 24px;
    padding: 0 10px;
    font-size: 0.68rem;
  }
}

@media (max-width: 400px) {
  .title {
    font-size: 1.55rem;
  }

  .subtitle {
    font-size: 0.86rem;
  }

  .card-title {
    font-size: 0.98rem;
  }

  .card-description {
    font-size: 0.74rem;
  }
}

.category-card {
  appearance: none;
  border: 0;
  padding: 0;
  text-align: left;
  border-radius: 18px;
  cursor: pointer;
  color: rgba(255, 255, 255, 0.98);
  min-height: 290px;
  position: relative;
  overflow: hidden;
  box-shadow: 0 18px 40px rgba(0, 0, 0, 0.18);
  transition: transform 180ms ease, box-shadow 180ms ease;
  background-color: rgba(10, 10, 10, 0.88);
}

.bg-image {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 0;
}

.category-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 24px 55px rgba(0, 0, 0, 0.22);
}

.category-card:active {
  transform: translateY(0) scale(0.99);
}

.category-card:focus-visible {
  outline: 3px solid rgba(215, 172, 67, 0.55);
  outline-offset: 4px;
}

.card-gradient {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(circle at 18% 85%, rgba(16, 185, 129, 0.26) 0%, transparent 52%),
    radial-gradient(circle at 85% 30%, rgba(215, 172, 67, 0.28) 0%, transparent 55%),
    linear-gradient(180deg, rgba(0, 0, 0, 0.35) 0%, rgba(0, 0, 0, 0.75) 70%, rgba(0, 0, 0, 0.86) 100%);
  z-index: 1;
}

.icon-badge {
  position: absolute;
  top: 16px;
  right: 16px;
  width: 42px;
  height: 42px;
  border-radius: 999px;
  border: 2px solid rgba(215, 172, 67, 0.9);
  background: rgba(10, 10, 10, 0.35);
  display: grid;
  place-items: center;
  z-index: 3;
}

.icon-badge svg {
  width: 20px;
  height: 20px;
  color: rgba(215, 172, 67, 0.95);
}

.card-body {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 1.45rem 1.45rem 1.25rem;
  z-index: 3;
}

.card-title {
  margin: 0;
  font-size: 2rem;
  font-weight: 800;
  letter-spacing: 0.2px;
}

.title-underline {
  width: 44px;
  height: 2px;
  margin-top: 0.75rem;
  background: rgba(215, 172, 67, 0.95);
}

.card-description {
  margin: 0.75rem 0 0;
  font-size: 0.92rem;
  line-height: 1.35;
  color: rgba(255, 255, 255, 0.85);
  max-width: 18rem;
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 4;
  line-clamp: 4;
}

.pill {
  margin-top: 1rem;
  display: inline-flex;
  align-items: center;
  height: 26px;
  padding: 0 12px;
  border-radius: 999px;
  border: 1px solid rgba(215, 172, 67, 0.7);
  color: rgba(215, 172, 67, 0.95);
  font-size: 0.72rem;
  font-weight: 800;
  letter-spacing: 0.7px;
  text-transform: uppercase;
  background: rgba(10, 10, 10, 0.25);
}
</style>
