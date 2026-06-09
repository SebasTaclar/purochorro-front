<template>
  <!-- ══════════════════════════════════════════
   FOOTER
══════════════════════════════════════════ -->
<footer class="footer">
  <div class="footer-grid">
    <div class="footer-brand">
      <img src="/images/logo.png" alt="ODA Gelato" class="footer-logo-img">
      <!-- <img src="/public/assets/logos/logo-oda-gelato-white.svg" style="height:36px"> -->
      <p>Fabricante colombiano de helados premium para marcas propias. 19 años de experiencia, tecnología italiana y vocación de exportación.</p>
      <div class="f-social">
        <!-- <a href="#" class="fsoc" aria-label="Instagram"><svg viewBox="0 0 24 24"><rect x="2" y="2" width="20" height="20" rx="5"/><path d="M16 11.37A4 4 0 1112.63 8 4 4 0 0116 11.37z"/><line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/></svg></a> -->
        <a href="https://www.youtube.com/watch?v=xGjQ-bp3tCo" class="fsoc" target="_blank" rel="noopener" aria-label="YouTube"><svg viewBox="0 0 24 24"><rect x="1" y="5" width="22" height="14" rx="3" ry="3" fill="none"/><polygon points="10 9 16 12 10 15 10 9" fill="none"/></svg></a>
        <a href="https://wa.me/573202002489" class="fsoc" aria-label="WhatsApp"><svg viewBox="0 0 24 24"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg></a>
      </div>
    </div>
    <div class="footer-col">
      <h4>Empresa</h4>
      <ul>
        <li><a href="#about">Quiénes Somos</a></li>
        <li><a href="#portafolio">Portafolio</a></li>
        <li><a href="#valor">Propuesta de Valor</a></li>
        <li><a href="#portafolio">Tienda</a></li>
        <li><a href="#contacto">Contacto</a></li>
      </ul>
    </div>
    <div class="footer-col">
      <h4>Documentos</h4>
      <ul>
        <li><a href="/public/docs/politica-calidad.pdf" target="_blank">Política de Calidad</a></li>
        <li><a href="/public/docs/politica-inocuidad.pdf" target="_blank">Política de Inocuidad</a></li>
        <li><a href="/public/docs/terminos-legales.pdf" target="_blank">Términos Legales</a></li>
        <li><a href="/public/docs/politica-privacidad.pdf" target="_blank">Política de Privacidad</a></li>
      </ul>
    </div>
    <div class="footer-col">
      <h4>Contactos</h4>
      <ul>
        <li><a href="mailto:servicioalcliente@odagelato.com.co">Servicio al Cliente</a></li>
        <li><a href="mailto:gerencia.comercial@odagelato.com.co">Ser cliente / Aliado</a></li>
        <li><a href="mailto:compras@odagelato.com.co">¿Quieres ser proveedor?</a></li>
        <li><a href="mailto:coordinador.gestionhumana@odagelato.com.co">Trabaje con nosotros</a></li>
        <li><a href="mailto:contador@odagelato.com.co">Contabilidad</a></li>
      </ul>
    </div>
  </div>



  <div class="footer-bottom">
    <p>© 2026 ODA GELATO S.A.S. | Desarrollado por <a href="https://www.dator.org" target="_blank" rel="noopener">DataOr</a></p>
    <div class="footer-legal-links">
      <a href="/public/docs/politica-privacidad.pdf">Privacidad</a>
      <a href="/public/docs/terminos-legales.pdf">Términos</a>
      <a href="mailto:contador@odagelato.com.co">Contabilidad</a>
    </div>
  </div>
</footer>
</template>

<script setup lang="ts"></script>

<style scoped>
/* ═══════════════════════════════════════════════
   FOOTER
═══════════════════════════════════════════════ */
.footer{background:var(--secondary);padding:72px 48px 32px}
.footer-grid{
  max-width:1280px;margin:0 auto;
  display:grid;grid-template-columns:2fr 1fr 1fr 1fr;
  gap:52px;padding-bottom:48px;
  border-bottom:1px solid rgba(255,255,255,.07);
}
.footer-logo-img {
  height: 48px;
  width: auto;
  display: block;
  background: var(--white);
  border-radius: var(--r-sm);
  padding: 4px 10px;
  object-fit: contain;
  margin-bottom: 12px;
}
.footer-brand p{
  font-size:.82rem;color:rgba(255,255,255,.38);
  line-height:1.8;margin:16px 0 24px;max-width:260px;
}
.f-social{display:flex;gap:8px}
.fsoc{
  width:36px;height:36px;border-radius:50%;
  background:rgba(255,255,255,.06);
  border:1px solid rgba(255,255,255,.1);
  display:flex;align-items:center;justify-content:center;
  transition:background .2s,border-color .2s;text-decoration:none;
}
.fsoc:hover{background:rgba(118,180,242,.2);border-color:rgba(118,180,242,.4)}
.fsoc svg{width:14px;height:14px;stroke:rgba(255,255,255,.5);stroke-width:1.8;fill:none;stroke-linecap:round;stroke-linejoin:round}

.footer-col h4{
  font-family:var(--ff-display);
  font-size:.62rem;font-weight:800;
  letter-spacing:.14em;text-transform:uppercase;
  color:rgba(255, 255, 255, 0.658);margin-bottom:18px;
}
.footer-col ul{list-style:none;display:flex;flex-direction:column;gap:10px}
.footer-col ul li a{
  font-size:.82rem;color:rgba(255,255,255,.35);
  transition:color .2s;text-decoration:none;
}
.footer-col ul li a:hover{color:var(--primary)}

.footer-forms{
  max-width:1280px;margin:48px auto 0;
  display:grid;grid-template-columns:repeat(3,1fr);
  gap:20px;
}
.footer-form-card{
  background:rgba(255,255,255,.04);
  border:1px solid rgba(255,255,255,.07);
  border-radius:var(--r-lg);padding:24px;
  transition:border-color .25s,background .25s;
}
.footer-form-card:hover{background:rgba(255,255,255,.06);border-color:rgba(118,180,242,.2)}
.ffc-title{
  font-family:var(--ff-display);
  font-size:.78rem;font-weight:700;
  color:var(--white);margin-bottom:5px;letter-spacing:-.01em;
}
.ffc-desc{font-size:.72rem;color:rgba(255,255,255,.35);margin-bottom:16px;line-height:1.55}
.ffc-input{
  width:100%;padding:9px 12px;
  background:rgba(255,255,255,.06);
  border:1px solid rgba(255,255,255,.1);
  border-radius:var(--r-sm);
  color:var(--white);font-family:var(--ff-body);font-size:.8rem;
  outline:none;margin-bottom:8px;
  transition:border-color .2s,background .2s;
}
.ffc-input::placeholder{color:rgba(255,255,255,.25)}
.ffc-input:focus{border-color:rgba(118,180,242,.4);background:rgba(255,255,255,.09)}
.ffc-btn{
  width:100%;padding:9px;border-radius:var(--r-sm);
  background:rgba(118,180,242,.15);
  border:1px solid rgba(118,180,242,.25);
  color:var(--primary);
  font-family:var(--ff-display);
  font-size:.65rem;font-weight:700;letter-spacing:.06em;
  text-transform:uppercase;
  transition:background .2s,border-color .2s;
}
.ffc-btn:hover{background:rgba(118,180,242,.25);border-color:rgba(118,180,242,.5)}

.footer-bottom{
  max-width:880px;margin:28px auto 0;
  display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:12px;
}
.footer-bottom p{font-size:.72rem;color:rgba(255,255,255,.2)}
.footer-bottom p a{color:rgba(255,255,255,.35);text-decoration:none;transition:color .2s}
.footer-bottom p a:hover{color:var(--primary)}
.footer-legal-links{display:flex;gap:18px}
.footer-legal-links a{font-size:.7rem;color:rgba(255,255,255,.2);transition:color .2s;text-decoration:none}
.footer-legal-links a:hover{color:var(--primary)}

@media (max-width: 768px) {
  .footer { padding: 48px 24px 24px; }
  .footer-grid {
    grid-template-columns: 1fr;
    gap: 32px;
    padding-bottom: 32px;
    text-align: center;
  }
  .footer-brand {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .footer-logo-img { margin: 0 auto 12px; }
  .footer-brand p { max-width: 100%; margin: 8px 0 20px; }
  .f-social { justify-content: center; }
  .footer-col h4 { margin-bottom: 12px; }
  .footer-forms {
    grid-template-columns: 1fr;
    margin-top: 32px;
  }
  .footer-bottom {
    flex-direction: column;
    text-align: center;
    gap: 8px;
  }
  .footer-legal-links { justify-content: center; flex-wrap: wrap; }
}
</style>

