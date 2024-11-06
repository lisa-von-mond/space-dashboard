<script setup>

import { ref, reactive, onMounted } from 'vue'

const props = defineProps({
  data: Object,
})

</script>

<template>
  <div id="weather">
    <p>{{ data.temp_c }}</p>
    <p>{{ data.feelslike_c }}</p>
    <p>{{ data.wind_kph }}</p>
  
  </div>
</template>

<style lang='scss' scoped>

@import '../../vars.scss';
@import '../../mixins.scss';


</style>
