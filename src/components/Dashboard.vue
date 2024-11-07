<script setup>

import { reactive, ref } from 'vue'
import { useMediaQuery } from '@vueuse/core'

import SideBar from './SideBar.vue';
import Weather from './widgets/Weather.vue';
import AirQuality from './widgets/AirQuality.vue';
import Color from './widgets/Color.vue';
import Tomorrow from './widgets/Tomorrow.vue';

const smallerDevice = useMediaQuery('(max-width: 768px)')

const airKey = ref(0);
const weatherKey = ref(0);

function forceRerender() {
  airKey.value += 1;
  weatherKey.value += 1;
};

const props = defineProps({
  name: String,
})

const state = reactive({
  sidebarOpen: false,
  isTomorrow: false
})

function switchDay() {
  state.isTomorrow = !state.isTomorrow
  forceRerender()
}

// const count = ref(0)

</script>

<template>
  <div id="dashboard" class="dashboard">
    <div class="dashboard__content">
      <section class="dashboard__header">
        <h1>{{ state.isTomorrow ? 'Tomorrow' : 'Today' }} is the day!</h1>
      </section>
      <section class="dashboard__widget-area">
        <Weather 
          class="dashboard__widget dashboard__widget-weather" 
          :isTomorrow = state.isTomorrow 
          :key="weatherKey" />
        <Color 
          class="dashboard__widget dashboard__widget-color" 
          :isTomorrow = state.isTomorrow />
        <AirQuality 
          class="dashboard__widget dashboard__widget-air" 
          :isTomorrow = state.isTomorrow 
          :key="airKey"/>
        <Tomorrow 
          class="dashboard__widget dashboard__widget-tomorrow"
          @switchday="switchDay()"/>
      </section>
    </div>
    <nav class="dashboard__nav">
      <button 
        v-if="smallerDevice"
        class="dashboard__menu-button"
        @click="state.sidebarOpen = !state.sidebarOpen"
        :class="{'dashboard__menu-button--open': state.sidebarOpen}">
          <span></span>
          <span></span>
      </button>
      <Transition name="opacity">
        <SideBar 
          v-if="!smallerDevice || state.sidebarOpen"
          :name="props.name"/>
      </Transition>
    </nav>
  </div>

  <!---div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div--->

  <!---p class="dashboard__disclaimer">Disclaimer and info text style</p--->

</template>

<style lang='scss'>

@use '../vars.scss' as v;
@use '../mixins.scss' as m;

/* general dashboard sizing and layout: */

.dashboard {
  @include m.flex(column, space-between, space-between, v.$gap-small);
  width: 100%;
  min-height: 100vh;
  margin: auto;
  overflow: hidden;

  @media (min-width: v.$screen-xsmall) {
    min-height: calc(100vh - (v.$gap-small * 2));
    border-radius: 0.8rem;
  }

  @media (min-width: v.$screen-small) {
    @include m.flex(row, center, space-between, v.$gap-small);
    max-width: 1080px;
    width: 90vw;
    height: 90vh;
    min-height: 600px;
    max-height: 800px;
  }

  @media (min-width: v.$screen-medium) {
    width: 80vw;
    height: 80vh;
  }

  /* left / permanently visible part of dashboard: */

  &__content {
    @include m.flex(column, space-between, space-between, v.$gap-small);
    flex-grow: 1;
  }

  &__header {
    @include m.flex(row, flex-start, center, 1.3rem);
    height: 4rem;
    padding: 0.6rem 1rem;
    @include m.gradient-nice(v.$color-secondary, v.$color-secondary--dark, 135deg);
    @media (min-width: v.$screen-xsmall) {
      padding: 0.6rem 1.6rem;
    }
    @media (min-width: v.$screen-small) {
      padding: 0.6rem 2.2rem;
    }
  }

  /* widget-area */

  &__widget-area {
    @include m.gradient-nice(v.$color-secondary, v.$color-secondary--dark, 45deg);
    padding: v.$gap-small;
    flex-grow: 1;

    /* grid */

    display: grid;
    grid-template-rows: auto auto auto auto;
    grid-template-columns: 1fr;
    grid-template-areas: 
    "widget-weather"
    "widget-color"
    "widget-air"
    "widget-tomorrow";
    column-gap: v.$gap-small;
    row-gap: v.$gap-small;

    @media (min-width: v.$screen-xsmall) {
      column-gap: v.$gap-large;
      row-gap: v.$gap-large;
      padding: v.$gap-large;
    }

    @media (min-width: v.$screen-xsmall) {
      grid-template-rows: 1fr 1fr;
      grid-template-columns: 1fr 1fr;
      grid-template-areas: 
      "widget-weather widget-color"
      "widget-air widget-tomorrow";
    }
  }

  &__widget {
    border: 0.2rem dotted v.$color-accent;
    border-radius: 0.8rem;
    font-size: 0.8rem;
    padding: 1.4rem v.$gap-large;

    &-weather {
      grid-area: widget-weather;
    }
    &-color {
      grid-area: widget-color;
    }
    &-air {
      grid-area: widget-air;
    }
    &-tomorrow {
      grid-area: widget-tomorrow;
    }

    h3 {
      color: v.$color-accent;
      margin: 0;
    }
  }

  /* navigation: */

  &__nav {
    position: absolute;

    @media (min-width: v.$screen-small) {
      position: relative;
    }

    .dashboard__menu-button {
      position: fixed;
      height: 3.2rem;
      width: 3.2rem;
      right: 0.8rem;
      top: 1rem;
      @include m.gradient-nice(v.$color-secondary--dark, v.$color-secondary--darker, 40deg);
      z-index: 200;

      @media (min-width: v.$screen-xsmall) {
        top: 1.4rem;
        right: 1.6rem;
      }

      span {
        position: absolute;
        content: '';
        width: 2.1rem;
        height: 0.3rem;
        left: 0.5rem;
        background: v.$color-light;
        transform-origin: 50% 50%;
        transform: rotate(0deg);
        transition: all 0.6s;

        &:nth-child(1) {
          top: 1rem;
        }

        &:nth-child(2) {
          bottom: 1rem;
        }
      }

      /* hamburger animation */

      &--open {
        span:nth-child(1) {
          top: 1.45rem;
          transform: rotate(-135deg);
        }
        span:nth-child(2) {
          bottom: 1.45rem;
          transform: rotate(135deg);
        }
      }
    }
  }

  &__disclaimer {
    color: v.$text-color--light;
  }
}

/* transition classes: */

.opacity-enter-from, .opacity-leave-to {
  opacity: 0;
}
.opacity-leave-from, .opacity-enter-to {
  opacity: 1;
}
.opacity-enter-active, .opacity-leave-active {
  transition: opacity 0.6s;
}

</style>
