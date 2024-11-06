<script setup>

import { reactive, onMounted, computed } from 'vue'
import axios from 'axios'

const state = reactive({
  data: null
})

const indexColor = computed(() => {
  const index = state.data['gb-defra-index']
  if (index < 4) {
    return '#11e462';
  } else if (index < 7) {
    return '#dce615'
  } else if (index < 9) {
    return '#e41170'
  } else {
    return '#820964'
  }
})

onMounted(() => {
// fetch data from weather api
axios.get('http://api.weatherapi.com/v1/current.json?key=efb8776062704d21bcc124921240611&q=Berlin&aqi=yes')
  .then(response => {
    console.log(response.data)
    if(response) {
        // push only air quality data into data array for widget component
        state.data = response.data.current.air_quality
      }
    })
    .catch(error => {console.log(error)})
})

</script>

<template>

  <div id="airquality" class="airquality" v-if="state.data !== null">
    <h3>Air Quality Berlin</h3>
    <div class="airquality__content">

      <div class="airquality__section airquality__section-co">
        <p> {{ state.data.co.toFixed(1) }}<span class="smaller-text"> µg/m</span></p>
        <hr>
        <p>CO</p>
      </div>

      <div class="airquality__section airquality__section-co">
        <p class="airquality__index" :style="{'color': `${indexColor}`}">
          {{ state.data['gb-defra-index'] }}
        </p>
        <hr>
        <p>DAQI</p>
      </div>

      <div class="airquality__section airquality__section-no">
        <p> {{ state.data.no2.toFixed(1) }}<span class="smaller-text"> µg/m</span></p>
        <hr>
        <p>NO2</p>
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

  &__section {
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

  &__index {
    font-weight: 800;
  }

}

</style>
