<script setup>

import { computed, reactive, onMounted } from 'vue'

const getColor = computed(() => {

  const values = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f']
  let hexArray = ['#']
  const length = values.length

  var thisDate = new Date()

  var day = thisDate.getDay()
  var month = thisDate.getMonth()
  var year = thisDate.getYear()
  var date = thisDate.getDate()
  var fullYear = thisDate.getFullYear()
  var random = month * date

  const array = [day, month, year, date, fullYear, random]

  array.forEach(el => {
    let index = el % length
    hexArray.push(values[index])
  })

  return hexArray.join("")

})

</script>

<template>

<div id="color" class="color">
  <h3>Color of the day</h3>
  <div class="color__content">
    <p>{{ getColor }}</p>
    <div 
      class="color__circle"
      :style="{'background': `${getColor}`}"></div>
  </div>
</div>

</template>

<style lang='scss' scoped>

@import '../../vars.scss';
@import '../../mixins.scss';

.color {
  @include flex(column, space-between, space-between, 1rem);

  &__content {
    flex-grow: 1;
    @include flex(row, center, center, 2rem);
    flex-wrap: wrap;
    p {
      font-size: 1.4rem;
      margin: 0;
      @media (min-width: $screen-small) and (max-width: $screen-medium-max) {
        font-size: 1.2rem;
      }
    }
  }

  &__circle {
    height: 3rem;
    width: 3rem;
    border-radius: 50%;
  }
}

</style>
