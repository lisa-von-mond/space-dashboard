<script setup>

import { reactive, onMounted } from 'vue'
import axios from 'axios'

const state = reactive({
  airQualityData: null
})

onMounted(() => {
// fetch data from weather api
axios.get('http://api.weatherapi.com/v1/current.json?key=efb8776062704d21bcc124921240611&q=Berlin&aqi=yes')
  .then(response => {
    console.log(response.data)
    if(response) {
        // push only air quality data into data array for widget component
        state.airQualityData = response.data.current.air_quality
      }
    })
    .catch(error => {console.log(error)})
})

</script>

<template>

  <div id="airquality" class="airquality" v-if="state.airQualityData !== null">
    <h3>Air Quality Berlin</h3>
    <div class="airquality__content">

      <div class="airquality__section airquality__section-co2">
         <p>TEXT</p>
        <hr>
        <p>TEXT</p>
        <!---p> {{ state.weatherData.feelslike_c }}°</p--->
      </div>

    </div>
  </div>

</template>

<style lang='scss' scoped>

@import '../../vars.scss';
@import '../../mixins.scss';

.airquality {
  @include flex(column, space-between, space-between, 1rem);

  h3 {
    margin: 0;
  }

  &__content {
    @include flex(row, space-between, center, 0.4rem);
    flex-grow: 1;
  }

  &__section-co2 {
    p {
      font-size: 1.4rem;
      margin: 0;
    }
    .smaller-text {
      font-size: 0.6rem;
    }
    hr {
      border-top: 1.6px solid $color-light;
    }
  }

}

</style>
