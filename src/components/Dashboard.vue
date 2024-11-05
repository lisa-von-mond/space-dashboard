<script setup>
import { ref } from 'vue'

defineProps({
  name: String,
})

const count = ref(0)
</script>

<template>
  <div id="dashboard" class="dashboard">
    <section class="dashboard__header">
      <img src="../assets/icon_planet.svg" class="logo" alt="Planet logo" />
      <h1>Hi {{ name }}</h1>
    </section>
    <section class="dashboard__widgets">
      widgets will go here
    </section>
  </div>

  <!---div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div--->

  <!---p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>

  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p--->

</template>

<style scoped>

.dashboard {
  width: 100%;
  height: 100%
}
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
.read-the-docs {
  color: #888;
}
</style>
