<script setup>

import { ref, reactive } from 'vue'


defineProps({
  name: String,
})

const state = reactive({
  sidebarOpen: true
})

const count = ref(0)
</script>

<template>
  <div id="dashboard" class="dashboard">
    <div class="dashboard__content">
      <section class="dashboard__header">
        <img src="../assets/icon_planet.svg" class="logo" alt="Planet logo" />
        <h1>Hi {{ name }}</h1>
      </section>
      <section class="dashboard__widget-area">
        widgets will go here
      </section>
    </div>
    <nav class="dashboard__navigation">
      <button>open / close</button>
      <div 
        class="sidebar"
        v-if="state.sidebarOpen">
          sidebar content will go here
      </div>
    </nav>
  </div>

  <!---div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div--->

  <p class="disclaimer">Disclaimer and info text style</p>

</template>

<style lang='scss' scoped>

@import '../vars.scss';

.dashboard {
  width: 100%;
  height: 100%;

  &__header {
    background: pink;
  }
  &__widget-area {
    background: skyblue;
  }
  &__navigation {
    background: gold;
  }
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
.disclaimer {
  color: $text-color--light;
}
</style>
