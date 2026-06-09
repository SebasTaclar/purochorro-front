<template>
  <div class="admin-dashboard">
    <div class="dashboard-header">
      <h1 class="dashboard-title">
        <span class="icon">⚙️</span>
        Panel de Administración - ODA GELATO
      </h1>
      <p class="dashboard-subtitle">Gestiona productos, categorías y configuraciones</p>
    </div>

    <!-- Estadísticas rápidas -->
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-icon">📦</div>
        <div class="stat-content">
          <div class="stat-number">{{ products.length }}</div>
          <div class="stat-label">Productos</div>
        </div>
      </div>
      <div class="stat-card">
        <div class="stat-icon">🏷️</div>
        <div class="stat-content">
          <div class="stat-number">{{ categories.length }}</div>
          <div class="stat-label">Categorías</div>
        </div>
      </div>
      <div class="stat-card">
        <div class="stat-icon">✅</div>
        <div class="stat-content">
          <div class="stat-number">{{ availableProductsCount }}</div>
          <div class="stat-label">Disponibles</div>
        </div>
      </div>
      <div class="stat-card">
        <div class="stat-icon">💰</div>
        <div class="stat-content">
          <div class="stat-number">${{ totalValue.toLocaleString() }} COP</div>
          <div class="stat-label">Valor Total</div>
        </div>
      </div>
    </div>

    <!-- Navegación por pestañas -->
    <div class="tabs-container">
      <div class="tabs">
        <button
          v-for="tab in tabs"
          :key="tab.id"
          type="button"
          :class="['tab', { active: activeTab === tab.id } ]"
          @click="selectTab(tab.id)"
        >
          <span class="tab-icon">{{ tab.icon }}</span>
          {{ tab.name }}
        </button>
      </div>
    </div>

    <!-- Contenido de las pestañas -->
    <div class="tab-content">
      <!-- Pestaña de Productos -->
      <div v-if="activeTab === 'products'" class="content-section">
        <div class="section-header">
          <h2>Gestión de Productos</h2>
          <button class="btn btn-primary" @click="showProductForm = true">
            <span class="btn-icon">➕</span>
            Nuevo Producto
          </button>
        </div>

        <!-- Barra de búsqueda y filtros en una sola fila -->
        <div class="search-bar">
          <div class="search-input-wrapper">
            <svg class="search-icon" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true">
              <path fill="currentColor" d="M10 2a8 8 0 1 1 0 16 8 8 0 0 1 0-16zm8.707 17.293-4.387-4.387a9 9 0 1 0-1.414 1.414l4.387 4.387a1 1 0 0 0 1.414-1.414z"/>
            </svg>
            <input
              type="search"
              v-model="searchProducts"
              placeholder="Buscar productos por nombre..."
              aria-label="Buscar productos"
              class="search-input"
            />
            <button v-if="searchProducts" class="search-clear" @click.prevent="searchProducts = ''" aria-label="Limpiar búsqueda">X</button>
          </div>

          <!-- Filtro de categoría -->
          <div class="filter-group">
            <!-- Desktop: select nativo -->
            <select v-model="selectedCategoryFilter" class="filter-select native-select">
              <option value="">Todas las categorías</option>
              <option v-for="category in categories" :key="category.id" :value="category.id">
                {{ category.name }}
              </option>
            </select>

            <!-- Mobile: dropdown controlado (evita overflow del desplegable nativo) -->
            <div ref="categoryDropdownRef" class="custom-select">
              <button
                type="button"
                class="filter-select filter-select-btn"
                @click.stop="toggleCategoryDropdown"
                :aria-expanded="categoryDropdownOpen ? 'true' : 'false'"
              >
                <span class="filter-select-text">{{ selectedCategoryLabel }}</span>
                <span class="filter-select-caret">▾</span>
              </button>

              <div v-if="categoryDropdownOpen" class="filter-dropdown" role="listbox">
                <button
                  type="button"
                  class="filter-dropdown-item"
                  :class="{ active: selectedCategoryFilter === '' }"
                  @click="selectCategoryFilter('')"
                >
                  Todas
                </button>
                <button
                  v-for="category in categories"
                  :key="category.id"
                  type="button"
                  class="filter-dropdown-item"
                  :class="{ active: selectedCategoryFilter === category.id }"
                  @click="selectCategoryFilter(category.id)"
                >
                  {{ category.name }}
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Lista de productos -->
        <div class="products-grid">
          <div v-for="product in filteredProducts" :key="product.id" class="product-card">
            <span
              v-if="product.colors && product.colors.length > 0 && product.colors[0]"
              class="classification-badge"
              :class="[
                product.colors[0].toLowerCase() === 'nuevo' ? 'classification-badge--nuevo' : 'classification-badge--novedad'
              ]"
            >{{ product.colors[0].toUpperCase() }}</span>
            <div class="product-image">
              <img v-if="product.images && product.images.length > 0" :src="product.images[0]" :alt="product.name" />
              <div v-else class="no-image">📷</div>
            </div>
            <div class="product-info">
              <h3>{{ product.name }}</h3>
              <p class="product-description">{{ product.description }}</p>
              <div class="product-meta">
                <span class="price">${{ product.price.toLocaleString() }} COP</span>
                <span :class="['status', product.status]">{{ getStatusText(product.status) }}</span>
              </div>
              <div class="product-actions">
                <button class="btn btn-sm btn-secondary btn-edit" @click="editProduct(product)">✏️ Editar</button>
                <button class="btn btn-sm btn-danger btn-delete" @click="deleteProductConfirm(product.id)">🗑️ Eliminar</button>
              </div>
            </div>
          </div>
        </div>

        <!-- Estado vacío o sin resultados -->
        <div v-if="filteredProducts.length === 0 && !searchProducts" class="empty-state">
          <div class="empty-icon">📦</div>
          <h3>No hay productos</h3>
          <p>Comienza agregando tu primer producto</p>
          <button class="btn btn-primary" @click="showProductForm = true">
            Crear Primer Producto
          </button>
        </div>
        <div v-else-if="filteredProducts.length === 0 && searchProducts" class="empty-state">
          <div class="empty-icon">🔍</div>
          <h3>No se encontraron resultados</h3>
          <p>No hay productos que coincidan con "{{ searchProducts }}"</p>
          <button class="btn btn-secondary" @click="searchProducts = ''">
            Limpiar búsqueda
          </button>
        </div>
      </div>

      <!-- Pestaña de Categorías -->
      <div v-if="activeTab === 'categories'" class="content-section">
        <div class="section-header">
          <h2>Gestión de Categorías</h2>
          <button class="btn btn-primary" @click="showCategoryForm = true">
            <span class="btn-icon">➕</span>
            Nueva Categoría
          </button>
        </div>

        <!-- Barra de búsqueda para categorías -->
        <div class="search-bar">
          <div class="search-input-wrapper">
            <svg class="search-icon" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true">
              <path fill="currentColor" d="M10 2a8 8 0 1 1 0 16 8 8 0 0 1 0-16zm8.707 17.293-4.387-4.387a9 9 0 1 0-1.414 1.414l4.387 4.387a1 1 0 0 0 1.414-1.414z"/>
            </svg>
            <input
              type="search"
              v-model="searchCategories"
              placeholder="Buscar categorías por nombre..."
              aria-label="Buscar categorías"
              class="search-input"
            />
            <button v-if="searchCategories" class="search-clear" @click.prevent="searchCategories = ''" aria-label="Limpiar búsqueda">X</button>
          </div>
        </div>

        <!-- Lista de categorías -->
        <div class="categories-list">
          <div v-for="category in filteredCategories" :key="category.id" class="category-item">
            <div class="category-info">
              <h3>{{ category.name }}</h3>
              <p>{{ category.description }}</p>
              <span class="category-count">{{ getProductsInCategory(category.id) }} productos</span>
            </div>
            <div class="category-actions">
              <button class="btn btn-sm btn-secondary" @click="editCategory(category)">✏️</button>
              <button class="btn btn-sm btn-danger" @click="handleDeleteCategory(category.id)">🗑️</button>
            </div>
          </div>
        </div>

        <!-- Estado vacío o sin resultados -->
        <div v-if="filteredCategories.length === 0 && !searchCategories" class="empty-state">
          <div class="empty-icon">🏷️</div>
          <h3>No hay categorías</h3>
          <p>Crea categorías para organizar tus productos</p>
          <button class="btn btn-primary" @click="showCategoryForm = true">
            Crear Primera Categoría
          </button>
        </div>
        <div v-else-if="filteredCategories.length === 0 && searchCategories" class="empty-state">
          <div class="empty-icon">🔍</div>
          <h3>No se encontraron resultados</h3>
          <p>No hay categorías que coincidan con "{{ searchCategories }}"</p>
          <button class="btn btn-secondary" @click="searchCategories = ''">
            Limpiar búsqueda
          </button>
        </div>
      </div>

      <!-- Pestaña de Resumen de Compras -->
      <div v-if="activeTab === 'sales'" class="content-section">
        <div class="section-header">
          <h2>Resumen de Compras</h2>
          <button @click="loadPurchases" class="btn-secondary" :disabled="isLoadingSales">
            {{ isLoadingSales ? 'Cargando...' : 'Actualizar' }}
          </button>
        </div>

        <!-- Barra de búsqueda para compras -->
        <div v-if="!isLoadingSales && !salesError && sales.length > 0" class="search-bar">
          <div class="search-input-wrapper">
            <svg class="search-icon" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true">
              <path fill="currentColor" d="M10 2a8 8 0 1 1 0 16 8 8 0 0 1 0-16zm8.707 17.293-4.387-4.387a9 9 0 1 0-1.414 1.414l4.387 4.387a1 1 0 0 0 1.414-1.414z"/>
            </svg>
            <input
              type="search"
              v-model="searchSales"
              placeholder="Buscar por cliente o producto..."
              aria-label="Buscar compras"
              class="search-input"
            />
            <button v-if="searchSales" class="search-clear" @click.prevent="searchSales = ''" aria-label="Limpiar búsqueda">X</button>
          </div>
        </div>

        <!-- Estado de carga -->
        <div v-if="isLoadingSales" class="loading-state">
          <div class="spinner"></div>
          <p>Cargando compras...</p>
        </div>

        <!-- Error -->
        <div v-else-if="salesError" class="error-state">
          <div class="error-icon">⚠️</div>
          <p>{{ salesError }}</p>
          <button @click="loadPurchases" class="btn-primary">Reintentar</button>
        </div>

        <!-- Contenido -->
        <div v-else>
          <!-- Estadísticas de ventas -->
          <div class="sales-stats">
            <div class="stat-card">
              <div class="stat-icon">💰</div>
              <div class="stat-content">
                <div class="stat-number">${{ totalRevenue.toLocaleString() }}</div>
                <div class="stat-label">Ingresos Totales</div>
              </div>
            </div>
            <div class="stat-card">
              <div class="stat-icon">⏳</div>
              <div class="stat-content">
                <div class="stat-number">{{ pendingSales }}</div>
                <div class="stat-label">Ventas Pendientes</div>
              </div>
            </div>
            <div class="stat-card">
              <div class="stat-icon">📈</div>
              <div class="stat-content">
                <div class="stat-number">{{ totalSalesCount }}</div>
                <div class="stat-label">Total Ventas</div>
              </div>
            </div>
          </div>

          <!-- Tabla de ventas -->
          <div class="sales-table-container" v-if="sales.length > 0">
            <table class="sales-table">
              <thead>
                <tr>
                  <th>Cliente</th>
                  <th>Productos</th>
                  <th>Cantidad</th>
                  <th>Total</th>
                  <th>Estado</th>
                  <th>Fecha</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="sale in filteredSales" :key="sale.id" class="sale-row">
                  <td>
                    <div class="customer-info">
                      <div class="customer-name">{{ sale.customerName }}</div>
                      <div class="customer-email">{{ sale.customerEmail }}</div>
                    </div>
                  </td>
                  <td>
                    <div class="product-info">
                      <!-- Un solo producto -->
                      <div v-if="sale.items && sale.items.length === 1" class="single-product">
                        <div class="product-name">{{ sale.items[0].productName }}</div>
                        <div v-if="sale.items[0].selectedColor" class="product-color">
                          <span class="color-dot" :style="{ backgroundColor: getColorHex(sale.items[0].selectedColor) }"></span>
                          {{ sale.items[0].selectedColor }}
                        </div>
                      </div>

                      <!-- Múltiples productos -->
                      <div v-else-if="sale.items && sale.items.length > 1" class="multiple-products">
                        <div class="products-summary">
                          <span class="products-badge">{{ sale.items.length }} productos</span>
                        </div>
                        <details class="products-details">
                          <summary class="products-toggle">Ver detalles</summary>
                          <ul class="products-list">
                            <li v-for="(item, idx) in sale.items" :key="idx" class="product-item">
                              <span class="item-name">{{ item.productName }}</span>
                              <span class="item-quantity">x{{ item.quantity }}</span>
                              <span v-if="item.selectedColor" class="item-color">
                                <span class="color-dot-small" :style="{ backgroundColor: getColorHex(item.selectedColor) }"></span>
                                {{ item.selectedColor }}
                              </span>
                            </li>
                          </ul>
                        </details>
                      </div>

                      <!-- Fallback -->
                      <div v-else class="product-name">{{ sale.productName }}</div>
                    </div>
                  </td>
                  <td>
                    <div class="quantity-info">
                      <span class="quantity-badge">{{ sale.quantity }}</span>
                      <span v-if="sale.items && sale.items.length > 1" class="quantity-label">unidades totales</span>
                    </div>
                  </td>
                  <td>
                    <span class="amount">${{ sale.totalAmount.toLocaleString() }}</span>
                  </td>
                  <td>
                    <span :class="['status-badge', sale.status]">
                      {{ getSaleStatusText(sale.status) }}
                    </span>
                  </td>
                  <td>
                    <span class="date">{{ formatDate(sale.date) }}</span>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- Estado vacío -->
          <div v-else class="empty-state">
            <div class="empty-icon">📊</div>
            <h3>No hay ventas registradas</h3>
            <p>Las ventas aparecerán aquí cuando los clientes realicen compras</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal de Producto -->
    <div v-if="showProductForm" class="modal-overlay" @click="closeProductForm">
      <div class="modal" @click.stop>
        <div class="modal-header">
          <h3>{{ editingProduct ? 'Editar Producto' : 'Nuevo Producto' }}</h3>
          <button class="modal-close" @click="closeProductForm">✕</button>
        </div>
        <div class="modal-body">
          <form @submit.prevent="saveProduct">
            <div class="form-group">
              <label>Nombre del Producto *</label>
              <input v-model="productForm.name" type="text" class="form-input" required placeholder="Ej: Cono de chocolate" />
            </div>

            <div class="form-group">
              <label>Descripción</label>
              <textarea v-model="productForm.description" class="form-input" rows="3" placeholder="Describe las características principales del producto"></textarea>
            </div>

            <div class="form-row">
              <div class="form-group">
                <label>Precio *</label>
                <div class="price-input">
                  <span class="currency">$</span>
                  <input
                    :value="formatPriceInput(productForm.price)"
                    @input="handlePriceInput($event, 'price')"
                    type="text"
                    class="form-input"
                    required
                    placeholder="1"
                  />
                </div>
              </div>
              <div class="form-group">
                <label>Precio Original (descuento)</label>
                <div class="price-input">
                  <span class="currency">$</span>
                  <input
                    :value="formatPriceInput(productForm.originalPrice)"
                    @input="handlePriceInput($event, 'originalPrice')"
                    type="text"
                    class="form-input"
                    placeholder="0"
                  />
                </div>
              </div>
            </div>

            <div class="form-row">
              <div class="form-group">
                <label>Categoría *</label>
                <select v-model="productForm.category" class="form-input" required>
                  <option value="">Seleccionar categoría</option>
                  <option v-for="category in categories" :key="category.id" :value="category.id">
                    {{ category.name }}
                  </option>
                </select>
              </div>
              <div class="form-group">
                <label>Estado de Disponibilidad *</label>
                <select v-model="productForm.status" class="form-input" required>
                  <option value="available">✅ Disponible</option>
                  <option value="out-of-stock">❌ Sin Stock</option>
                  <option value="coming-soon">🔜 Próximamente</option>
                </select>
              </div>
            </div>

            <!-- Selector de Clasificación -->
            <div class="form-group">
              <label>Clasificación</label>
              <select v-model="productForm.colors[0]" class="form-input">
                <option value="">Seleccionar</option>
                <option value="Nuevo">Nuevo</option>
                <option value="Novedad">Novedad</option>
              </select>
            </div>

            <!-- Imágenes (URLs) -->
            <div class="form-group">
              <label>Imágenes del Producto (URLs) *</label>

              <div class="image-input-section">
                <div class="image-urls">
                  <div v-for="(_, index) in productForm.images" :key="index" class="image-url-row">
                    <input
                      :value="productForm.images[index]"
                      @input="(e) => updateImageUrl(index, (e.target as HTMLInputElement).value)"
                      type="text"
                      class="form-input"
                      placeholder="https://ejemplo.com/imagen.jpg"
                    />
                    <button
                      type="button"
                      class="btn btn-secondary btn-sm"
                      @click="removeSingleImage(index)"
                      :disabled="productForm.images.length === 1"
                      title="Eliminar"
                    >
                      ✕
                    </button>
                  </div>
                </div>

                <div class="image-url-actions">
                  <button type="button" class="btn btn-secondary btn-sm" @click="addImageUrl">
                    + Agregar otra URL
                  </button>
                </div>

                <div v-if="productForm.images.some(i => i.trim())" class="images-preview-grid">
                  <div
                    v-for="(image, index) in productForm.images"
                    :key="`${index}-${image}`"
                    class="image-preview-item"
                    v-show="image.trim()"
                  >
                    <img :src="image" :alt="`Preview ${index + 1}`" />

                    <div class="image-actions">
                      <button
                        type="button"
                        class="img-action-btn"
                        :class="{ primary: index === 0 }"
                        @click="setPrimaryImage(index)"
                        :disabled="index === 0"
                        :title="index === 0 ? 'Imagen principal' : 'Marcar como principal'"
                      >
                        {{ index === 0 ? 'Principal' : 'Hacer principal' }}
                      </button>
                    </div>

                    <span class="image-index">{{ index + 1 }}</span>
                  </div>
                </div>
              </div>
            </div>

            <!-- Información adicional -->
            <div v-if="productForm.originalPrice && productForm.originalPrice > productForm.price" class="discount-info">
              <span class="discount-badge">
                💰 Descuento: {{ Math.round(((productForm.originalPrice - productForm.price) / productForm.originalPrice) * 100) }}%
              </span>
            </div>

            <div class="form-actions">
              <button type="button" class="btn btn-secondary" @click="closeProductForm">Cancelar</button>
              <button type="submit" class="btn btn-primary" :disabled="!isFormValid">
                {{ editingProduct ? 'Actualizar Producto' : 'Crear Producto' }}
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <!-- Modal de Categoría -->
    <div v-if="showCategoryForm" class="modal-overlay" @click="closeCategoryForm">
      <div class="modal" @click.stop>
        <div class="modal-header">
          <h3>{{ editingCategory ? 'Editar Categoría' : 'Nueva Categoría' }}</h3>
          <button class="modal-close" @click="closeCategoryForm">✕</button>
        </div>
        <div class="modal-body">
          <form @submit.prevent="saveCategory">
            <div class="form-group">
              <label>Nombre de la Categoría</label>
              <input v-model="categoryForm.name" type="text" class="form-input" required />
            </div>
            <div class="form-group">
              <label>Descripción</label>
              <textarea v-model="categoryForm.description" class="form-input" rows="3"></textarea>
            </div>
            <div class="form-actions">
              <button type="button" class="btn btn-secondary" @click="closeCategoryForm">Cancelar</button>
              <button type="submit" class="btn btn-primary">{{ editingCategory ? 'Actualizar' : 'Crear' }}</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount, watch } from 'vue'
import { useProducts } from '@/composables/useProducts'
import type { Product } from '@/types/ProductType'
import type { Category, CreateCategoryRequest } from '@/types/CategoryType'
import { paymentService } from '@/services/api/paymentService'
import type { Purchase, ProductPaymentItem } from '@/services/api/paymentService'

// Tipos
interface Sale {
  id: string
  productId: string
  productName: string
  customerName: string
  customerEmail: string
  quantity: number
  unitPrice: number
  totalAmount: number
  status: 'completed' | 'pending' | 'cancelled'
  date: Date
  selectedColor?: string
  items?: ProductPaymentItem[] // Items detallados de la compra
}

// Estado reactivo (persistente)
const ACTIVE_TAB_KEY = 'admin_active_tab'
const getInitialTab = () => {
  try {
    const saved = localStorage.getItem(ACTIVE_TAB_KEY)
    const allowed = ['products', 'categories', 'sales']
    return saved && allowed.includes(saved) ? saved : 'products'
  } catch {
    return 'products'
  }
}

const activeTab = ref<string>(getInitialTab())
const showProductForm = ref(false)
const showCategoryForm = ref(false)
const editingProduct = ref<Product | null>(null)
const editingCategory = ref<Category | null>(null)

// Variables para búsqueda
const searchProducts = ref('')
const searchCategories = ref('')
const searchSales = ref('')

// Filtros (Productos)
const selectedCategoryFilter = ref('')

// Usar el composable de productos
const {
  regularProducts, // Productos regulares (sin showcase) - para mostrar en sección Productos
  categories,
  availableProducts,
  addProduct,
  updateProduct,
  deleteProduct,
  loadCategories,
  loadProducts,
  addCategory,
  updateCategory,
  deleteCategory
} = useProducts()

// Alias para compatibilidad: usar regularProducts en la vista de productos
const products = regularProducts

// Dropdown móvil para filtro de categoría (evita overflow del select nativo)
const categoryDropdownOpen = ref(false)
const categoryDropdownRef = ref<HTMLElement | null>(null)

const selectedCategoryLabel = computed(() => {
  const selectedId = selectedCategoryFilter.value
  if (!selectedId) return 'Todas'
  const found = categories.value.find(c => c.id === selectedId)
  return found?.name || 'Todas'
})

const toggleCategoryDropdown = () => {
  categoryDropdownOpen.value = !categoryDropdownOpen.value
}

const closeCategoryDropdown = () => {
  categoryDropdownOpen.value = false
}

const selectCategoryFilter = (categoryId: string) => {
  selectedCategoryFilter.value = categoryId
  closeCategoryDropdown()
}

const onCategoryDropdownDocumentClick = (event: MouseEvent) => {
  if (!categoryDropdownOpen.value) return
  const target = event.target as Node | null
  const root = categoryDropdownRef.value
  if (!target || !root) {
    closeCategoryDropdown()
    return
  }
  if (!root.contains(target)) closeCategoryDropdown()
}

// Cargar categorías y productos desde el backend al montar el componente
onMounted(async () => {
  console.log('🔄 Cargando categorías y productos al montar el componente...')
  await loadCategories()
  await loadProducts()
  await loadPurchases()
  console.log('✅ Categorías cargadas:', categories.value)
  console.log('✅ Productos cargados:', products.value)

  document.addEventListener('click', onCategoryDropdownDocumentClick)
})

onBeforeUnmount(() => {
  document.removeEventListener('click', onCategoryDropdownDocumentClick)
})

// Watcher para debug: observar cambios en categorías
watch(categories, (newCategories) => {
  console.log('🔔 [Watch] Categorías cambiaron:', newCategories.length, newCategories)
}, { deep: true })

// Sales / Purchases data
const sales = ref<Sale[]>([])
const isLoadingSales = ref(false)
const salesError = ref('')

// Transform Purchase to Sale format
const transformPurchaseToSale = (purchase: Purchase): Sale => {
  const firstItem = purchase.items?.[0]
  const itemCount = purchase.items?.length || 0
  const totalQuantity = purchase.items?.reduce((sum, item) => sum + item.quantity, 0) || 0

  // Generar nombre descriptivo del producto
  let productName = 'Múltiples productos'
  if (itemCount === 1) {
    productName = firstItem?.productName || 'Producto desconocido'
  } else if (itemCount > 1) {
    productName = `${itemCount} productos diferentes`
  }

  return {
    id: purchase.id.toString(),
    productId: '', // No longer available from API
    productName: productName,
    customerName: purchase.buyerName,
    customerEmail: purchase.buyerEmail,
    quantity: totalQuantity, // Suma total de cantidades
    unitPrice: firstItem?.unitPrice || 0,
    totalAmount: purchase.amount, // Use amount directly from API
    status: mapPurchaseStatus(purchase.status),
    date: new Date(purchase.createdAt),
    selectedColor: firstItem?.selectedColor,
    // Información adicional para mostrar detalles
    items: purchase.items
  }
}

// Map purchase status to sale status
const mapPurchaseStatus = (status: string): 'completed' | 'pending' | 'cancelled' => {
  const upperStatus = status.toUpperCase()
  if (upperStatus === 'COMPLETED' || upperStatus === 'APPROVED') return 'completed'
  if (upperStatus === 'CANCELLED' || upperStatus === 'REJECTED') return 'cancelled'
  return 'pending'
}

// Load purchases from API
const loadPurchases = async () => {
  isLoadingSales.value = true
  salesError.value = ''
  try {
    console.log('📦 Cargando compras desde API...')
    const response = await paymentService.getAllPurchases()
    console.log('📦 Respuesta completa de compras:', response)

    if (response.success && response.data) {
      console.log('📦 Purchases raw data:', response.data.purchases)
      sales.value = response.data.purchases.map((purchase) => {
        console.log('📦 Transformando purchase:', {
          id: purchase.id,
          amount: purchase.amount,
          items: purchase.items
        })
        return transformPurchaseToSale(purchase)
      })
      console.log('✅ Compras transformadas:', sales.value)
    } else {
      salesError.value = 'No se pudieron cargar las compras'
      console.error('❌ Error en respuesta de compras:', response)
    }
  } catch (error) {
    salesError.value = 'Error al cargar las compras'
    console.error('❌ Error cargando compras:', error)
  } finally {
    isLoadingSales.value = false
  }
}

// Formularios
const productForm = ref({
  name: '',
  description: '',
  price: 1,
  originalPrice: 0,
  images: [''] as string[],
  category: '',
  status: 'available' as 'available' | 'out-of-stock' | 'coming-soon',
  colors: [''] as string[]
})

// Clasificación del producto (se guarda en productForm.colors[0] por compatibilidad)
const materialOptions = ['Nuevo', 'Novedad']

const categoryForm = ref<CreateCategoryRequest>({
  name: '',
  description: ''
})

// Pestañas
const tabs = [
  { id: 'products', name: 'Productos', icon: '📦' },
  { id: 'categories', name: 'Categorías', icon: '🏷️' },
  // { id: 'sales', name: 'Resumen de Compras', icon: '📊' }
]

// Computed
const availableProductsCount = computed(() =>
  availableProducts.value.length
)

const totalValue = computed(() =>
  sales.value
    .filter(sale => sale.status === 'completed')
    .reduce((sum, sale) => sum + sale.totalAmount, 0)
)

// Computed para estadísticas de ventas
const completedSales = computed(() =>
  sales.value.filter(s => s.status === 'completed')
)

const totalRevenue = computed(() =>
  completedSales.value.reduce((sum, s) => sum + s.totalAmount, 0)
)

const pendingSales = computed(() =>
  sales.value.filter(s => s.status === 'pending').length
)

const totalSalesCount = computed(() => sales.value.length)

// Computed properties para búsqueda y filtrado
const filteredProducts = computed(() => {
  const selectedCategory = selectedCategoryFilter.value
  const baseList = selectedCategory
    ? products.value.filter(p => p.category === selectedCategory)
    : products.value

  if (!searchProducts.value.trim()) {
    return baseList
  }
  const searchLower = searchProducts.value.toLowerCase().trim()
  return baseList.filter(product =>
    product.name.toLowerCase().includes(searchLower) ||
    product.description?.toLowerCase().includes(searchLower)
  )
})

const filteredCategories = computed(() => {
  if (!searchCategories.value.trim()) {
    return categories.value
  }
  const searchLower = searchCategories.value.toLowerCase().trim()
  return categories.value.filter(category =>
    category.name.toLowerCase().includes(searchLower) ||
    category.description?.toLowerCase().includes(searchLower)
  )
})

const filteredSales = computed(() => {
  if (!searchSales.value.trim()) {
    return sales.value
  }
  const searchLower = searchSales.value.toLowerCase().trim()
  return sales.value.filter(sale =>
    sale.customerName.toLowerCase().includes(searchLower) ||
    sale.customerEmail.toLowerCase().includes(searchLower) ||
    sale.productName.toLowerCase().includes(searchLower) ||
    // Buscar también en los items individuales
    (sale.items && sale.items.some(item =>
      item.productName.toLowerCase().includes(searchLower)
    ))
  )
})

// Helper para convertir nombres de colores a hex
const getColorHex = (colorName: string): string => {
  const colorMap: Record<string, string> = {
    'esmeralda': '#76B4F2',
    'naranja cósmico': '#ff5e00',
    'naranja cosmico': '#ff5e00',
    'azul profundo': '#003d5c',
    'plata': '#c0c0c0',
    'silver': '#c0c0c0',
    'azul': '#1976d2',
    'blue': '#1976d2',
    'negro': '#000000',
    'black': '#000000',
    'blanco': '#ffffff',
    'white': '#ffffff',
    'azul neblina': '#a8c7dd',
    'dorado claro': '#f7e7a1',
    'azul cielo': '#87ceeb',
    'rosa': '#ff69b4',
    'pink': '#ff69b4',
    'amarillo': '#ffeb3b',
    'yellow': '#ffeb3b',
    'verde': '#4caf50',
    'green': '#4caf50',
    'púrpura': '#9c27b0',
    'purpura': '#9c27b0',
    'purple': '#9c27b0',
    'morado': '#9c27b0',
    'oro': '#ffd700',
    'gold': '#ffd700'
  }

  const normalized = colorName.toLowerCase().trim()
  return colorMap[normalized] || '#9e9e9e' // Gris por defecto
}

// Cambio de pestaña con persistencia
const selectTab = (tabId: string) => {
  activeTab.value = tabId
  try {
    localStorage.setItem(ACTIVE_TAB_KEY, tabId)
  } catch (e) {
    console.warn('No se pudo persistir la pestaña activa', e)
  }
}

// Métodos
const getStatusText = (status: string) => {
  const statusMap: Record<string, string> = {
    'available': 'Disponible',
    'out-of-stock': 'Sin Stock',
    'coming-soon': 'Próximamente'
  }
  return statusMap[status] || status
}

const getSaleStatusText = (status: string) => {
  const statusMap: Record<string, string> = {
    'completed': 'Completada',
    'pending': 'Pendiente',
    'cancelled': 'Cancelada'
  }
  return statusMap[status] || status
}

const formatDate = (date: Date) => {
  return date.toLocaleDateString('es-CO', {
    year: 'numeric',
    month: 'short',
    day: 'numeric'
  })
}

const getProductsInCategory = (categoryId: string) => {
  return products.value.filter(p => p.category === categoryId).length
}

const editProduct = (product: Product) => {
  editingProduct.value = product
  productForm.value = {
    name: product.name,
    description: product.description,
    price: product.price,
    originalPrice: product.originalPrice || 0,
    images: product.images ? [...product.images] : [],
    category: product.category,
    status: product.status,
    colors: product.colors ? [...product.colors] : ['']
  }
  if (!productForm.value.images || productForm.value.images.length === 0) {
    productForm.value.images = ['']
  }
  showProductForm.value = true
}

const editCategory = (category: Category) => {
  editingCategory.value = category
  categoryForm.value = {
    name: category.name,
    description: category.description || '' // Manejar descripción opcional
  }
  showCategoryForm.value = true
}

const deleteProductConfirm = (id: string) => {
  if (confirm('¿Estás seguro de eliminar este producto?')) {
    deleteProduct(id)
  }
}

// Función wrapper para eliminar categoría con confirmación
const handleDeleteCategory = async (id: string) => {
  const productsCount = getProductsInCategory(id)

  let confirmMessage = '¿Estás seguro de eliminar esta categoría?'

  if (productsCount > 0) {
    confirmMessage = `⚠️ ADVERTENCIA: Esta categoría tiene ${productsCount} producto${productsCount > 1 ? 's' : ''} asociado${productsCount > 1 ? 's' : ''}.\n\n` +
      `Si eliminas esta categoría, ${productsCount > 1 ? 'estos productos' : 'este producto'} quedará${productsCount > 1 ? 'n' : ''} sin categoría asignada.\n\n` +
      `¿Estás seguro de que deseas continuar?`
  }

  if (confirm(confirmMessage)) {
    await deleteCategory(Number(id))
  }
}

// Computed para validación de formulario
const isFormValid = computed(() => {
  const hasImage = productForm.value.images.some(i => i.trim().length > 0)
  return productForm.value.name.trim() !== '' &&
         productForm.value.price > 0 &&
         productForm.value.category !== '' &&
         hasImage
})

const addImageUrl = () => {
  productForm.value.images.push('')
}

const updateImageUrl = (index: number, value: string) => {
  productForm.value.images[index] = value
}

const setPrimaryImage = (index: number) => {
  if (index <= 0) return
  const imgs = productForm.value.images
  const selected = imgs[index]
  imgs.splice(index, 1)
  imgs.unshift(selected)
}

// removeImage eliminado (uso sustituido por removeSingleImage o reinicio manual)

const removeSingleImage = (index: number) => {
  productForm.value.images.splice(index, 1)
  if (productForm.value.images.length === 0) {
    productForm.value.images = ['']
  }
}

// setAsCover removido (no se usa actualmente)

// Función auxiliar para normalizar strings (quitar tildes, espacios extra, etc.)
const normalizeString = (str: string): string => {
  return str
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '') // Quitar tildes y diacríticos
    .trim()
    .replace(/\s+/g, ' ') // Normalizar espacios múltiples a uno solo
}

const isMaterialSelected = (material: string) => {
  const normalizedMaterial = normalizeString(material)
  return productForm.value.colors.some(
    c => normalizeString(c) === normalizedMaterial
  )
}

const toggleProductMaterial = (material: string) => {
  const normalizedMaterial = normalizeString(material)
  const index = productForm.value.colors.findIndex(
    c => normalizeString(c) === normalizedMaterial
  )

  if (index > -1) {
    productForm.value.colors.splice(index, 1)
  } else {
    productForm.value.colors.push(material)
  }
}

// Formatear precio para mostrar con separadores de miles
const formatPriceInput = (value: number): string => {
  if (!value || value === 0) return ''
  return value.toLocaleString('es-CO')
}

// Manejar input de precio y parsear el valor con separadores
const handlePriceInput = (event: Event, field: 'price' | 'originalPrice') => {
  const input = event.target as HTMLInputElement
  const rawValue = input.value.replace(/\./g, '').replace(/[^\d]/g, '')
  const numValue = rawValue ? parseInt(rawValue, 10) : 0
  productForm.value[field] = numValue
}

const saveProduct = () => {
  const payload = {
    ...productForm.value,
    images: productForm.value.images.map(i => i.trim()).filter(Boolean),
    colors: productForm.value.colors.map(c => c.trim()).filter(Boolean)
  }
  if (editingProduct.value) {
    // Actualizar producto existente - mostrar confirmación
    const confirmMessage = `¿Estás seguro de que deseas actualizar el producto "${editingProduct.value.name}"?\n\nSe actualizarán todos los cambios realizados.`
    if (!confirm(confirmMessage)) {
      return
    }
    updateProduct(editingProduct.value.id, payload)
  } else {
    // Crear nuevo producto
    addProduct(payload)
  }
  closeProductForm()
}

const saveCategory = async () => {
  if (editingCategory.value) {
    // Actualizar categoría existente - mostrar confirmación
    const confirmMessage = `¿Estás seguro de que deseas actualizar la categoría "${editingCategory.value.name}"?\n\nSe actualizarán todos los cambios realizados.`
    if (!confirm(confirmMessage)) {
      return
    }
    await updateCategory(Number(editingCategory.value.id), categoryForm.value)
  } else {
    // Crear nueva categoría
    await addCategory(categoryForm.value)
  }
  closeCategoryForm()
}

const closeProductForm = () => {
  showProductForm.value = false
  editingProduct.value = null
  productForm.value = {
    name: '',
    description: '',
    price: 1,
    originalPrice: 0,
    images: [''],
    category: '',
    status: 'available',
    colors: ['']
  }
}

const closeCategoryForm = () => {
  showCategoryForm.value = false
  editingCategory.value = null
  categoryForm.value = {
    name: '',
    description: ''
  }
}
</script>

<style scoped>
.admin-dashboard,
.admin-dashboard *,
.admin-dashboard *::before,
.admin-dashboard *::after {
  box-sizing: border-box;
}

.admin-dashboard {
  /* Variables locales adaptadas a la paleta ODA Gelato */
  --admin-ink: var(--secondary);
  --admin-gold: var(--primary);
  --admin-gold-deep: var(--primary-dark);
  --admin-gold-soft: rgba(118, 180, 242, 0.22);
  --admin-gold-soft-2: rgba(118, 180, 242, 0.32);

  /* Acento esmeralda reemplazado por primary */
  --admin-emerald: var(--primary);
  --admin-emerald-deep: var(--primary-dark);
  --admin-emerald-soft: rgba(118, 180, 242, 0.22);

  /* Override de marca SOLO para el Admin */
  --brand-primary: var(--admin-gold);
  --brand-primary-contrast: rgba(255, 255, 255, 0.96);
  --brand-bg-start: var(--admin-ink);
  --brand-bg-end: var(--secondary-mid);
  --brand-surface: rgba(49, 46, 87, 0.92);
  --brand-border: var(--admin-gold-soft);
  --brand-accent: var(--admin-gold);
  --brand-accent-alt: var(--primary-pale);
  --brand-accent-glow: rgba(118, 180, 242, 0.35);
  --brand-success: #76B4F2;
  --brand-gradient: linear-gradient(135deg, var(--brand-bg-start) 0%, var(--brand-bg-end) 100%);
  --brand-accent-gradient: linear-gradient(135deg, var(--admin-gold) 0%, var(--admin-gold-deep) 100%);

  /* Variables usadas por modales genéricos */
  --overlay-bg: rgba(0, 0, 0, 0.78);
  --backdrop-blur: blur(8px);
  --bg-secondary: var(--brand-surface);
  --bg-tertiary: rgba(255, 255, 255, 0.06);
  --text-primary: rgba(255, 255, 255, 0.96);
  --text-secondary: rgba(255, 255, 255, 0.82);
  --border-primary: var(--brand-border);
  --border-secondary: rgba(118, 180, 242, 0.32);
  --shadow-primary: rgba(0, 0, 0, 0.55);
  --shadow-secondary: rgba(0, 0, 0, 0.35);

  min-height: 100vh;
  background: var(--brand-gradient);
  padding: 20px;
  color: var(--brand-primary-contrast);
  overflow-x: hidden;
}

.dashboard-header {
  text-align: center;
  margin-bottom: 40px;
  padding: 30px;
  background: var(--brand-surface);
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
  border: 1px solid var(--brand-border);
}

.dashboard-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: var(--brand-primary-contrast);
  margin: 3rem 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
}

.dashboard-title .icon {
  font-size: 3rem;
}

.dashboard-title .highlight {
  color: var(--admin-gold);
}

.dashboard-subtitle {
  font-size: 1.1rem;
  color: var(--brand-accent-alt);
  margin: 0;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 40px;
}

.stat-card {
  background: var(--brand-surface);
  border-radius: 16px;
  padding: 25px;
  display: flex;
  align-items: center;
  gap: 20px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  border: 1px solid var(--brand-border);
  transition: all 0.3s ease;
}

.stat-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.4);
  border-color: var(--brand-accent);
}

.stat-icon {
  font-size: 2.5rem;
  background: linear-gradient(135deg, var(--admin-emerald) 0%, var(--admin-emerald-deep) 100%);
  border-radius: 50%;
  width: 70px;
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.stat-number {
  font-size: 2rem;
  font-weight: 700;
  color: var(--brand-primary-contrast);
  line-height: 1;
}

.stat-label {
  font-size: 0.9rem;
  color: var(--brand-accent-alt);
  font-weight: 500;
}

.tabs-container {
  margin-bottom: 30px;
}

.tabs {
  display: flex;
  gap: 5px;
  background: var(--brand-surface);
  padding: 5px;
  border-radius: 12px;
  width: fit-content;
  border: 1px solid var(--brand-border);
}

.tab {
  background: none;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  color: var(--brand-accent-alt);
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 8px;
}

.tab.active {
  background: var(--brand-success);
  color: var(--brand-primary-contrast);
  box-shadow: 0 2px 8px rgba(118, 180, 242, 0.5);
}

.tab:hover:not(.active) {
  background: var(--brand-border);
  color: var(--brand-primary-contrast);
}

.content-section {
  background: var(--brand-surface);
  border-radius: 20px;
  padding: 13px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  border: 1px solid var(--brand-border);
  max-width: 100%;
  overflow-x: hidden;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  padding-bottom: 20px;
  border-bottom: 2px solid var(--brand-border);
}

.section-header h2 {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--brand-primary-contrast);
  margin: 0;
}

/* Barras de búsqueda y filtros en una sola fila */
.search-bar {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  gap: 1rem;
  margin: 0 0 2rem;
  position: relative;
  flex-wrap: wrap;
}

.search-input-wrapper {
  position: relative;
  flex: 1 1 300px;
  max-width: 600px;
  min-width: 0;
}

.search-icon {
  position: absolute;
  left: 14px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--brand-accent-alt);
  pointer-events: none;
  z-index: 1;
}

.search-input {
  width: 100%;
  padding: 0.875rem 3.5rem 0.875rem 2.75rem;
  border-radius: 999px;
  border: 1px solid var(--brand-border);
  background: var(--brand-bg-end);
  font-size: 1rem;
  color: var(--brand-primary-contrast);
  box-shadow: 0 4px 12px rgba(0,0,0,0.2);
  transition: all 0.3s ease;
  outline: none;
}

.search-input:focus {
  background: var(--brand-surface);
  border-color: var(--brand-success);
  box-shadow: 0 0 0 3px rgba(118, 180, 242, 0.2);
}

.search-input:hover {
  border-color: var(--brand-accent);
}

.search-input::placeholder {
  color: var(--brand-accent-alt);
  opacity: 0.7;
}

.search-clear {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(118, 180, 242, 0.1);
  border: none;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--brand-success);
  width: 28px;
  height: 28px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.search-clear:hover {
  background: rgba(118, 180, 242, 0.2);
  transform: translateY(-50%) scale(1.05);
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

/* En celular: 2 productos por fila */
@media (max-width: 640px) {
  .search-bar {
    flex-direction: column;
    align-items: stretch;
  }

  .search-input-wrapper,
  .filter-group {
    flex: 1 1 auto;
    max-width: none;
  }

  .products-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 12px;
  }

  .product-card {
    padding: 14px;
  }

  .product-meta {
    flex-wrap: wrap;
    justify-content: flex-start;
    gap: 10px;
  }
}

.product-card {
  position: relative;
  background: var(--brand-bg-end);
  border-radius: 16px;
  padding: 20px;
  border: 2px solid var(--brand-border);
  transition: all 0.3s ease;
}

.product-card:hover {
  border-color: var(--brand-success);
  box-shadow: 0 8px 25px rgba(118, 180, 242, 0.3);
  transform: translateY(-2px);
}

.classification-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  z-index: 2;
  font-family: var(--ff-display);
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 4px 10px;
  border-radius: 100px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.classification-badge--nuevo {
  background: var(--accent);
  color: white;
}

.classification-badge--novedad {
  background: var(--primary);
  color: var(--secondary);
}

.product-image {
  width: 100%;
  height: 230px;
  border-radius: 12px;
  overflow: hidden;
  margin-bottom: 15px;
  background: var(--brand-border);
  display: flex;
  align-items: center;
  justify-content: center;
}

/* En celular, mantener altura más baja (esta regla va después para que no la sobreescriba la base) */
@media (max-width: 640px) {
  .product-image {
    height: 130px;
  }
}

.product-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.no-image {
  font-size: 3rem;
  color: var(--brand-accent-alt);
}

.product-info h3 {
  font-size: 1.2rem;
  font-weight: 600;
  color: var(--brand-primary-contrast);
  margin: 0 0 8px;
}

.product-description {
  color: var(--brand-accent-alt);
  font-size: 0.9rem;
  margin: 0 0 15px;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}

.product-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.price {
  font-size: 1.3rem;
  font-weight: 700;
  color: rgba(74, 142, 216, 0.95);
}

.status {
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
}

.status.available {
  background: rgba(16, 185, 129, 0.18);
  color: #34d399;
  border: 1px solid rgba(16, 185, 129, 0.35);
}

.status.out-of-stock {
  background: rgba(208, 79, 109, 0.15);
  color: #F9D5DD;
  border: 1px solid rgba(208, 79, 109, 0.35);
}

.status.coming-soon {
  background: rgba(245, 158, 11, 0.15);
  color: #fbbf24;
  border: 1px solid rgba(245, 158, 11, 0.35);
}

/* Botones de acciones (Editar/Eliminar) */
.product-actions .btn-edit,
.product-actions .btn-edit.btn-secondary {
  background: var(--primary) !important;
  color: var(--secondary) !important;
  border: 1px solid var(--primary) !important;
}

.product-actions .btn-edit:hover,
.product-actions .btn-edit.btn-secondary:hover {
  background: var(--primary-dark) !important;
  border-color: var(--primary-dark) !important;
  color: var(--white) !important;
}

.product-actions .btn-delete,
.product-actions .btn-delete.btn-danger {
  background: var(--accent) !important;
  color: var(--white) !important;
  border: 1px solid var(--accent) !important;
}

.product-actions .btn-delete:hover,
.product-actions .btn-delete.btn-danger:hover {
  background: var(--accent-dark) !important;
  border-color: var(--accent-dark) !important;
  color: var(--white) !important;
}

/* Filtros */
.filter-group {
  flex: 0 0 220px;
  display: grid;
  grid-template-columns: 1fr;
  gap: 8px;
  min-width: 0;
}

.filter-label {
  font-weight: 600;
  color: var(--brand-accent-alt);
  font-size: 0.9rem;
}

.filter-select {
  width: 100%;
  max-width: 100%;
  min-width: 0;
  padding: 0.75rem 1rem;
  border-radius: 12px;
  border: 1px solid var(--brand-border);
  background: var(--brand-bg-end);
  color: var(--brand-primary-contrast);
  outline: none;
  box-sizing: border-box;
}

/* Por defecto (desktop): usar select nativo */
.native-select {
  display: block;
}

.custom-select {
  position: relative;
  width: 100%;
  display: none;
}

.filter-select-btn {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  cursor: pointer;
}

.filter-select-text {
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.filter-select-caret {
  flex: 0 0 auto;
  opacity: 0.9;
}

.filter-dropdown {
  position: absolute;
  top: calc(100% + 8px);
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  max-height: 50vh;
  overflow: auto;
  background: var(--brand-bg-end);
  border: 1px solid var(--brand-border);
  border-radius: 12px;
  padding: 6px;
  z-index: 100;
}

.filter-dropdown-item {
  width: 100%;
  text-align: left;
  padding: 10px 12px;
  border-radius: 10px;
  border: none;
  background: transparent;
  color: var(--brand-primary-contrast);
  cursor: pointer;
  white-space: normal;
}

.filter-dropdown-item:hover {
  background: rgba(255, 255, 255, 0.06);
}

.filter-dropdown-item.active {
  background: rgba(118, 180, 242, 0.18);
  border: 1px solid rgba(118, 180, 242, 0.25);
}

/* Móvil: esconder select nativo y usar dropdown controlado */
@media (max-width: 640px) {
  .native-select {
    display: none;
  }

  .custom-select {
    display: block;
  }
}

.filter-select:focus {
  border-color: var(--brand-success);
  box-shadow: 0 0 0 3px rgba(118, 180, 242, 0.2);
}

.product-actions {
  display: flex;
  gap: 10px;
}

.categories-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.category-item {
  background: var(--brand-bg-end);
  border-radius: 12px;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 2px solid var(--brand-border);
  transition: all 0.3s ease;
}

.category-item:hover {
  border-color: var(--brand-success);
  transform: translateY(-1px);
}

.category-info h3 {
  font-size: 1.2rem;
  font-weight: 600;
  color: var(--brand-primary-contrast);
  margin: 0 0 5px;
}

.category-info p {
  color: var(--brand-accent-alt);
  margin: 0 0 5px;
  font-size: 0.9rem;
}

.category-count {
  font-size: 0.8rem;
  color: var(--brand-success);
  font-weight: 600;
}

.category-actions {
  display: flex;
  gap: 10px;
}

.empty-state {
  text-align: center;
  padding: 60px 20px;
  color: var(--brand-accent-alt);
}

.empty-icon {
  font-size: 4rem;
  margin-bottom: 20px;
}

.empty-state h3 {
  font-size: 1.5rem;
  margin: 0 0 10px;
  color: var(--brand-primary-contrast);
}

.empty-state p {
  margin: 0 0 30px;
  font-size: 1rem;
}

/* Loading and Error States */
.loading-state, .error-state {
  text-align: center;
  padding: 60px 20px;
}

.loading-state p, .error-state p {
  margin-top: 20px;
  font-size: 1rem;
  color: var(--brand-accent-alt);
}

.error-state {
  color: var(--admin-gold-deep);
}

.error-icon {
  font-size: 4rem;
  margin-bottom: 20px;
}

.spinner {
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-left-color: var(--brand-primary);
  border-radius: 50%;
  width: 50px;
  animation: spin 1s linear infinite;
  min-width: 0;
  margin: 0 auto;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* Product info in sales table */
.product-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.product-name {
  font-weight: 500;
}

.product-color {
  font-size: 0.85rem;
  color: var(--brand-accent-alt);
}

/* === SELECTOR DE MATERIAL === */
.materials-selector {
  margin-top: 10px;
}

.materials-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 12px;
}

.material-option {
  padding: 10px 14px;
  border-radius: 999px;
  border: 1px solid var(--brand-border);
  background: rgba(255, 255, 255, 0.05);
  color: var(--brand-primary-contrast);
  font-weight: 700;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease, background 0.2s ease;
}

.material-option:hover {
  border-color: var(--admin-emerald);
  background: rgba(118, 180, 242, 0.12);
  transform: translateY(-1px);
}

.material-option.selected {
  border-color: var(--admin-emerald);
  background: rgba(118, 180, 242, 0.18);
  box-shadow: 0 0 0 3px rgba(118, 180, 242, 0.18);
}

.selected-materials {
  padding: 10px 12px;
  background: rgba(118, 180, 242, 0.08);
  border: 1px solid rgba(118, 180, 242, 0.22);
  border-radius: 8px;
  margin-top: 10px;
}

.selected-label {
  font-weight: 600;
  color: rgba(74, 142, 216, 0.95);
  font-size: 0.85rem;
}

.selected-list {
  color: var(--brand-primary-contrast);
  font-size: 0.85rem;
}

.image-url-row {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 10px;
  align-items: center;
  margin-bottom: 10px;
}

.image-url-actions {
  display: flex;
  justify-content: flex-start;
  margin-top: 6px;
}

/* Botones */
.btn {
  padding: 10px 20px;
  border-radius: 8px;
  border: none;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  text-decoration: none;
}

.btn.btn-primary {
  background: var(--brand-accent-gradient);
  color: var(--admin-ink);
  box-shadow: 0 10px 30px rgba(118, 180, 242, 0.28);
}

.btn.btn-primary:hover {
  transform: translateY(-1px);
  box-shadow: 0 18px 46px rgba(118, 180, 242, 0.32);
  background: linear-gradient(135deg, rgba(74, 142, 216, 1) 0%, rgba(118, 180, 242, 1) 100%);
}

.btn-secondary {
  background: var(--brand-border);
  color: var(--brand-primary-contrast);
}

.btn-secondary:hover {
  background: var(--brand-surface);
}

.btn-danger {
  background: rgba(118, 180, 242, 0.14);
  color: var(--admin-gold);
  border: 1px solid var(--admin-gold-soft);
}

.btn-danger:hover {
  background: rgba(118, 180, 242, 0.22);
}

/* Hover específico para acciones de categoría */
.category-actions .btn.btn-secondary:hover {
  background: rgba(118, 180, 242, 0.14);
  border-color: rgba(118, 180, 242, 0.35);
  color: var(--brand-success);
}

.category-actions .btn.btn-danger:hover {
  background: rgba(208, 79, 109, 0.14);
  border-color: rgba(208, 79, 109, 0.35);
  color: var(--brand-danger);
}

.btn-sm {
  padding: 6px 12px;
  font-size: 0.8rem;
}

/* Modal */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
}

.modal {
  background: var(--brand-surface);
  border-radius: 20px;
  width: 100%;
  max-width: 600px;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.6);
  border: 1px solid var(--brand-border);
}

.modal-header {
  padding: 30px 30px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-header h3 {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--brand-primary-contrast);
  margin: 0;
}

.modal-close {
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: var(--brand-accent-alt);
  padding: 5px;
  border-radius: 50%;
  width: 35px;
  height: 35px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.modal-close:hover {
  background: var(--brand-border);
  color: var(--brand-primary-contrast);
}

.modal-body {
  padding: 30px;
}

/* Formularios */
.form-group {
  margin-bottom: 20px;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.form-group label {
  display: block;
  font-weight: 600;
  color: var(--brand-primary-contrast);
  margin-bottom: 8px;
}

.form-input {
  width: 100%;
  padding: 12px 16px;
  border: 2px solid var(--brand-border);
  border-radius: 8px;
  font-size: 1rem;
  transition: border-color 0.3s ease;
  box-sizing: border-box;
  background: var(--brand-bg-end);
  color: var(--brand-primary-contrast);
}

.form-input:focus {
  outline: none;
  border-color: var(--brand-success);
  box-shadow: 0 0 0 3px rgba(118, 180, 242, 0.2);
}

.form-input::placeholder {
  color: var(--brand-accent-alt);
}

.price-input {
  position: relative;
  display: flex;
  align-items: center;
}

.currency {
  position: absolute;
  left: 16px;
  font-weight: 600;
  color: var(--brand-accent-alt);
  z-index: 1;
}

.price-input .form-input {
  padding-left: 35px;
}

/* Subida de imágenes */
.image-upload-area {
  margin-bottom: 15px;
}

.image-preview {
  position: relative;
  width: 200px;
  height: 200px;
  border-radius: 12px;
  overflow: hidden;
  border: 2px solid var(--brand-border);
  margin-bottom: 15px;
}

.image-preview img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.remove-image {
  position: absolute;
  top: 8px;
  right: 8px;
  background: rgba(118, 180, 242, 0.9);
  color: white;
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  transition: background 0.3s ease;
}

.remove-image:hover {
  background: rgba(74, 142, 216, 1);
}

/* Estilos para vista previa de múltiples imágenes */
.images-preview-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 15px;
  max-height: 300px;
  overflow-y: auto;
  padding: 10px;
}

.image-preview-item {
  position: relative;
  width: 120px;
  height: 120px;
  border-radius: 12px;
  overflow: hidden;
  border: 2px solid var(--brand-border);
  background: var(--brand-bg-end);
}

.image-preview-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.remove-single-image {
  position: absolute;
  top: 5px;
  right: 5px;
  background: rgba(118, 180, 242, 0.9);
  color: white;
  border: none;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  transition: background 0.3s ease;
}

.remove-single-image:hover {
  background: rgba(74, 142, 216, 1);
}

.image-index {
  position: absolute;
  bottom: 5px;
  left: 5px;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 11px;
  font-weight: 600;
}

.image-actions {
  position: absolute;
  left: 5px;
  top: 5px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.img-action-btn {
  background: rgba(0,0,0,0.55);
  color: #fff;
  border: none;
  padding: 2px 6px;
  font-size: 10px;
  border-radius: 4px;
  cursor: pointer;
  line-height: 1.1;
  transition: background .2s ease, transform .15s ease;
}

.img-action-btn:hover:not(:disabled) {
  background: rgba(0,0,0,0.8);
  transform: translateY(-1px);
}

.img-action-btn:disabled {
  opacity: .35;
  cursor: default;
}

.img-action-btn.primary {
  background: var(--brand-success);
}

.img-action-btn.primary:hover {
  background: #4A8ED8;
}

.drop-zone {
  border: 2px dashed var(--brand-border);
  border-radius: 12px;
  padding: 40px;
  text-align: center;
  transition: all 0.3s ease;
  background: var(--brand-bg-end);
  cursor: pointer;
}

.drop-zone.dragover {
  border-color: var(--brand-success);
  background: rgba(118, 180, 242, 0.1);
}

.drop-zone:hover {
  border-color: var(--brand-success);
  background: rgba(118, 180, 242, 0.1);
}

.drop-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.upload-icon {
  font-size: 3rem;
  color: var(--brand-accent-alt);
}

.upload-btn {
  background: none;
  border: none;
  color: var(--brand-success);
  font-weight: 600;
  cursor: pointer;
  text-decoration: underline;
}

.upload-btn:hover {
  color: #4A8ED8;
}

.url-input {
  margin-top: 15px;
  padding-top: 15px;
  border-top: 1px solid var(--brand-border);
}

.url-input label {
  font-size: 0.9rem;
  color: var(--brand-accent-alt);
  margin-bottom: 8px;
}

.discount-info {
  background: rgba(118, 180, 242, 0.1);
  border: 1px solid rgba(118, 180, 242, 0.3);
  border-radius: 8px;
  padding: 12px;
  margin-bottom: 20px;
}

.discount-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 10px;
  border-radius: 999px;
  background: rgba(118, 180, 242, 0.18);
  border: 1px solid rgba(118, 180, 242, 0.3);
  color: rgba(255, 255, 255, 0.96);
  font-weight: 700;
  font-size: 0.9rem;
  line-height: 1.1;
}

.form-actions {
  display: flex;
  gap: 15px;
  justify-content: flex-end;
  margin-top: 30px;
  padding-top: 20px;
  border-top: 2px solid var(--brand-border);
}

/* Responsive */
@media (max-width: 768px) {
  .admin-dashboard {
    padding: 10px;
  }

  .dashboard-header {
    text-align: center;
    margin-bottom: 20px;
  }

  .dashboard-title {
    font-size: 1.6rem;
    line-height: 1.2;
  }

  .dashboard-subtitle {
    font-size: 0.9rem;
  }

  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
  }

  .stat-card {
    padding: 12px;
  }

  .stat-number {
    font-size: 1.3rem;
  }

  .stat-label {
    font-size: 0.75rem;
  }

  .stat-icon {
    font-size: 1.8rem;
    width: 50px;
    height: 50px;
  }

  .tabs-container {
    margin-bottom: 20px;
  }

  .tabs {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
  }

  .tab {
    padding: 8px 12px;
    font-size: 0.9rem;
    min-width: auto;
  }

  .tab-icon {
    display: none;
  }

  .section-header {
    flex-direction: column;
    gap: 15px;
    align-items: flex-start;
  }

  .section-header h2 {
    font-size: 1.4rem;
  }

  .products-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 12px;
  }

  .product-card {
    padding: 15px;
  }

  .product-actions {
    flex-direction: column;
    gap: 8px;
  }

  .btn {
    padding: 8px 16px;
    font-size: 0.9rem;
  }

  .form-row {
    grid-template-columns: 1fr;
    gap: 15px;
  }

  .form-actions {
    flex-direction: column;
    gap: 10px;
  }

  .modal {
    margin: 10px;
    max-width: none;
  }

  .modal-header h3 {
    font-size: 1.3rem;
  }

  .modal-body {
    padding: 15px;
  }
}

/* Estilos para la sección de ventas */
.sales-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.sales-table-container {
  background: var(--brand-bg-end);
  border-radius: 12px;
  padding: 20px;
  border: 1px solid var(--brand-border);
  overflow-x: auto;
}

.sales-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.9rem;
}

.sales-table th {
  background: var(--brand-surface);
  color: var(--brand-primary-contrast);
  font-weight: 600;
  padding: 18px 100px;
  text-align: center;
  border-bottom: 2px solid var(--brand-border);
  white-space: nowrap;
  font-size: 0.95rem;
}

.sales-table td {
  padding: 18px 60px;
  border-bottom: 1px solid var(--brand-border);
  color: var(--brand-accent-alt);
  text-align: center;
}

.sale-row {
  transition: background-color 0.3s ease;
}

.sale-row:hover {
  background: var(--brand-surface);
}

.customer-info {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.customer-name {
  font-weight: 600;
  color: var(--brand-primary-contrast);
  font-size: 0.85rem;
}

.customer-email {
  font-size: 0.75rem;
  color: var(--brand-accent-alt);
}

.product-name {
  font-weight: 500;
  color: var(--brand-primary-contrast);
  font-size: 0.85rem;
}

.quantity {
  background: var(--brand-accent);
  color: white;
  padding: 4px 8px;
  border-radius: 8px;
  font-weight: 600;
  display: inline-block;
  min-width: 30px;
  text-align: center;
}

.amount {
  font-weight: 700;
  color: var(--brand-success);
  font-size: 0.9rem;
}

.status-badge {
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  display: inline-block;
}

.status-badge.completed {
  background: rgba(34, 197, 94, 0.2);
  color: #22c55e;
  border: 1px solid rgba(34, 197, 94, 0.3);
}

.status-badge.pending {
  background: rgba(251, 191, 36, 0.2);
  color: #f59e0b;
  border: 1px solid rgba(251, 191, 36, 0.3);
}

.status-badge.cancelled {
  background: rgba(118, 180, 242, 0.14);
  color: rgba(118, 180, 242, 0.98);
  border: 1px solid rgba(118, 180, 242, 0.22);
}

.date {
  color: var(--brand-accent-alt);
  font-size: 0.75rem;
}

/* Estilos para productos múltiples */
.product-info {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.single-product {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.product-color {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.75rem;
  color: var(--brand-accent-alt);
}

.color-dot {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  border: 2px solid #fff;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
}

.multiple-products {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.products-summary {
  display: flex;
  align-items: center;
  gap: 8px;
}

.products-badge {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 600;
  display: inline-block;
}

.products-details {
  margin-top: 4px;
}

.products-toggle {
  cursor: pointer;
  color: var(--brand-accent);
  font-size: 0.75rem;
  font-weight: 500;
  padding: 4px 0;
  user-select: none;
  transition: color 0.2s;
}

.products-toggle:hover {
  color: var(--brand-primary-contrast);
}

.products-toggle::marker {
  color: var(--brand-accent);
}

.products-list {
  list-style: none;
  padding: 8px 0 0 0;
  margin: 4px 0 0 0;
  display: flex;
  flex-direction: column;
  gap: 6px;
  border-top: 1px solid var(--brand-border);
}

.product-item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 6px 8px;
  background: var(--brand-surface);
  border-radius: 6px;
  font-size: 0.8rem;
}

.item-name {
  flex: 1;
  font-weight: 500;
  color: var(--brand-primary-contrast);
}

.item-quantity {
  background: var(--brand-accent);
  color: white;
  padding: 2px 8px;
  border-radius: 10px;
  font-size: 0.7rem;
  font-weight: 600;
}

.item-color {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 0.7rem;
  color: var(--brand-accent-alt);
}

.color-dot-small {
  width: 10px;
  max-width: 100%;
  height: 10px;
  border-radius: 50%;
  border: 1.5px solid #fff;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
}
/* Estilos para cantidad mejorada */
.quantity-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
}

.quantity-header {
  font-size: 0.7rem;
  font-weight: 600;
  color: var(--brand-accent);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  text-align: center;
  width: 100%;
}

.quantity-badge {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  color: white;
  padding: 6px 12px;
  border-radius: 12px;
  font-weight: 700;
  font-size: 0.9rem;
  display: inline-block;
  min-width: 40px;
  text-align: center;
  box-shadow: 0 2px 8px rgba(245, 87, 108, 0.3);
}

.quantity-sublabel {
  font-size: 0.65rem;
  color: var(--brand-accent-alt);
  text-align: left;
  line-height: 1.2;
  width: 100%;
}

.quantity-label {
  font-size: 0.65rem;
  color: var(--brand-accent-alt);
  text-align: center;
  line-height: 1.2;
}

@media (max-width: 768px) {
  .sales-stats {
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
    margin-bottom: 20px;
  }

  .stat-card {
    padding: 12px;
  }

  .sales-table-container {
    overflow-x: scroll;
    padding: 12px;
    border-radius: 8px;
  }

  .sales-table {
    min-width: 400px;
    font-size: 0.75rem;
  }

  .sales-table th:last-child,
  .sales-table td:last-child {
    min-width: 80px;
    width: 80px;
  }

  .sales-table th {
    padding: 8px 6px;
    font-size: 0.7rem;
  }

  .sales-table td {
    padding: 8px 6px;
  }

  .customer-info {
    gap: 1px;
  }

  .customer-name {
    font-size: 0.8rem;
  }

  .customer-email {
    font-size: 0.7rem;
  }

  .product-name {
    font-size: 0.8rem;
  }

  .quantity {
    padding: 2px 6px;
    font-size: 0.7rem;
    min-width: 24px;
  }

  .amount {
    font-size: 0.8rem;
  }

  .status-badge {
    padding: 4px 8px;
    font-size: 0.65rem;
  }

  .date {
    font-size: 0.7rem;
  }
}

/* Estilos para la sección de showcase/novedades */
.showcase-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 20px;
}

.showcase-card {
  background: var(--brand-bg-end);
  border-radius: 16px;
  padding: 20px;
  border: 2px solid var(--brand-border);
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
}

.showcase-card:hover {
  border-color: var(--brand-accent);
  box-shadow: 0 8px 25px rgba(59, 130, 246, 0.3);
  transform: translateY(-2px);
}

.showcase-image {
  width: 100%;
  height: 180px;
  border-radius: 12px;
  overflow: hidden;
  margin-bottom: 15px;
  background: var(--brand-border);
}

.showcase-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.showcase-info {
  flex: 1;
  margin-bottom: 15px;
}

.showcase-info h3 {
  font-size: 1.2rem;
  font-weight: 600;
  color: var(--brand-primary-contrast);
  margin: 0 0 8px;
}

.showcase-description {
  color: var(--brand-accent-alt);
  font-size: 0.9rem;
  margin: 0 0 15px;
  line-height: 1.4;
}

.showcase-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
}

.showcase-category {
  background: var(--brand-accent);
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
  max-width: 100%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.showcase-status {
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
  max-width: 100%;
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.showcase-status.available {
  background: rgba(34, 197, 94, 0.2);
  color: #22c55e;
  border: 1px solid rgba(34, 197, 94, 0.3);
}

.showcase-status.unavailable {
  background: rgba(118, 180, 242, 0.14);
  color: rgba(118, 180, 242, 0.98);
  border: 1px solid var(--admin-gold-soft);
}

.showcase-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  color: var(--brand-primary-contrast);
  font-weight: 500;
}

.checkbox-label input[type="checkbox"] {
  width: 18px;
  height: 18px;
  accent-color: var(--brand-success);
}

/* Estilos para upload de imágenes en showcase */
.image-tabs {
  display: flex;
  margin-bottom: 1rem;
  border-radius: 8px;
  overflow: hidden;
  border: 1px solid #333;
}

.tab-btn {
  flex: 1;
  padding: 0.75rem 1rem;
  background: #2a2a2a;
  color: rgba(255, 255, 255, 0.7);
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
}

.tab-btn.active {
  background: var(--brand-success);
  color: white;
}

.tab-btn:hover:not(.active) {
  background: #333;
  color: rgba(255, 255, 255, 0.9);
}

.image-input-section {
  margin-top: 0.5rem;
}

.file-input {
  display: none;
}

.file-upload-area {
  border: 2px dashed #333;
  border-radius: 12px;
  padding: 2rem;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  background: #1a1a1a;
}

.file-upload-area:hover {
  border-color: var(--brand-success);
  background: rgba(118, 180, 242, 0.05);
}

.upload-placeholder {
  color: rgba(255, 255, 255, 0.7);
}

.upload-placeholder svg {
  color: #555;
  margin-bottom: 1rem;
}

.upload-placeholder p {
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: rgba(255, 255, 255, 0.8);
}

.upload-placeholder span {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.5);
}

.image-preview {
  position: relative;
  display: inline-block;
}

.image-preview img {
  max-width: 200px;
  max-height: 150px;
  border-radius: 8px;
  object-fit: cover;
}

.remove-image {
  position: absolute;
  top: -8px;
  right: -8px;
  background: rgba(118, 180, 242, 0.9);
  color: white;
  border: none;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  font-size: 0.7rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.remove-image:hover {
  background: rgba(74, 142, 216, 1);
  transform: scale(1.1);
}

@media (max-width: 1400px) {
  .showcase-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}

@media (max-width: 1000px) {
  .showcase-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .showcase-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .admin-dashboard {
    padding: 8px;
  }

  .dashboard-title {
    font-size: 1.4rem;
  }

  .dashboard-subtitle {
    font-size: 0.85rem;
  }

  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
  }

  .stat-card {
    padding: 10px;
  }

  .stat-number {
    font-size: 1.1rem;
  }

  .stat-label {
    font-size: 0.7rem;
  }

  .stat-icon {
    font-size: 1.5rem;
    width: 40px;
    height: 40px;
  }

  .tabs {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
  }

  .tab {
    padding: 10px;
    text-align: center;
    border-radius: 8px;
  }

  .section-header h2 {
    font-size: 1.2rem;
  }

  .product-card {
    padding: 12px;
  }

  .product-info h3 {
    font-size: 1rem;
  }

  .product-description {
    font-size: 0.8rem;
    line-height: 1.1;
  }

  .price {
    font-size: 0.9rem;
  }

  .btn {
    padding: 6px 12px;
    font-size: 0.8rem;
  }

  .modal-header h3 {
    font-size: 1.1rem;
  }

  .form-group label {
    font-size: 0.9rem;
  }

  .form-input,
  .form-textarea,
  .form-select {
    padding: 8px;
    font-size: 0.9rem;
  }

  .showcase-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }

  .showcase-card {
    padding: 15px;
  }

  .showcase-image {
    height: 160px;
  }

  .showcase-info h3 {
    font-size: 1rem;
  }

  .showcase-description {
    font-size: 0.8rem;
  }

  .showcase-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
  }

  .file-upload-area {
    padding: 1.2rem;
  }

  .upload-placeholder svg {
    width: 28px;
    height: 28px;
  }

  .upload-text {
    font-size: 0.8rem;
  }

  /* Sales section optimizations for very small screens */
  .sales-stats {
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
    margin-bottom: 15px;
  }

  .sales-table-container {
    padding: 8px;
    margin: 0 -8px;
  }

  .sales-table {
    min-width: 350px;
    font-size: 0.7rem;
  }

  .sales-table th:last-child,
  .sales-table td:last-child {
    min-width: 90px;
    width: 90px;
  }

  .sales-table th {
    padding: 6px 4px;
    font-size: 0.65rem;
  }

  .sales-table td {
    padding: 6px 4px;
  }

  .customer-name {
    font-size: 0.75rem;
  }

  .customer-email {
    font-size: 0.65rem;
  }

  .product-name {
    font-size: 0.75rem;
  }

  .quantity {
    padding: 2px 4px;
    font-size: 0.65rem;
    min-width: 20px;
  }

  .amount {
    font-size: 0.75rem;
  }

  .status-badge {
    padding: 3px 6px;
    font-size: 0.6rem;
  }

  .date {
    font-size: 0.65rem;
  }
}
</style>
