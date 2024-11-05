<script setup>

import { ref, reactive } from 'vue'
import { useMediaQuery } from '@vueuse/core'

import SideBar from './SideBar.vue';

const smallerDevice = useMediaQuery('(max-width: 768px)')

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
        <!---div class="widget-dummy"></div>
        <div class="widget-dummy"></div>
        <div class="widget-dummy"></div--->
      </section>
    </div>
    <nav class="dashboard__navigation">
      <button 
        v-if="smallerDevice"
        class="dashboard__menu-button"
        @click="state.sidebarOpen = !state.sidebarOpen">
        <span>x</span>
        <span></span>
      </button>
      <SideBar v-if="!smallerDevice || state.sidebarOpen"/>
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
@import '../mixins.scss';

.dashboard {
  width: 100%;
  min-height: calc(100vh - ($standard-gap * 2));
  margin: auto;
  @include flex(column, space-between, space-between, $standard-gap);

  @media (min-width: $screen-small) {
    @include flex(row, center, space-between, $standard-gap);
    max-width: 1080px;
    width: 90vw;
    height: 90vh;
    overflow: hidden;
    min-height: auto;
  }

  @media (min-width: $screen-medium) {
    width: 80vw;
    height: 80vh;
  }

  &__content {
    @include flex(column, space-between, space-between, $standard-gap);
    flex-grow: 1;
  }

  &__header {
    height: 4rem;
    background: $color-secondary;
    padding: 0.6rem 1.4rem;
    @include flex(row, flex-start, center, 1.3rem);
    border-radius: 0.8rem 0.8rem 0 0;

    @media (min-width: $screen-small) {
      border-radius: 0.8rem 0 0 0;
    }
  }

  &__widget-area {
    background: $color-secondary;
    padding: 1rem;
    border-radius: 0 0 0.8rem 0.8rem;
    flex-grow: 1;

    @media (min-width: $screen-small) {
      border-radius: 0 0 0 0.8rem;
    }

    .widget-dummy {
      height: 18rem;
      width: 18rem;
      border: 0.2rem solid $color-primary;
      border-radius: 0.8rem;
      margin: 1rem auto;
    }
  }

  &__navigation {
    position: absolute;

    @media (min-width: $screen-small) {
      position: relative;
    }

    .dashboard__menu-button {
      position: fixed;
      height: 3.2rem;
      width: 3.2rem;
      background: $color-secondary--dark;
      right: 1.4rem;
      top: 1.4rem;
      @include standard-shadow(0.3rem, 0.3rem, 4rem);
      z-index: 200;
    }
  }
}

.logo {
  height: 2rem;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  @include standard-shadow(0, 0, 2rem);
}

.disclaimer {
  color: $text-color--light;
}

</style>
