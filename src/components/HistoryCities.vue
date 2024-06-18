<template>
  <h2 class="mb-1">Últimas: </h2>
  <div class="overflow-x-scroll h-14 my-2 ">
    <div v-for="city in citiesStorage"
         class="inline mx-1 whitespace-nowrap cursor-pointer  rounded-md bg-weather-secondary py-1.5 px-3 font-sans text-xs uppercase text-white"
         @click="searchCityChild(city)">
      <span>{{ city.name }}, {{ city.state ?? ' - ' }}, {{ city.country }}</span>
    </div>
  </div>
</template>

<script>
import {defineComponent} from "vue";

export default defineComponent({
  name: "HistoryCities",
  props: ['citiesStorage'],
  emits: ['search-city'],
  setup(props, {emit}) {
    const searchCityChild = city => {
      city.history = true
      emit('search-city', city)
    }

    return {
      searchCityChild
    }
  }
  })
</script>

<style>
::-webkit-scrollbar{
  height: 6px;
  width: 4px;
  background: #004E71;
}
::-webkit-scrollbar-thumb:horizontal{
  height: 4px;
  background: #1498c7;
}
</style>