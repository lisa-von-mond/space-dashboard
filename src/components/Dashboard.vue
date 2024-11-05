<script setup>

import { ref, reactive } from 'vue'


defineProps({
  name: String,
})

const state = reactive({
  sidebarOpen: false,
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
        <div class="widget-dummy"></div>
        <div class="widget-dummy"></div>
        <div class="widget-dummy"></div>
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

  <!---p class="disclaimer">Disclaimer and info text style</p--->

</template>

<style lang='scss' scoped>

@import '../vars.scss';

.dashboard {
  width: 100%;
  min-height: 100vh;
  margin: auto;

  &__content {
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    gap: $standard-gap;
  }

  &__header {
    height: 4rem;
    background: $color-secondary;
    padding: 0.6rem 1.2rem;
    display: flex;
    gap: 0.6rem;
    align-items: center;
    border-radius: 0.8rem 0.8rem 0 0;
  }

  &__widget-area {
    background: $color-secondary;
    padding: 1rem;
    border-radius: 0 0 0.8rem 0.8rem;

    .widget-dummy {
      height: 18rem;
      width: 18rem;
      border: 0.2rem solid $color-primary;
      border-radius: 0.8rem;
      margin: 1rem auto;
    }
  }
  &__navigation {
    button {
      position: fixed;
      right: 1.6rem;
      top: 1.8rem;
    }
  }
}
.logo {
  height: 2rem;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em $color-primary);
}

.disclaimer {
  color: $text-color--light;
}
</style>
