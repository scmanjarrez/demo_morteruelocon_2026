<script setup lang="ts">
import { computed, ref } from 'vue';
import { routes } from '@/router';
import AppPage from '@/components/AppPage.vue';
import AppLink from '@/components/AppLink.vue';
import { popup } from '@tma.js/sdk-vue';

const clipboardDemo = computed(() => routes.find((r) => r.name === 'clipboard-demo'));
const clipboardContent = ref<string>('');
const isReading = ref(false);

async function readClipboard() {
  isReading.value = true;
  try {
    if (navigator.clipboard && navigator.clipboard.readText) {
      const text = await navigator.clipboard.readText();
      clipboardContent.value = text;

      await popup.show({
        title: '📋 Contenido del Portapapeles',
        message: text || '(vacío)',
        buttons: [{ id: 'ok', type: 'default', text: 'OK' }],
      });
    } else {
      await popup.show({
        title: '⚠️ No disponible',
        message: 'La lectura del portapapeles no está disponible en este navegador o plataforma.',
        buttons: [{ id: 'ok', type: 'default', text: 'OK' }],
      });
    }
  } catch (error) {
    console.error('Error al leer el portapapeles:', error);
    await popup.show({
      title: '⚠️ Permiso denegado',
      message: 'No se pudo leer el portapapeles. Es posible que necesites dar permisos explícitos.',
      buttons: [{ id: 'ok', type: 'default', text: 'OK' }],
    });
  } finally {
    isReading.value = false;
  }
}
</script>

<template>
  <AppPage
    title="MorterueloCon 2026"
    :back="false"
  >
    <div class="home-container">
      <div class="hero-section">
        <h2>🎯 Demo de Seguridad</h2>
        <p class="subtitle">Manipulación de Portapapeles en Mini Apps de Telegram</p>
        <p class="description">
          Esta demo muestra cómo un atacante podría usar ingeniería social combinada con
          manipulación del portapapeles para comprometer sistemas de escritorio.
        </p>
      </div>

      <div
        class="main-demo"
        v-if="clipboardDemo"
      >
        <AppLink
          class="demo-button-link"
          :to="{ name: clipboardDemo.name }"
        >
          <div class="demo-button-content">
            <span class="demo-icon">🔒</span>
            <div class="demo-text">
              <h3>Ver Demo Interactiva</h3>
              <p>Haz clic para ver cómo funciona el ataque</p>
            </div>
            <span class="arrow">→</span>
          </div>
        </AppLink>
      </div>

      <div class="clipboard-reader">
        <h3>🔍 Comprobar Portapapeles</h3>
        <p class="reader-description">
          Haz clic en el botón para ver qué hay actualmente en tu portapapeles.
          Esta funcionalidad demuestra cómo las aplicaciones pueden leer el portapapeles.
        </p>
        <button
          class="read-button"
          @click="readClipboard"
          :disabled="isReading"
        >
          <span v-if="isReading">⏳ Leyendo...</span>
          <span v-else>📋 Leer Portapapeles</span>
        </button>
      </div>

      <div class="info-section">
        <div class="info-card">
          <h3>⚠️ ¿Por qué es peligroso?</h3>
          <ul>
            <li>No requiere vulnerabilidades técnicas</li>
            <li>Aprovecha la confianza del usuario</li>
            <li>Difícil de detectar sin revisar</li>
            <li>Funciona en cualquier plataforma desktop</li>
          </ul>
        </div>

        <div class="info-card">
          <h3>🛡️ Cómo protegerse</h3>
          <ul>
            <li>Siempre revisa antes de pegar en terminal</li>
            <li>Desconfía de "validaciones" mediante comandos</li>
            <li>Usa herramientas que muestren el portapapeles</li>
            <li>Educa a tus usuarios sobre estos riesgos</li>
          </ul>
        </div>
      </div>

      <div class="footer-info">
        <p>🛡️ MorterueloCon 2026 - Demostración Educativa</p>
      </div>
    </div>
  </AppPage>
</template>

<style scoped>
.home-container {
  max-width: 600px;
  margin: 0 auto;
}

.hero-section {
  text-align: center;
  margin-bottom: 32px;
}

.hero-section h2 {
  font-size: 28px;
  margin: 0 0 8px 0;
  color: var(--tg-theme-text-color, #000);
}

.subtitle {
  font-size: 16px;
  font-weight: 600;
  color: var(--tg-theme-link-color, #3390ec);
  margin: 0 0 16px 0;
}

.description {
  font-size: 14px;
  line-height: 1.6;
  color: var(--tg-theme-text-color, #000);
  opacity: 0.8;
  margin: 0;
}

.main-demo {
  margin-bottom: 40px;
}

.demo-button-link {
  text-decoration: none;
  display: block;
}

.demo-button-content {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 24px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(102, 126, 234, 0.3);
  transition: all 0.3s ease;
  cursor: pointer;
}

.demo-button-content:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 32px rgba(102, 126, 234, 0.4);
}

.demo-icon {
  font-size: 48px;
  flex-shrink: 0;
}

.demo-text {
  flex-grow: 1;
  text-align: left;
}

.demo-text h3 {
  margin: 0 0 4px 0;
  font-size: 20px;
  color: white;
}

.demo-text p {
  margin: 0;
  font-size: 14px;
  color: rgba(255, 255, 255, 0.9);
}

.arrow {
  font-size: 32px;
  color: white;
  flex-shrink: 0;
}

.clipboard-reader {
  background: var(--tg-theme-bg-color, #fff);
  padding: 24px;
  border-radius: 12px;
  border: 2px solid var(--tg-theme-hint-color, #999);
  margin-bottom: 32px;
}

.clipboard-reader h3 {
  margin: 0 0 12px 0;
  font-size: 18px;
  color: var(--tg-theme-text-color, #000);
}

.reader-description {
  font-size: 14px;
  line-height: 1.5;
  color: var(--tg-theme-text-color, #000);
  opacity: 0.8;
  margin: 0 0 16px 0;
}

.read-button {
  width: 100%;
  padding: 14px;
  font-size: 16px;
  font-weight: 600;
  color: var(--tg-theme-button-text-color, white);
  background: var(--tg-theme-button-color, #3390ec);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.read-button:hover:not(:disabled) {
  opacity: 0.9;
  transform: translateY(-1px);
}

.read-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.info-section {
  display: grid;
  gap: 20px;
  margin-top: 40px;
}

.info-card {
  background: var(--tg-theme-secondary-bg-color, #f5f5f5);
  padding: 24px;
  border-radius: 12px;
  border: 1px solid var(--tg-theme-section-separator-color, #e0e0e0);
}

.info-card h3 {
  margin: 0 0 16px 0;
  font-size: 18px;
  color: var(--tg-theme-text-color, #000);
}

.info-card ul {
  margin: 0;
  padding-left: 20px;
  list-style: none;
}

.info-card ul li {
  position: relative;
  padding-left: 8px;
  margin: 10px 0;
  line-height: 1.5;
  color: var(--tg-theme-text-color, #000);
}

.info-card ul li::before {
  content: '•';
  position: absolute;
  left: -12px;
  color: var(--tg-theme-link-color, #3390ec);
  font-weight: bold;
}

.footer-info {
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid var(--tg-theme-section-separator-color, #e0e0e0);
  text-align: center;
}

.footer-info p {
  margin: 0;
  font-size: 14px;
  color: var(--tg-theme-hint-color, #999);
}
</style>