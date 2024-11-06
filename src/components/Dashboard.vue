<script setup>

import { ref, reactive } from 'vue'
import { useMediaQuery } from '@vueuse/core'

import SideBar from './SideBar.vue';

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
        <div class="dashboard__widget dashboard__widget-first">
          first widget
        </div>
        <div class="dashboard__widget dashboard__widget-second">
          second widget
        </div>
        <div class="dashboard__widget dashboard__widget-third">
          third widget
        </div>
        <div class="dashboard__widget dashboard__widget-fourth">
          fourth widget
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
  min-height: calc(100vh - ($standard-gap * 2));
  margin: auto;
  overflow: hidden;
  border-radius: 0.8rem;

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
    padding: 0.6rem 1.4rem;
    background: $color-secondary;
  }

  /* grid */

  &__widget-area {
    background: $color-secondary;
    padding: 1rem;
    flex-grow: 1;

    display: grid;
    grid-template-rows: 1fr 300px 1fr 1fr;;
    grid-template-columns: 1fr ;
    grid-template-areas: 
    "widget-first"
    "widget-second"
    "widget-third"
    "widget-fourth";
  }

  &__widget {
    border: 0.2rem solid $color-primary;
    border-radius: 0.8rem;
    font-size: 2rem;
    text-transform: uppercase;

    &-first {
      grid-area: widget-first;
      color: hotpink !important;
    }
    &-second {
      grid-area: widget-second;
      color: skyblue;
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
      right: 1.4rem;
      top: 1.4rem;
      @include standard-shadow(0.3rem, 0.3rem, 4rem);
      background: $color-secondary--dark;
      z-index: 200;

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
.opacity-enter-active, .opacity-leave-active‚ {
  transition: opacity 0.6s;
}

</style>
