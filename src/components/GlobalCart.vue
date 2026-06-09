<template>
  <!-- Carrito global: oculto en Admin; botón solo si hay productos -->
  <div v-if="!isAdminRoute" class="global-cart">
    <button
      v-if="shouldShowCartButton"
      type="button"
      class="floating-cart"
      @click="handleToggleCart"
      aria-label="Abrir carrito"
    >
      <div class="cart-icon" aria-hidden="true">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 6h15l-1.5 9H7.5L6 6Z" />
          <path d="M6 6 5 3H2" />
          <circle cx="9" cy="20" r="1.6" />
          <circle cx="18" cy="20" r="1.6" />
        </svg>
        <span v-if="totalItems > 0" class="cart-badge">{{ totalItems }}</span>
      </div>
      <div class="cart-tooltip">Ver carrito de compras</div>
    </button>

    <!-- Modal del carrito -->
    <div v-if="isCartOpen && totalItems > 0" class="cart-overlay" @click="closeCart">
      <div class="cart-modal" @click.stop>
        <div class="cart-header">
          <h3>Tu Carrito</h3>
          <button type="button" @click="closeCart" class="close-btn" aria-label="Cerrar carrito">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="m18 6-12 12" />
              <path d="m6 6 12 12" />
            </svg>
          </button>
        </div>

        <div class="cart-content">
          <div v-if="cartItems.length === 0" class="empty-cart">
            <p>Tu carrito está vacío</p>
          </div>

          <div v-else class="cart-items">
            <div v-for="item in cartItems" :key="`${item.id}-${item.selectedColor || 'default'}`" class="cart-item">
              <img :src="item.image" :alt="item.name" />
              <div class="item-details">
                <h4 class="item-title">{{ item.name }}</h4>
                <span v-if="item.category && isNaN(Number(item.category))" class="item-category">{{ item.category }}</span>
                <div v-if="item.characteristics && item.characteristics.length > 0" class="item-characteristics">
                  <span v-for="(char, index) in item.characteristics" :key="index" class="item-char-badge">
                    {{ char }}
                  </span>
                </div>
                <div class="item-price">${{ item.price.toLocaleString() }} COP</div>
              </div>

              <div class="item-controls">
                <div class="quantity-controls" aria-label="Cantidad">
                  <button
                    type="button"
                    class="quantity-btn minus"
                    @click="updateQuantity(item.id, item.quantity - 1, item.selectedColor)"
                    aria-label="Disminuir cantidad"
                  >
                    -
                  </button>
                  <span class="quantity-value">{{ item.quantity }}</span>
                  <button
                    type="button"
                    class="quantity-btn plus"
                    @click="updateQuantity(item.id, item.quantity + 1, item.selectedColor)"
                    aria-label="Aumentar cantidad"
                  >
                    +
                  </button>
                </div>

                <button
                  type="button"
                  class="remove-btn"
                  @click="removeFromCart(item.id, item.selectedColor)"
                  aria-label="Eliminar producto"
                >
                  <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M3 6h18" />
                    <path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6" />
                    <path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2" />
                  </svg>
                </button>
              </div>
            </div>
          </div>
        </div>

        <div v-if="cartItems.length > 0" class="cart-footer">
          <div class="cart-total-display">
            <strong>Total: ${{ totalPrice.toLocaleString() }} COP</strong>
          </div>
          <div class="cart-actions">
            <button type="button" @click="clearCart" class="btn-clear">Limpiar carrito</button>
            <button type="button" @click="goToCheckout" class="btn-checkout">Finalizar Pedido</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { useCart } from '@/composables/useCart'

const router = useRouter()
const route = useRoute()

const {
  cartItems,
  isCartOpen,
  totalItems,
  totalPrice,
  removeFromCart,
  updateQuantity,
  clearCart,
  toggleCart,
  closeCart
} = useCart()

const isAdminRoute = computed(() => route.path.startsWith('/admin'))
const shouldShowCartButton = computed(() => totalItems.value > 0)

const handleToggleCart = () => {
  if (totalItems.value <= 0) return
  toggleCart()
}

watch(
  () => isAdminRoute.value,
  (isAdmin) => {
    if (isAdmin) closeCart()
  },
  { immediate: true }
)

watch(
  () => totalItems.value,
  (count) => {
    if (count <= 0) closeCart()
  },
  { immediate: true }
)

const goToCheckout = () => {
  closeCart()
  router.push('/checkout')
}
</script>

<style scoped>
.global-cart {
  /* Paleta Angelie (misma línea usada en ProductStore) */
  --cart-ink: #071e25;
  --cart-gold: rgb(201, 168, 89);
  --cart-gold-deep: rgb(215, 172, 67);
  --cart-gold-glow: rgba(201, 168, 89, 0.35);
}

/* Carrito flotante */
.floating-cart {
  position: fixed;
  bottom: 500px;
  right: 20px;
  background: var(--cart-gold);
  color: var(--cart-ink);
  padding: 1rem;
  border-radius: 50%;
  cursor: pointer;
  box-shadow: 0 10px 30px rgba(201, 168, 89, 0.28);
  transition: all 0.3s ease;
  z-index: 2500;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 60px;
  height: 60px;
  border: 1px solid rgba(7, 30, 37, 0.22);
}

.floating-cart:hover {
  transform: translateY(-3px);
  box-shadow: 0 18px 46px rgba(201, 168, 89, 0.32);
}

.cart-icon {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.cart-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  background: var(--cart-ink);
  color: rgba(255, 255, 255, 0.96);
  border-radius: 50%;
  width: 20px;
  height: 20px;
  font-size: 0.8rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 800;
  border: 1px solid rgba(201, 168, 89, 0.35);
}

.cart-tooltip {
  position: absolute;
  right: 70px;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(7, 30, 37, 0.92);
  color: rgba(255, 255, 255, 0.96);
  padding: 0.5rem 1rem;
  border-radius: 10px;
  font-size: 0.85rem;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  pointer-events: none;
  z-index: 2501;
  border: 1px solid rgba(201, 168, 89, 0.28);
}

.cart-tooltip::after {
  content: '';
  position: absolute;
  left: 100%;
  top: 50%;
  transform: translateY(-50%);
  border: 6px solid transparent;
  border-left-color: rgba(7, 30, 37, 0.92);
}

.floating-cart:hover .cart-tooltip {
  opacity: 1;
  visibility: visible;
}

/* Modal del carrito */
.cart-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.75);
  z-index: 3000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  backdrop-filter: blur(8px);
}

.cart-modal {
  background: var(--cart-ink);
  border-radius: 20px;
  max-width: 900px;
  width: 100%;
  max-height: 85vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  border: 1px solid rgba(201, 168, 89, 0.22);
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.6);
}

.cart-header {
  padding: 1.5rem 1.5rem 1rem;
  border-bottom: 1px solid rgba(201, 168, 89, 0.18);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cart-header h3 {
  margin: 0;
  font-size: 1.45rem;
  color: rgba(255, 255, 255, 0.96);
}

.close-btn {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(201, 168, 89, 0.2);
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 12px;
  transition: background 0.2s ease, transform 0.2s ease;
  color: rgba(255, 255, 255, 0.92);
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.12);
  transform: translateY(-1px);
}

.cart-content {
  flex: 1;
  overflow-y: auto;
  padding: 1rem 1.5rem;
}

.empty-cart {
  text-align: center;
  padding: 3rem 0;
  color: rgba(255, 255, 255, 0.75);
}

.cart-items {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.cart-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.06);
  border-radius: 16px;
  border: 1px solid rgba(201, 168, 89, 0.14);
}

.cart-item img {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 12px;
  border: 1px solid rgba(201, 168, 89, 0.18);
}

.item-details {
  flex: 1;
}

.item-details .item-title {
  margin: 0 0 0.25rem;
  color: rgba(255, 255, 255, 0.96);
  text-transform: uppercase;
}

.item-details h4 {
  margin: 0 0 0.25rem;
  color: rgba(255, 255, 255, 0.96);
}

.item-category {
  font-size: 0.9rem;
  color: rgba(201, 168, 89, 0.98);
  font-weight: 700;
  text-transform: lowercase;
}

.item-color {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.86);
  background: rgba(201, 168, 89, 0.14);
  padding: 0.2rem 0.6rem;
  border-radius: 12px;
  display: inline-block;
  margin-top: 0.3rem;
  font-weight: 600;
  border: 1px solid rgba(201, 168, 89, 0.2);
}

.item-characteristics {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-top: 0.4rem;
}

.item-char-badge {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.9);
  background: rgba(201, 168, 89, 0.18);
  padding: 0.2rem 0.55rem;
  border-radius: 10px;
  display: inline-block;
  font-weight: 600;
  border: 1px solid rgba(201, 168, 89, 0.25);
  text-transform: lowercase;
}

.item-price {
  font-weight: 800;
  color: rgba(201, 168, 89, 0.98);
  margin-top: 0.45rem;
}

.item-controls {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.6rem;
}

.quantity-controls {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(201, 168, 89, 0.16);
  padding: 0.35rem 0.5rem;
  border-radius: 12px;
}

.quantity-value {
  min-width: 20px;
  text-align: center;
  font-weight: 800;
  color: rgba(255, 255, 255, 0.95);
}

.quantity-btn {
  border: none;
  width: 28px;
  height: 28px;
  border-radius: 10px;
  cursor: pointer;
  transition: transform 0.2s ease, background 0.2s ease;
  font-weight: 900;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.quantity-btn.minus {
  background: rgba(255, 255, 255, 0.08);
  color: rgba(255, 255, 255, 0.92);
}

.quantity-btn.plus {
  background: rgba(201, 168, 89, 0.92);
  color: var(--cart-ink);
}

.quantity-btn:hover {
  transform: translateY(-1px);
}

.remove-btn {
  background: rgba(201, 168, 89, 0.18);
  color: rgba(255, 255, 255, 0.95);
  border: 1px solid rgba(201, 168, 89, 0.22);
  border-radius: 12px;
  width: 34px;
  height: 34px;
  cursor: pointer;
  transition: transform 0.2s ease, background 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.remove-btn:hover {
  background: rgba(215, 172, 67, 0.28);
  transform: scale(1.06);
}

.cart-footer {
  padding: 1rem 1.5rem 1.5rem;
  border-top: 1px solid rgba(201, 168, 89, 0.18);
}

.cart-total-display {
  text-align: center;
  margin-bottom: 1.25rem;
  font-size: 1.25rem;
  color: rgba(255, 255, 255, 0.96);
  font-weight: 800;
}

.cart-actions {
  display: flex;
  gap: 0.75rem;
  justify-content: center;
  flex-wrap: wrap;
}

.btn-clear,
.btn-checkout {
  border: 1px solid rgba(201, 168, 89, 0.28);
  border-radius: 12px;
  padding: 0.75rem 1.2rem;
  cursor: pointer;
  font-weight: 800;
  transition: transform 0.2s ease, background 0.2s ease;
}

.btn-clear {
  background: rgba(255, 255, 255, 0.06);
  color: rgba(255, 255, 255, 0.92);
}

.btn-checkout {
  background: rgba(201, 168, 89, 0.92);
  color: var(--cart-ink);
}

.btn-clear:hover,
.btn-checkout:hover {
  transform: translateY(-1px);
}

@media (max-width: 768px) {
  .cart-overlay {
    padding: 0.5rem;
  }

  .floating-cart {
    bottom: 420px;
    right: 16px;
    width: 56px;
    height: 56px;
  }

  .cart-modal {
    max-height: 95vh;
    border-radius: 16px;
  }

  .cart-header {
    padding: 1rem;
  }

  .cart-header h3 {
    font-size: 1.2rem;
  }

  .cart-content {
    padding: 0.75rem;
  }

  .cart-item {
    flex-wrap: wrap;
    padding: 0.75rem;
    gap: 0.75rem;
  }

  .cart-item img {
    width: 70px;
    height: 70px;
  }

  .item-details {
    flex: 1;
    min-width: 0;
  }

  .item-details h4 {
    font-size: 0.95rem;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .item-category {
    font-size: 0.8rem;
  }

  .item-color {
    font-size: 0.75rem;
    padding: 0.15rem 0.5rem;
  }

  .item-price {
    font-size: 0.95rem;
  }

  .item-controls {
    flex-direction: row;
    width: 100%;
    justify-content: space-between;
  }

  .quantity-controls {
    gap: 0.4rem;
    padding: 0.3rem 0.4rem;
  }

  .quantity-btn {
    width: 26px;
    height: 26px;
    font-size: 0.9rem;
  }

  .remove-btn {
    width: 32px;
    height: 32px;
  }

  .cart-footer {
    padding: 1rem;
  }

  .cart-total-display {
    font-size: 1.1rem;
    margin-bottom: 1rem;
  }

  .cart-actions {
    flex-direction: column;
    gap: 0.5rem;
  }

  .btn-clear,
  .btn-checkout {
    width: 100%;
    padding: 0.75rem 1rem;
  }
}

@media (max-width: 480px) {
  .cart-overlay {
    padding: 0.25rem;
  }

  .cart-modal {
    max-height: 97vh;
    border-radius: 12px;
  }

  .cart-header {
    padding: 0.75rem;
  }

  .cart-header h3 {
    font-size: 1.1rem;
  }

  .cart-content {
    padding: 0.5rem;
  }

  .cart-item {
    padding: 0.5rem;
    gap: 0.5rem;
  }

  .cart-item img {
    width: 60px;
    height: 60px;
  }

  .item-details h4 {
    font-size: 0.85rem;
  }

  .item-category {
    font-size: 0.75rem;
  }

  .item-price {
    font-size: 0.85rem;
  }

  .quantity-controls {
    gap: 0.3rem;
    padding: 0.25rem 0.35rem;
  }

  .quantity-btn {
    width: 24px;
    height: 24px;
    font-size: 0.85rem;
  }

  .quantity-value {
    font-size: 0.85rem;
    min-width: 18px;
  }

  .remove-btn {
    width: 28px;
    height: 28px;
  }

  .remove-btn svg {
    width: 14px;
    height: 14px;
  }

  .cart-footer {
    padding: 0.75rem 0.5rem;
  }

  .cart-total-display {
    font-size: 1rem;
    margin-bottom: 0.75rem;
  }

  .btn-clear,
  .btn-checkout {
    padding: 0.65rem 0.85rem;
    font-size: 0.9rem;
  }
}
</style>
