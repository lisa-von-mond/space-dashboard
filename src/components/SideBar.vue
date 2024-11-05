<script setup>

import { ref, reactive } from 'vue'

defineProps({})

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
      <a v-if="el.submenu === false">{{ el.name }}</a>
      <div v-else>
        <button 
          class="sidebar__button"
          @click="handleMenu(idx)"
          :class="{'sidebar__button--open': state.submenu === idx}">
            {{ el.name }}
        </button>
        <div 
          class="sidebar__submenu"
          v-if="state.submenu === idx">
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

.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  background: $color-secondary--dark;
  color: $color-light;
  z-index: 100;
  @include flex(column, center, center);

  @media (min-width: $screen-small) {
    @include flex(column, flex-start, flex-start);
    padding-top: 3rem;
    padding-left: 3rem;
    position: relative;
    height: 100%;
    width: 12rem;
    border-radius: 0 0.8rem 0.8rem 0;
  }

  ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
    @include flex(column, center, center);

    @media (min-width: $screen-small) {
      @include flex(column, flex-start, flex-start);
    }
  }

  li {

    a, button {
      color: $color-light;
      min-height: 2rem;
      margin: 0;
      @include flex(row, center, center);
      height: 100%;
      text-transform: uppercase;
      font-weight: 600;
      cursor: pointer;
    }

    .sidebar__button {
      padding-left: 0;

      &:after {
        content: '▾';
        margin-left: 0.5rem;
        font-size: 1.6rem;
        transition: transform 0.3s;
        transform: translateY(-0.1rem);
      }

      &--open:after {
        transform: rotate(180deg) translateY(-0.1rem);
      }
    }
  }

  &__submenu {
    @include flex(column, center, center);
    @media (min-width: $screen-small) {
      @include flex(column, flex-start, flex-start);
      padding-left: 1rem;
    }    
  }

  /* hover effects: */

  button, a {
    transition: all 0.3s;
    &:hover {
      font-weight: 800;
    }
  }
  
  a:hover {
    transform: translateX(0.2rem);
    font-weight: 800;
  }
}

</style>
