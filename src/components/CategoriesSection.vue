<template>
  <!-- ══ MARQUEE ══ -->
<div class="marquee-band">
  <div class="marquee-track">
    <div class="mq-item">Private Label <span class="mq-sep">✦</span></div>
    <div class="mq-item">Maquila Industrial <span class="mq-sep">✦</span></div>
    <div class="mq-item">19 Años de Experiencia <span class="mq-sep">✦</span></div>
    <div class="mq-item">Tecnología Italiana <span class="mq-sep">✦</span></div>
    <div class="mq-item">Exportación USA <span class="mq-sep">✦</span></div>
    <div class="mq-item">Certificación FDA <span class="mq-sep">✦</span></div>
    <div class="mq-item">30M Litros al Año <span class="mq-sep">✦</span></div>
    <div class="mq-item">Laboratorio Propio <span class="mq-sep">✦</span></div>
    <!-- repeat for seamless loop -->
    <div class="mq-item">Private Label <span class="mq-sep">✦</span></div>
    <div class="mq-item">Maquila Industrial <span class="mq-sep">✦</span></div>
    <div class="mq-item">19 Años de Experiencia <span class="mq-sep">✦</span></div>
    <div class="mq-item">Tecnología Italiana <span class="mq-sep">✦</span></div>
    <div class="mq-item">Exportación USA <span class="mq-sep">✦</span></div>
    <div class="mq-item">Certificación FDA <span class="mq-sep">✦</span></div>
    <div class="mq-item">30M Litros al Año <span class="mq-sep">✦</span></div>
    <div class="mq-item">Laboratorio Propio <span class="mq-sep">✦</span></div>
  </div>
</div>
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
/* ═══════════════════════════════════════════════
   MARQUEE BAND
═══════════════════════════════════════════════ */
.marquee-band{
  background:var(--primary);
  padding:23px 0;overflow:hidden;
  overflow:hidden;
  border:none;
  box-shadow:none;
}
.marquee-track{
  display:flex;width:max-content;
  animation:marquee 25s linear infinite;
}
.mq-item{
  display:flex;align-items:center;gap:20px;
  padding:0 28px;
  font-family:var(--ff-display);
  font-size:.65rem;font-weight:600;
  letter-spacing:.14em;text-transform:uppercase;
  color:var(--secondary);white-space:nowrap;
}
.mq-sep{color:rgba(49,46,87,.4);font-size:.45rem}
@keyframes marquee{from{transform:translateX(0)}to{transform:translateX(-50%)}}
</style>
