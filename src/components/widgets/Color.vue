<script setup>

import { computed, defineProps } from 'vue'

const props = defineProps({
  isTomorrow: Boolean,
})

const getColor = computed(() => {

  const values = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f']
  let hexArray = ['#']
  const length = values.length

  let today = new Date()
  let todayMs = today.getTime()
  let tomorrowMs = todayMs + (1000 * 60 * 60 * 24)
  let tomorrow = new Date(tomorrowMs)

  let thisDate = today
  if (props.isTomorrow === true) {
    thisDate = tomorrow
  }

  let day = thisDate.getDay()
  let month = thisDate.getMonth() * day
  let year = thisDate.getYear() * day
  let date = thisDate.getDate() * day
  let fullYear = thisDate.getFullYear() * day
  let random = day * month * year * date * fullYear

  const params = [day, month, year, date, fullYear, random]

  params.forEach(el => {
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

@use '../../vars.scss' as v;
@use '../../mixins.scss' as m;

.color {
  @include m.flex(column, space-between, space-between, v.$gap-large);

  &__content {
    flex-grow: 1;
    @include m.flex(row, center, center, 2rem);
    flex-wrap: wrap;
    p {
      font-size: 1.4rem;
      margin: 0;
      text-transform: uppercase;
      @media (min-width: v.$screen-small) and (max-width: v.$screen-medium-max) {
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
