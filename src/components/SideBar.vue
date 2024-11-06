<script setup>

import { onMounted, reactive, computed } from 'vue'

const props = defineProps({
  name: String,
})

const menuData = [
  {name: "my account", submenu: false},
  {name: "my profile", submenu: ["view", "edit"]},
  {name: "settings", submenu: ["widgets", "alarms", "notifications"] },
  {name: "discover", submenu: ["about", "contribute"]},
  {name: "help", submenu: false},
]

const state = reactive({
  submenu: null,
})

function handleMenu(el) {
  if (state.submenu === el) {
    state.submenu = null
  } else {
    state.submenu = el
  }
}

const hello = computed(() => {
  var thisDate = new Date()
  var hour = thisDate.getHours()
  if (hour < 6 || hour >= 23) {
    return "Good Evening"
  } else if (hour >= 6 && hour < 10) {
    return "Good Morning"
  } else if (hour >= 10 && hour < 18) {
    return "Good Day"
  } else {
    return "Good Evening"
  }
})

</script>

<template>

<div id="sidebar" class="sidebar">
  <div class="sidebar__intro">
    <img src="../assets/icon_planet.svg" class="sidebar__logo" alt="Planet logo" />
    <hr/>
    <h2>{{ hello }}<br>{{ name }}</h2>
    <hr/>
  </div>
  <ul>
    <li v-for="(el,idx) in menuData">
      <!--- menu elements without submenu: -->
      <a v-if="el.submenu === false">{{ el.name }}</a>
      <!--- menu elements with submenu: -->
      <div v-else>
        <button 
          class="sidebar__menu-button"
          @click="handleMenu(idx)"
          :class="{'sidebar__menu-button--open': state.submenu === idx}">
            {{ el.name }}
        </button>
        <div 
          class="sidebar__submenu"
          :style="{'height': state.submenu === idx ? `${el.submenu.length * 2}rem` : '0rem'}">
              <a v-for="sub in el.submenu">{{ sub }}</a>
        </div>
      </div>
    </li>
  </ul>
</div>

</template>

<style lang='scss' scoped>

@import '../vars.scss';
@import '../mixins.scss';

/* sidebar general layout */

.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  @include flex(column, center, center, 1rem);
  background: $color-secondary--dark;
  color: $color-light;
  z-index: 100;

  @media (min-width: $screen-small) {
    position: relative;
    @include flex(column, flex-start, flex-start, 1rem);
    height: 100%;
    width: 12rem;
    padding: 1rem;
  }

  /* upper part of sidebar */

  &__intro {
    @include flex(column, center, center, 0.4rem);
    width: 100%;

    hr {
      width: 100%;
      max-width: 6rem;
      border-top: 0.3rem dotted $color-light;
      border-bottom: none;
      @media (min-width: $screen-small) {
        max-width: 100%;
      }
    }

    h2 {
      padding: 0;
    }
  }

  &__logo {
    height: 3rem;
    margin: 1.4rem auto 1.2rem auto;
    will-change: filter;
    transition: filter 300ms;
    &:hover {
      @include standard-shadow(0, 0, 2rem);
    }
  }

  /* menu list and elements */

  ul {
    @include flex(column, center, center);
    padding: 0;
    margin: 0;
    list-style-type: none;

    @media (min-width: $screen-small) {
      @include flex(column, flex-start, flex-start);
      margin-left: 1.6rem;
    }
  }

  a, button {
    @include flex(row, center, center);
    height: 100%;
    min-height: 2rem;
    margin: 0;
    color: $color-light;
    font-weight: 600;
    text-transform: uppercase;
    cursor: pointer;

  /* hover effects: */

    transition: all 0.3s;
    &:hover {
      font-weight: 800;
    }
  }

  a:hover {
    transform: translateX(0.2rem);
    font-weight: 800;
  }

  /* */

  .sidebar__menu-button {
    padding-left: 0;

    &:after {
      content: '▾';
      margin-left: 0.5rem;
      font-size: 1.6rem;
      transform: translateY(-0.1rem);
      transition: transform 0.3s;
    }

    &--open:after {
      transform: rotate(180deg) translateY(-0.1rem);
    }
  }

  /* submenu */

  &__submenu {
    
    @include flex(column, center, center);
    overflow: hidden;
    transition: height 0.5s;

    @media (min-width: $screen-small) {
      @include flex(column, flex-start, flex-start);
      padding-left: 1rem;
    }
  }
}

</style>
