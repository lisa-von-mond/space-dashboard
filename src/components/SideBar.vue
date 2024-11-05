<script setup>

import { ref, reactive } from 'vue'

defineProps({

})

const state = reactive({

})

</script>

<template>

  <div id="sidebar" class="sidebar">
    menu content will go here
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
  background: gold;
  border-radius: 0.8rem;
  z-index: 100;
  @include flex(column, center, center);
}

</style>
