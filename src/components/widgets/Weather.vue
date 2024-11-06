<script setup>

import { computed, reactive, onMounted } from 'vue'
import axios from 'axios'

const state = reactive({
  data: null
})

/* computed prop to get angle for full word of wind direction (e.g. 'northeast' 
and angle for the arrow, corresponding to wind direction) */
const windDirection = computed(() => {
  let windDirection = state.data.wind_dir
  const windDirectionToArray = windDirection.split("")

  const translator = [
    {letter: 'N', word: 'north', angle: 0},
    {letter: 'E', word: 'east', angle: 90},
    {letter: 'S', word: 'south', angle: 180},
    {letter: 'W', word: 'west', angle: 270},
  ]

  /* loop through string and replace letters with full words */
  for (let i = 0; i < translator.length; i++) {
    windDirection = windDirection.replace(translator[i].letter, translator[i].word);
  }

  /* calculate angle by building sum and dividing by number of string elements */
  let angle = 0
  for (let i = 0; i < windDirectionToArray.length; i++) {
    let a = translator.find(el => el.letter == windDirectionToArray[i]).angle
    angle = angle + a
  }
  let finalAngle = angle/windDirectionToArray.length

  /* return both values as object to have simple access */
  return {direction: windDirection, angle: finalAngle}
})

onMounted(() => {

// fetch data from weather api
axios.get('http://api.weatherapi.com/v1/current.json?key=efb8776062704d21bcc124921240611&q=Berlin&aqi=yes')
  .then(response => {
    if(response) {

      // parameters needed for weather widget:
      const params = ['condition', 'temp_c', 'feelslike_c', 'wind_kph', 'wind_dir']
      let responseData = response.data.current

      let weatherData = {}
      // push only relevant weather data into data object for widget component
        params.forEach(el => weatherData[el] = responseData[el])
        state.data = weatherData
      }
    })
    .catch(error => {console.log(error)})
})

</script>

<template>

<div id="weather" class="weather" v-if="state.data !== null">
  <h3>Weather Berlin</h3>
  <div class="weather__content">

    <div class="weather__section weather__section-warmth">
      <span class="hover-area">
        <p>{{ state.data.temp_c }}°</p>
        <div class="tooltip">real temperature</div>
      </span>
      <hr>
      <span class="hover-area">
        <p> {{ state.data.feelslike_c }}°</p>
        <div class="tooltip">feels-like temperature</div>
      </span>
    </div>

    <div class="weather__section weather__section-condition">
      <span class="hover-area">
        <img :src="state.data.condition.icon"/>
        <div class="tooltip">{{ state.data.condition.text }}</div>
      </span>
    </div>

    <div class="weather__section weather__section-wind">
      <p>{{ state.data.wind_kph }} <span class="smaller-text">km/h</span></p>
      <hr>
      <span class="hover-area">
        <p :style="{'transform': `rotate(${windDirection.angle}deg)`}">↑</p>
        <div class="tooltip">{{ windDirection.direction }}</div>
      </span>
    </div>

  </div>
</div>
<div class="no-data" v-else>waiting for data...</div>

</template>

<style lang='scss' scoped>

@import '../../vars.scss';
@import '../../mixins.scss';

.weather {
  @include flex(column, space-between, space-between, 1rem);

  h3 {
    margin: 0;
  }

  &__content {
    @include flex(row, center, center, 1rem);
    flex-grow: 1;
    @media (min-width: $screen-small) {
      gap: 2rem
    }
  }

  &__section-warmth, &__section-wind {
    p {
      font-size: 1.4rem;
      margin: 0;
      white-space: nowrap;
      @media (min-width: $screen-small) and (max-width: $screen-medium-max) {
        font-size: 1.2rem;
      }

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
