<script setup>

import { computed, reactive, onMounted } from 'vue'

</script>

<template>

<div id="tomorrow" class="tomorrow">
  <h3>Don't like today?</h3>
  <div class="tomorrow__content">
    <p>what about tomorrow?</p>
    <label class="switch">
      <input type="checkbox">
      <span class="slider round"></span>
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
    @include flex(column, center, center, 1rem);
    flex-grow: 1;
    @media (min-width: $screen-small) {
      gap: 2rem
    }
    p {
      font-size: 0.8rem;
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
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}

</style>
