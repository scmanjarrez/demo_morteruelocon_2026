<script setup lang="ts">
import { computed } from 'vue';
import { routes } from '@/router';
import AppPage from '@/components/AppPage.vue';
import AppLink from '@/components/AppLink.vue';

const nonIndexRoutes = computed(() => routes.filter((r) => !!r.meta?.title));
const clipboardDemo = computed(() => routes.find((r) => r.name === 'clipboard-demo'));
const otherRoutes = computed(() => routes.filter((r) => !!r.meta?.title && r.name !== 'clipboard-demo'));
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

      <div class="additional-pages">
        <h3>Páginas Adicionales</h3>
        <ul class="index-page__links">
          <li
            v-for="route in otherRoutes"
            :key="route.name"
            class="index-page__link-item"
          >
            <AppLink
              class="index-page__link"
              :to="{ name: route.name }"
            >
              <i
                v-if="route.meta?.icon"
                class="index-page__link-icon"
              >
                <component :is="route.meta.icon" />
              </i>
              {{ route.meta!.title }}
            </AppLink>
          </li>
        </ul>
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

.additional-pages {
  margin-top: 40px;
  padding-top: 32px;
  border-top: 2px solid var(--tg-theme-section-separator-color, #e0e0e0);
}

.additional-pages h3 {
  font-size: 18px;
  margin: 0 0 16px 0;
  color: var(--tg-theme-text-color, #000);
}

.index-page__links {
  list-style: none;
  padding-left: 0;
}

.index-page__link {
  font-weight: bold;
  display: inline-flex;
  gap: 5px;
}

.index-page__link-item+.index-page__link-item {
  margin-top: 10px;
}

.index-page__link-icon {
  width: 20px;
  display: block;
}

.index-page__link-icon svg {
  display: block;
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