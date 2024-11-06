<script setup>

import { computed, reactive, onMounted } from 'vue'

const getColor = computed(() => {
  const values = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f']
  let hexArray = ['#']
  const length = values.length

  var thisDate = new Date()
  console.log(thisDate)

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
  {{ getColor }}
  <div class="moon__content">

  </div>
</div>


</template>

<style lang='scss' scoped>

@import '../../vars.scss';
@import '../../mixins.scss';

.color {
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
}

</style>
