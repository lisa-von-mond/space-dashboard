<script setup>

import { ref, reactive, onMounted } from 'vue'
import { useMediaQuery } from '@vueuse/core'

import SideBar from './SideBar.vue';
import Weather from './widgets/Weather.vue';
import AirQuality from './widgets/AirQuality.vue';

const smallerDevice = useMediaQuery('(max-width: 768px)')

const props = defineProps({
  name: String,
})

const state = reactive({
  sidebarOpen: false,
})

// const count = ref(0)

</script>

<template>
  <div id="dashboard" class="dashboard">
    <div class="dashboard__content">
      <section class="dashboard__header">
        <h1>Today is the day!</h1>
      </section>
      <section class="dashboard__widget-area">
        <Weather 
          class="dashboard__widget dashboard__widget-first" />
        <AirQuality 
          class="dashboard__widget dashboard__widget-second" />

        <div class="dashboard__widget dashboard__widget-third">
          <h3>third widget</h3>
        </div>
        <div class="dashboard__widget dashboard__widget-fourth">
          <h3>fourth widget</h3>
        </div>
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

<style lang='scss' scoped>

@import '../vars.scss';
@import '../mixins.scss';

/* general dashboard sizing and layout: */

.dashboard {
  @include flex(column, space-between, space-between, $standard-gap);
  width: 100%;
  min-height: 100vh;
  margin: auto;
  overflow: hidden;

  @media (min-width: $screen-xsmall) {
    min-height: calc(100vh - ($standard-gap * 2));
    border-radius: 0.8rem;
  }

  @media (min-width: $screen-small) {
    @include flex(row, center, space-between, $standard-gap);
    max-width: 1080px;
    width: 90vw;
    height: 90vh;
    min-height: auto;
  }

  @media (min-width: $screen-medium) {
    width: 80vw;
    height: 80vh;
  }

  /* left / permanently visible part of dashboard: */

  &__content {
    @include flex(column, space-between, space-between, $standard-gap);
    flex-grow: 1;
  }

  &__header {
    @include flex(row, flex-start, center, 1.3rem);
    height: 4rem;
    padding: 0.6rem 1rem;
    @include gradient-nice($color-secondary, $color-secondary--dark, 135deg);
    @media (min-width: $screen-xsmall) {
      padding: 0.6rem 1.6rem;
    }
    @media (min-width: $screen-small) {
      padding: 0.6rem 2.2rem;
    }
  }

  /* widget-area */

  &__widget-area {
    @include gradient-nice($color-secondary, $color-secondary--dark, 45deg);
    padding: $standard-gap;;
    flex-grow: 1;

    /* grid */

    display: grid;
    grid-template-rows: auto auto auto auto;
    grid-template-columns: 1fr;
    grid-template-areas: 
    "widget-first"
    "widget-second"
    "widget-third"
    "widget-fourth";
    column-gap: $standard-gap;
    row-gap: $standard-gap;

    @media (min-width: $screen-xsmall) {
      column-gap: 1rem;
      row-gap: 1rem;
      padding: 1rem;
    }

    @media (min-width: $screen-small) {
      padding: 2rem;
    }

    @media (min-width: $screen-xsmall) and (max-width: $screen-small-max), (min-width: $screen-medium) {
      grid-template-rows: 1fr 1fr;
      grid-template-columns: 1fr 1fr;
      grid-template-areas: 
      "widget-first widget-second"
      "widget-third widget-fourth";
    }
  }

  &__widget {
    border: 0.1rem solid $color-light;
    border-radius: 0.8rem;
    font-size: 0.8rem;
    padding: 1rem;
    text-transform: uppercase;

    &-first {
      grid-area: widget-first;
    }
    &-second {
      grid-area: widget-second;
    }
    &-third {
      grid-area: widget-third;
      color: goldenrod;
    }
    &-fourth {
      grid-area: widget-fourth;
      color: cyan;
    }
  }

  /* navigation: */

  &__nav {
    position: absolute;

    @media (min-width: $screen-small) {
      position: relative;
    }

    .dashboard__menu-button {
      position: fixed;
      height: 3.2rem;
      width: 3.2rem;
      right: 0.8rem;
      top: 1rem;
      @include gradient-nice($color-secondary--dark, $color-secondary--darker, 40deg);
      z-index: 200;

      @media (min-width: $screen-xsmall) {
        top: 1.4rem;
        right: 1.6rem;
      }

      span {
        position: absolute;
        content: '';
        width: 2.1rem;
        height: 0.3rem;
        left: 0.5rem;
        background: $color-light;
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
    color: $text-color--light;
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
