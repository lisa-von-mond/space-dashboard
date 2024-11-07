<script setup>

import { ref, watch } from 'vue'

const emit = defineEmits(['switchday'])

const isTomorrow = ref(null)

watch(isTomorrow, () => {
    emit('switchday')
})

</script>

<template>

<div id="tomorrow" class="tomorrow">
  <h3>Don't like this day?</h3>
  <div class="tomorrow__content">
    <p>What&nbsp;about<br>{{ isTomorrow == true ? 'today' : 'tomorrow' }} ?</p>
    <label class="switch">
      <input type="checkbox" v-model="isTomorrow" :checked="true">
      <span class="slider"></span>
    </label>
  </div>

</div>


</template>

<style lang='scss' scoped>

@import '../../vars.scss';
@import '../../mixins.scss';

.tomorrow {
  @include flex(column, space-between, space-between, 1rem);

  &__content {
    @include flex(column, center, center, 2rem);
    flex-grow: 1;
    @media (min-width: $screen-small) {
      gap: 2rem
    }
    p {
      font-size: 1rem;
      letter-spacing: 0.3rem;
      max-height: 2rem;
    }
  }
}

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: $color-light;
  -webkit-transition: .4s;
  transition: .4s;
  border-radius: 34px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: $color-secondary;;
  -webkit-transition: .4s;
  transition: .4s;
  border-radius: 50%;
}

input:checked + .slider {
  background-color: $color-light;
}

input:focus + .slider {
  box-shadow: 0 0 1px $color-light;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
  background-color: $color-secondary--dark;
}

</style>
