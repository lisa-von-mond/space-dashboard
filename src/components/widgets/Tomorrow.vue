<script setup>

import { computed, reactive, onMounted } from 'vue'

</script>

<template>

<div id="tomorrow" class="tomorrow">
  <h3>Don't like today?</h3>
  <div class="tomorrow__content">
    <p>what about tomorrow?</p>
  </div>

</div>


</template>

<style lang='scss' scoped>

@import '../../vars.scss';
@import '../../mixins.scss';

.tomorrow {
  @include flex(column, space-between, space-between, 1rem);

  &__content {
    @include flex(row, center, center, 1rem);
    flex-grow: 1;
    @media (min-width: $screen-small) {
      gap: 2rem
    }
    p {
      font-size: 0.8rem;
    }
  }
}

</style>
