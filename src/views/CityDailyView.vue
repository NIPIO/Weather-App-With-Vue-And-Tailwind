<template>
  <div class="max-w-screen-md w-full py-12">
    <div class="mx-8 text-white">
      <h2 class="mb-4">Por día</h2>
      <div v-for="day in cityDataDaily" :key="day.dt" class="flex items-center">
        <p class="flex-1">
          {{dayUppercase(day.dt)}}
        </p>
        <img class="w-[50px] object-cover" :src="`http://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`">
        <div class="flex gap-2 flex-1 justify-end">
          <p>{{Math.round(day.temp.min  - 273.15) }}&deg/{{Math.round(day.temp.max  - 273.15) }}&deg</p>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "CityDailyView",
  props: ['cityDataDaily'],
  methods: {
    dayUppercase(dayDt) {
      let dayEdit = new Date(dayDt * 1000).toLocaleDateString("es-us", {weekday: "long"})
      return dayEdit.charAt(0).toUpperCase() + dayEdit.slice(1)
    }
  }
}
</script>