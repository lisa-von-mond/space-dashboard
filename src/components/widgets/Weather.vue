<script setup>

import { computed, reactive, onMounted } from 'vue'
import axios from 'axios'

const state = reactive({
  weatherData: null
})

const windDirection = computed(() => {
  let windDirection = state.weatherData.wind_dir

  const translator = [
    {letter: 'N', word: 'north'},
    {letter: 'S', word: 'south'},
    {letter: 'W', word: 'west'},
    {letter: 'E', word: 'east'}
  ]

  for (let i = 0; i < translator.length; i++) {
    windDirection = windDirection.replace(translator[i].letter, translator[i].word);
  }

  return windDirection
})

onMounted(() => {

// fetch data from weather api
axios.get('http://api.weatherapi.com/v1/current.json?key=efb8776062704d21bcc124921240611&q=Berlin&aqi=yes')
  .then(response => {
    console.log(response.data)
    if(response) {

      // parameters needed for weather widget:
      const params = ['condition', 'temp_c', 'feelslike_c', 'wind_kph', 'wind_dir']
      let responseData = response.data.current
      // console.log(responseData)

      let weatherData = {}
      // push only relevant weather data into data array for widget component
        params.forEach(el => weatherData[el] = responseData[el])
        state.weatherData = weatherData
        console.log(weatherData)
        // push only air quality data into data array for widget component
        // state.airQualityData = responseData.air_quality

      }
    })
    .catch(error => {console.log(error)})
})

</script>

<template>
  <div id="weather" class="weather" v-if="state.weatherData !== null">
    <h3>Wetter Berlin</h3>
      <div class="weather__content">
      <div class="weather__section weather__section-warmth">
        <p>{{ state.weatherData.temp_c }}°</p>
        <hr>
        <p> {{ state.weatherData.feelslike_c }}°</p>
      </div>
      <div class="weather__section weather__section-condition">
        <!---p>{{ state.weatherData.condition.text }}</p--->
        <img :src="state.weatherData.condition.icon"/>
      </div>
      <div class="weather__section weather__section-wind">
        <p>{{ state.weatherData.wind_kph }} <span class="smaller-text">km/h</span></p>
        <hr>
        <p>↑ {{ windDirection }}</p>
      </div>
    </div>
  </div>
</template>

<style lang='scss' scoped>

@import '../../vars.scss';
@import '../../mixins.scss';

.weather {
  @include flex(column, space-between, space-between, 1rem);

  &__content {
    @include flex(row, space-between, center, 0.4rem);
    flex-grow: 1;
  }

  h3 {
    margin: 0;
  }

  &__section-warmth, &__section-wind {
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

  &__section-condition {
    img {
      height: 5rem;
    }
  }
}


</style>
