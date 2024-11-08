<script setup>

import { reactive, defineProps, onMounted, computed } from 'vue'
import axios from 'axios'

const state = reactive({
  data: null
})

const props = defineProps({
  isTomorrow: Boolean,
})

// get color and explanation according to air quality index
const indexWarning = computed(() => {
  const index = state.data['gb-defra-index']
  if (index < 4) {
    return ['#11e462', 'low'];
  } else if (index < 7) {
    return ['#dce615', 'moderate']
  } else if (index < 9) {
    return ['#e41170', 'high']
  } else {
    return ['#820964', 'very high']
  }
})
 
function getTodaysData() {
  // fetch data from weather api
  axios.get('http://api.weatherapi.com/v1/current.json?key=efb8776062704d21bcc124921240611&q=Berlin&aqi=yes')
    .then(response => {
      if(response) {
          // console.log(response.data)
          // get only air quality data:
          state.data = response.data.current.air_quality
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
        const currentHour = new Date().getHours()
        // get air quality data for tomorrow, same time as now
        const data = response.data.forecast.forecastday[1].hour[currentHour].air_quality
        state.data = data
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

  <div id="airquality" class="airquality" v-if="state.data !== null">
    <h3>Air Quality Berlin</h3>
    <div class="airquality__content">

      <div class="airquality__section airquality__section-co">
        <span class="airquality__textelement hover-area">
          <p class="airquality__keyprop">CO:</p>
          <div class="tooltip">carbon dioxide</div>
        </span>
        <p class="airquality__textelement"> {{ state.data.co.toFixed(1) }}<span class="smaller-text"> µg/m</span></p>
      </div>

      <div class="airquality__section airquality__section-co">
        <span class="airquality__textelement hover-area">
          <p class="airquality__keyprop">DAQI:</p>
          <div class="tooltip">daily air quality index</div>
        </span>
        <span class="airquality__textelement hover-area">
          <p class="airquality__ac-index" :style="{'color': `${indexWarning[0]}`}">
            {{ state.data['gb-defra-index'] }}
          </p>
          <div class="tooltip">{{ indexWarning[1] }}</div>
        </span>
      </div>

      <div class="airquality__section airquality__section-no">
        <span class="airquality__textelement hover-area">
          <p class="airquality__keyprop">NO2:</p>
          <div class="tooltip">nitrogen dioxide</div>
        </span>
        <p class="airquality__textelement"> {{ state.data.no2.toFixed(1) }}<span class="smaller-text"> µg/m</span></p>
      </div>

    </div>
  </div>
  <div class="no-data" v-else>waiting for data...</div>

</template>

<style lang='scss' scoped>

@use '../../vars.scss' as v;
@use '../../mixins.scss' as m;

.airquality {
  @include m.flex(column, space-between, space-between, v.$gap-large);

  &__content {
    @include m.flex(column, center, center, 0.2rem);
    flex-grow: 1;
  }

  &__section {
    @include m.flex(row, baseline, center, v.$gap-large);
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
  }

  &__ac-index {
    font-weight: 800;
  }

}

</style>
