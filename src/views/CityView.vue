<template>
  <CityCurrentView :cityGeneralData="cityData.city" :cityDataCurrent="cityData.current"/>
  <hr class="border-white border-opacity-10 border w-full">
  <CityHourlyView :cityDataHourly="cityData.hourly"/>
  <hr class="border-white border-opacity-10 border w-full">
  <CityDailyView :cityDataDaily="cityData.daily"/>
</template>

<script>
import CityCurrentView from "@/views/CityCurrentView.vue";
import CityDailyView from "@/views/CityDailyView.vue";
import CityHourlyView from "@/views/CityHourlyView.vue";

export default {
  name: "CityView",
  components: {
    CityCurrentView,
    CityDailyView,
    CityHourlyView
  },
  props: ['cityData']
}
</script>