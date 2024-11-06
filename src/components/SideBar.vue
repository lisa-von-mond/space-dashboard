<script setup>

import { reactive } from 'vue'

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

</script>

<template>

<div id="sidebar" class="sidebar">
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
  @include flex(column, center, center);
  background: $color-secondary--dark;
  color: $color-light;
  z-index: 100;

  @media (min-width: $screen-small) {
    position: relative;
    @include flex(column, space-between, flex-start);
    height: 100%;
    width: 12rem;
    padding-top: 3rem;
    padding-left: 3rem;
  }

  /* menu list and elements */

  ul {
    @include flex(column, center, center);
    padding: 0;
    margin: 0;
    list-style-type: none;

    @media (min-width: $screen-small) {
      @include flex(column, flex-start, flex-start);
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
