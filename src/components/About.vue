<template>
  <section class="about-section section">
    <div class="about-content section-inner">
      <div class="about-text">
        <span class="about-label">— ¿QUIÉNES SOMOS?</span>
        <h2 class="about-title">
          Empoderamos marcas,<br />
          <span class="about-title-accent">dominamos el helado.</span>
        </h2>
        <p class="about-paragraph">
          ODA Gelato es una compañía colombiana especializada en el desarrollo y producción de helados para marcas propias. Ayudamos a supermercados, distribuidores, restaurantes y emprendedores a lanzar y escalar sus marcas sin asumir la complejidad operativa de una planta de producción.
        </p>
        <p class="about-paragraph">
          Con este compromiso, ODA GELATO S.A.S.  busca satisfacer la necesidad de sus clientes, contribuyendo   al bienestar del consumidor y fortaleciendo su posicionamiento en el sector de alimentos.
        </p>
        <a href="#" class="about-cta">
          Conoce más de nosotros
          <span class="about-cta-arrow">→</span>
        </a>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
defineOptions({ name: 'AboutSection' });
</script>

<style scoped>
.about-section {
  width: 100%;
  min-height: 520px;
  background-color: var(--gray-bg);
  overflow: hidden;
  display: flex;
  align-items: center;
}

.section{padding:100px 48px;position:relative}
.section-inner{max-width:1280px;margin:0 auto}

/* Imagen de fondo con difuminado hacia la izquierda */
.about-section::before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  width: 65%;
  height: 100%;
  background-image: url('https://res.cloudinary.com/dlwzazojt/image/upload/q_auto/f_auto/v1780677687/ChatGPT_Image_5_jun_2026_11_40_24_mstaao.png');
  background-size: cover;
  background-position: center right;
  background-repeat: no-repeat;
  mask-image: linear-gradient(
    to right,
    rgba(0, 0, 0, 0) 0%,
    rgba(0, 0, 0, 0.4) 25%,
    rgba(0, 0, 0, 1) 55%
  );
  -webkit-mask-image: linear-gradient(
    to right,
    rgba(0, 0, 0, 0) 0%,
    rgba(0, 0, 0, 0.4) 25%,
    rgba(0, 0, 0, 1) 55%
  );
  z-index: 1;
}

/* Capa adicional para suavizar el borde entre imagen y fondo */
.about-section::after {
  content: '';
  position: absolute;
  top: 0;
  left: 35%;
  width: 40%;
  height: 100%;
  background: linear-gradient(
    to right,
    var(--gray-bg) 0%,
    rgba(244, 246, 251, 0.95) 30%,
    rgba(244, 246, 251, 0.6) 60%,
    transparent 100%
  );
  z-index: 2;
  pointer-events: none;
}

.about-content {
  position: relative;
  z-index: 3;
  width: 100%;
  display: flex;
  align-items: center;
}

.about-text {
  max-width: 480px;
}

.about-label {
 display: inline-flex;
    align-items: center;
    gap: 8px;
    font-family: var(--ff-display);
    font-size: .62rem;
    font-weight: 600;
    letter-spacing: .14em;
    text-transform: uppercase;
    color: var(--primary-dark);
    margin-bottom: 14px;
}

.about-title {
  font-family: var(--ff-display);
  font-size: clamp(1.8rem,4vw,3rem);
  font-weight: 800;
  line-height: 1.15;
  color: var(--secondary);
  margin: 0 0 24px 0;
  letter-spacing: -0.02em;
}

.about-title-accent {
  color: var(--primary);
  font-weight: 800;
}

.about-paragraph {
  font-family: var(--ff-body);
  font-size: 0.95rem;
  line-height: 1.6;
  color: var(--gray);
  margin: 0 0 16px 0;
}

.about-paragraph:last-of-type {
  margin-bottom: 28px;
}

.about-cta {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background-color: var(--secondary);
  color: var(--white);
  font-family: var(--ff-body);
  font-size: 0.9rem;
  font-weight: 600;
  padding: 14px 24px;
  border-radius: var(--r);
  text-decoration: none;
  transition: background-color 0.3s var(--ease-out), transform 0.2s var(--ease-out);
}

.about-cta:hover {
  background-color: var(--secondary-mid);
  transform: translateY(-2px);
}

.about-cta-arrow {
  font-size: 1.1rem;
  transition: transform 0.2s var(--ease-out);
}

.about-cta:hover .about-cta-arrow {
  transform: translateX(4px);
}

/* Responsive */
@media (max-width: 768px) {
  .about-section {
    min-height: auto;
  }

  .section {
    padding: 60px 24px;
  }

  .about-section::before {
    width: 100%;
    opacity: 0.15;
    mask-image: none;
    -webkit-mask-image: none;
  }

  .about-section::after {
    display: none;
  }

  .about-text {
    max-width: 100%;
  }

  .about-title {
    font-size: 1.8rem;
  }
}
</style>
