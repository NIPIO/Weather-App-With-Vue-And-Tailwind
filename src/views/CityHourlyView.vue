<template>
  <div class="max-w-screen-md w-full py-12">
    <h2 class="mb-4"> Por hora</h2>
    <div class="flex gap-10 overflow-x-scroll">
      <div v-for="hourData in cityDataHourly" :key="hourData.dt" class="flex flex-col gap-4 items-center">
        <p class="whitespace-nowrap textx-md">
          {{hourData.dt}}
          {{
            new Date(hourData.dt).toLocaleTimeString("en-US", {hour: "numeric"})
          }}
        </p>
        <img class="w-[150px] h-auto" :src="`http://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`">
        <p class="text-xl">
          {{ Math.round(hourData.temp  - 273.15) }}&deg
        </p>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "CityHourlyView",
  props: ['cityDataHourly']
}
</script>