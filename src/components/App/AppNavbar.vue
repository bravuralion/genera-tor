<template>
  <nav class="app-navbar">
    <div class="navbar-brand">
      <img src="/favicon.ico" alt="generator logo" width="30" />
      <b>
        Genera<span class="text--accent">TOR</span> <sup class="text--grayed">v{{ version }}</sup>
      </b>
    </div>

    <div class="navbar-actions">
      <button class="g-button action icon" @click="switchDarkMode">
        <LucideMoon :size="20" v-if="store.orderDarkMode" />
        <LucideSun :size="20" v-else />
      </button>

      <button class="g-button action icon" @click="switchLang">
        <LucideGlobe :size="20" />
        <span>{{ t(`locale.${store.currentAppLocale}`) }}</span>
      </button>
    </div>
  </nav>
</template>

<script setup lang="ts">
import { useI18n } from 'vue-i18n';
import { LucideGlobe, LucideMoon, LucideSun } from 'lucide-vue-next';
import { version } from '../../../package.json';
import { useStore } from '../../store/store';

const store = useStore();
const { t } = useI18n();

const availableLocales = ['pl', 'en', 'de'] as const;

function switchDarkMode() {
  store.orderDarkMode = !store.orderDarkMode;
  window.localStorage.setItem('dark-mode', `${store.orderDarkMode}`);
}

function switchLang() {
  const currentIndex = availableLocales.indexOf(
    store.currentAppLocale as (typeof availableLocales)[number]
  );
  const nextLocale = availableLocales[
    currentIndex === -1 ? 0 : (currentIndex + 1) % availableLocales.length
  ];

  store.changeLang(nextLocale);
}
</script>

<style lang="scss" scoped>
.app-navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;

  width: 100%;
  height: 40px;
  padding: 0.25em;
  background-color: #1c1c1c;
}

.navbar-brand {
  display: flex;
  align-items: center;
  gap: 0.5em;

  sup {
    font-size: 0.75em;
  }
}

.navbar-actions {
  display: flex;
  align-items: center;
  gap: 0.5em;

  button {
    padding: 0.5em;
    gap: 0.25em;
    border-radius: 0.5em;
  }
}
</style>
