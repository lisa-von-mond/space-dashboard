<script setup>

import { computed, reactive, onMounted } from 'vue'
import axios from 'axios'

const state = reactive({
  weatherData: null
})

/* computed prop to get angle for full word of wind direction (e.g. 'northeast' 
and angle for the arrow, corresponding to wind direction) */
const windDirection = computed(() => {
  let windDirection = state.weatherData.wind_dir
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
        <span class="hover-area">
          <img :src="state.weatherData.condition.icon"/>
          <div class="tooltip">{{ state.weatherData.condition.text }}</div>
        </span>
      </div>

      <div class="weather__section weather__section-wind">
        <p>{{ state.weatherData.wind_kph }} <span class="smaller-text">km/h</span></p>
        <hr>
        <p :style="{'transform': `rotate(${windDirection.angle}deg)`}">↑</p>
        <!---p>{{ windDirection.direction }}</p--->
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

/* hover areas and tooltips - may be moved to global leven */

.hover-area {
  position: relative;
  display: block;

.tooltip {
  position: absolute;
  right: 2rem;
  top: -1rem;
  transform: translate(100%, 100%);
  padding: 0.2rem;
  background: $color-secondary--dark;
  color: $color-accent;
  font-size: 0.8rem;
  text-transform: lowercase;
  white-space: nowrap;
  letter-spacing: 0.2rem;
  transition: opacity 0.3s;
  opacity: 0;
}

&:hover .tooltip{
    opacity: 1;
  }
}

</style>
