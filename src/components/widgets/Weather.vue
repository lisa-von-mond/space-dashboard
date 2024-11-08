<script setup>

import { computed, reactive, defineProps, onMounted } from 'vue'
import axios from 'axios'

const state = reactive({
  data: null
})

const props = defineProps({
  isTomorrow: Boolean,
})

/* computed prop to get angle for full word of wind direction (e.g. 'northeast' 
and angle for the arrow, corresponding to wind direction) */
const windDirection = computed(() => {
  let windDirection = state.data.wind_dir
  const windDirectionToArray = windDirection.split("")

  const translator = [
    {letter: 'N', word: 'north', angle: 0},
    {letter: 'E', word: 'east', angle: 90},
    {letter: 'S', word: 'south', angle: windDirectionToArray.some(x => x == 'E') ? 180 : -180},
    {letter: 'W', word: 'west', angle: -90},
  ]

  /* calculate angle by building sum and dividing by number of string elements */
    let angle = 0
    for (let i = 0; i < windDirectionToArray.length; i++) {
    let a = translator.find(el => el.letter == windDirectionToArray[i]).angle
    angle = angle + a
  }
  let finalAngle = angle/windDirectionToArray.length

  /* get full word for wind direction - loop through string and replace letters with words */
  for (let i = 0; i < windDirectionToArray.length; i++) {
    let translation = translator.find(el => el.letter === windDirectionToArray[i]).word
    windDirectionToArray[i] = translation
  }
  let windDirectionAsWord = windDirectionToArray.join("")

  /* return both values */
  return {direction: windDirectionAsWord, angle: finalAngle}
})

function getTodaysData() {

// fetch data from weather api
axios.get('http://api.weatherapi.com/v1/current.json?key=efb8776062704d21bcc124921240611&q=Berlin&aqi=yes')
  .then(response => {
    if(response) {
      // console.log(response.data)

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
}

function getTomorrowsData() {

// fetch data from weather api
axios.get('http://api.weatherapi.com/v1/forecast.json?key=efb8776062704d21bcc124921240611&q=Berlin&days=2&aqi=yes&alerts=no')
  .then(response => {
    if(response) {
      // console.log(response.data)

      // parameters needed for weather widget / get data for day tomorrow, same time as now:
      const params = ['condition', 'temp_c', 'feelslike_c', 'wind_kph', 'wind_dir']
      const currentHour = new Date().getHours()
      let responseData = response.data.forecast.forecastday[1].hour[currentHour]

      let weatherData = {}
      // push only relevant weather data into data object for widget component
        params.forEach(el => weatherData[el] = responseData[el])
        state.data = weatherData
      }
    })
    .catch(error => {console.log(error)})

}

onMounted(() => {
  if (props.isTomorrow === true) {
    getTomorrowsData()
  } else {
    getTodaysData()
  }
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

@use '../../vars.scss' as v;
@use '../../mixins.scss' as m;

.weather {
  @include m.flex(column, space-between, space-between, v.$gap-large);

  &__content {
    @include m.flex(row, center, center, v.$gap-large);
    flex-grow: 1;
    @media (min-width: v.$screen-medium) {
      gap: 2rem
    }
  }

  &__section-warmth, &__section-wind {
    width: 4rem;
    p {
      font-size: 1.4rem;
      margin: 0;
      white-space: nowrap;
      text-transform: uppercase;
      @media (min-width: v.$screen-small) and (max-width: v.$screen-medium-max) {
        font-size: 1.2rem;
      }
    }
    .smaller-text {
      font-size: 0.8rem;
    }
    hr {
      border-top: 1.6px solid v.$color-light;
      width: 2.6rem;
    }
  }

  &__section-condition {
    width: 5rem;
    @include m.flex(row, center, center);
    img {
      height: 5rem;
    }
  }
}

</style>
