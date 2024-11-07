<script setup>

import { reactive, computed } from 'vue'

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
    <li v-for="el,idx in menuData">
      <!--- menu elements without submenu: -->
      <a v-if="el.submenu === false">{{ el.name }}</a>
      <!--- menu elements with submenu: -->
      <div v-else>
        <button 
          class="sidebar__menu-button"
          @click="handleMenu(idx)"
          :class="{'sidebar__menu-button--open': state.submenu === idx}">
            <span>{{ el.name }}</span>
        </button>
        <div 
          class="sidebar__submenu"
          :style="{'height': state.submenu === idx ? `${el.submenu.length * 2}rem` : '0rem'}">
              <a v-for="sub, index in el.submenu" :key="index">{{ sub }}</a>
        </div>
      </div>
    </li>
  </ul>
</div>

</template>

<style lang='scss' scoped>

@use '../vars.scss' as v;
@use '../mixins.scss' as m;

/* sidebar general layout: */

.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  @include m.flex(column, center, center, v.$gap-large);
  @include m.gradient-nice(v.$color-secondary--light, v.$color-secondary, 45deg);
  color: v.$color-light;
  z-index: 300;

  @media (min-width: v.$screen-small) {
    position: relative;
    @include m.flex(column, flex-start, flex-start, v.$gap-large);
    height: 100%;
    width: 12rem;
    padding: v.$gap-large;
  }

  /* upper part of sidebar: */

  &__intro {
    @include m.flex(column, center, center, 0.4rem);
    width: 100%;

    hr {
      width: 100%;
      max-width: 6rem;
      border-top: 0.3rem dotted v.$color-light;
      border-bottom: none;
      @media (min-width: v.$screen-small) {
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
      @include m.standard-shadow(0, 0, 2rem);
    }
  }

  /* menu list and elements: */

  ul {
    @include m.flex(column, center, center);
    padding: 0;
    margin: 0;
    list-style-type: none;

    @media (min-width: v.$screen-small) {
      @include m.flex(column, flex-start, flex-start);
      margin-left: 1.6rem;
    }
  }

  a, button {
    @include m.flex(row, center, center);
    height: 100%;
    min-height: 2rem;
    margin: 0;
    color: v.$color-light;
    font-weight: 400;
    text-transform: lowercase;
    letter-spacing: 0.1rem;
    cursor: pointer;

  /* hover effects: */

    transition: all 0.3s;
    &:hover {
      font-weight: 600;
    }
  }

  a:hover {
    transform: translateX(0.2rem);
    font-weight: 600;
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

    span {
      position: relative;
      
      &:before {
        content: '';
        border-bottom: 0.2rem dotted v.$color-accent;
        position: absolute;
        left: 50%;
        bottom: -0.3rem;
        width: 0%;
        transform: translateX(-50%);
        transition: width 0.3s;
      }
    }

    &--open {
      span:before {
        width: 100%;
      }

      &:after {
        transform: rotate(180deg) translateY(-0.2rem);
        text-decoration: none !important;
      }
    }
  }

  /* submenu: */

  &__submenu {
    
    @include m.flex(column, center, center);
    overflow: hidden;
    transition: height 0.5s;

    @media (min-width: v.$screen-small) {
      @include m.flex(column, flex-start, flex-start);
      padding-left: 1rem;
    }
  }
}

</style>
