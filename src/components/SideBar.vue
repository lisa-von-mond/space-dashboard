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
        <button @click="handleMenu(idx)">{{ el.name }}</button>
        <div v-if="state.submenu === idx">
          <p v-for="sub in el.submenu">{{ sub }}</p>
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
    position: relative;
    height: 100%;
    width: 14rem;
    border-radius: 0 0.8rem 0.8rem 0;
  }

  ul {
    list-style-type: none;
    padding: 0;
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
    }

    button {
      padding-left: 0;
    }

    button:after {
      content: '▾';
    }
  }
}

</style>
