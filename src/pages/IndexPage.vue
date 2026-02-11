<script setup lang="ts">
import { computed, ref } from 'vue';
import { routes } from '@/router';
import AppPage from '@/components/AppPage.vue';
import AppLink from '@/components/AppLink.vue';

const clipboardDemo = computed(() => routes.find((r) => r.name === 'clipboard-demo'));
const pastedContent = ref<string>('');

function handlePaste() {
  // El contenido ya estará en pastedContent gracias al v-model
  if (pastedContent.value.trim()) {
    // Opcional: podrías hacer algo con el contenido aquí
  }
}

function clearContent() {
  pastedContent.value = '';
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
          Después de hacer clic en "Validar mi Cuenta" en la demo, vuelve aquí y pega (Ctrl+V o Cmd+V)
          el contenido de tu portapapeles en el campo de abajo para comprobar que se ha inyectado el payload.
        </p>
        <textarea
          v-model="pastedContent"
          class="paste-area"
          placeholder="Pega aquí el contenido de tu portapapeles (Ctrl+V o Cmd+V)..."
          @paste="handlePaste"
        ></textarea>
        <div
          class="button-group"
          v-if="pastedContent"
        >
          <button
            class="clear-button"
            @click="clearContent"
          >
            🗑️ Limpiar
          </button>
        </div>
        <div
          v-if="pastedContent"
          class="result-box"
        >
          <p class="result-label">✅ Contenido detectado:</p>
          <code class="result-content">{{ pastedContent }}</code>
        </div>
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

.paste-area {
  width: 100%;
  min-height: 120px;
  padding: 12px;
  font-family: 'Courier New', monospace;
  font-size: 14px;
  color: var(--tg-theme-text-color, #000);
  background: var(--tg-theme-bg-color, #fff);
  border: 2px solid var(--tg-theme-section-separator-color, #e0e0e0);
  border-radius: 8px;
  resize: vertical;
  transition: border-color 0.2s;
}

.paste-area:focus {
  outline: none;
  border-color: var(--tg-theme-button-color, #3390ec);
}

.paste-area::placeholder {
  color: var(--tg-theme-hint-color, #999);
  font-style: italic;
}

.button-group {
  display: flex;
  gap: 10px;
  margin-top: 12px;
}

.clear-button {
  padding: 10px 16px;
  font-size: 14px;
  font-weight: 600;
  color: var(--tg-theme-destructive-text-color, #ff3b30);
  background: transparent;
  border: 2px solid var(--tg-theme-destructive-text-color, #ff3b30);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.clear-button:hover {
  background: var(--tg-theme-destructive-text-color, #ff3b30);
  color: white;
}

.result-box {
  margin-top: 16px;
  padding: 16px;
  background: #e8f5e9;
  border: 2px solid #4caf50;
  border-radius: 8px;
}

.result-label {
  margin: 0 0 8px 0;
  font-weight: 600;
  color: #2e7d32;
  font-size: 14px;
}

.result-content {
  display: block;
  padding: 12px;
  background: #fff;
  border-radius: 6px;
  font-family: 'Courier New', monospace;
  font-size: 13px;
  color: #000;
  word-break: break-all;
  white-space: pre-wrap;
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