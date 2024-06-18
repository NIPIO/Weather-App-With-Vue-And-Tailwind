<template>
  <div class="flex flex-col items-center text-white py-12">
    <h1 class="text-4xl mb-2">
      {{ cityGeneralData.name }}
    </h1>
    <p class="text-sx mb-12">
      {{ cityGeneralData.state }},
      {{ cityGeneralData.country }}
    </p>
    <p class="text-8xl mb-8">
      {{ Math.round(cityDataCurrent.temp  - 273.15) }}&deg
    </p>
    <div class="text-center">
      <p>
        Sensación Térmica {{ Math.round((cityDataCurrent.feels_like  - 273.15)) }}&deg
      </p>
      <img class="w-[150px] h-auto" :src="`http://openweathermap.org/img/wn/${cityDataCurrent.weather[0].icon}@2x.png`">
      <p>
        Nubosidad {{ cityDataCurrent.clouds }} %
      </p>
    </div>


  </div>
</template>


<script>
export default {
  name: "CityCurrentView",
  props: ['cityDataCurrent', 'cityGeneralData']
}
</script>