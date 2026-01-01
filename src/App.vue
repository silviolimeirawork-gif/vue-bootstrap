<script setup>
import HelloToast from './components/HelloToast.vue';
import HelloWorld from './components/HelloWorld.vue'
import PWABadge from './components/PWABadge.vue'
import Tooltips from './components/HelloTooltips.vue';
import HelloTooltips from './components/HelloTooltips.vue';
import HelloSpinners from './components/HelloSpinners.vue';
</script>

<template>
  <div>
    <a href="https://vite.dev" target="_blank">
      <img src="/favicon.svg" class="logo" alt="vite-project logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <!-- <HelloWorld msg="vite-project" /> -->
  <HelloSpinners />

  <PWABadge />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
