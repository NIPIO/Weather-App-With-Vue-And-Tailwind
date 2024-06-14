<template>
  <div class="pt-4 mb-8 relative">
    <input type="text" placeholder="Buscá por ciudad o provincia"
           class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0_1px_0_0_#FFFFFF]"
           v-model="cityToSearch" @input="getCities">
    <ul v-if="citiesSearched.length > 0"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
      <li v-for="city in citiesSearched" :key="city.code" class="py-2 px-3 cursor-pointer hover:bg-weather-primary"
          @click="searchCityWeather(city)">
        {{ city.name }}, {{ city.state }}, {{ city.country }}
      </li>
    </ul>
    <CityView v-if="cityData" :cityData="cityData"/>
  </div>
</template>


<script setup>

import {ref} from "vue";
import axios from "axios";
import CityView from "@/views/CityView.vue";

let cityToSearch = ref("");
let citiesSearched = ref([]);
let cityData = ref(null);
let previewCity = ref(null);

const getCities = async () => {
  if (cityToSearch.value !== "" && cityToSearch.value.length > 2) {
    let result = await axios.get(`https://api.thecompaniesapi.com/v1/locations/cities?search=${cityToSearch.value}`)

    citiesSearched = result.data.cities.map(city => {
      return {
        "name": city.name,
        "state": city.state.name,
        "country": city.country.name,
        "lat": city.latitude,
        "lon": city.longitude,
        "code": city.code
      }
    })
  } else {
    citiesSearched = []
  }
}


const searchCityWeather = async (city) => {
  //guardo en el localStorage la anterior (en caso de que exista).
  saveInStorage(`${city.name}, ${city.state}, ${city.country}`)

  citiesSearched = []
  let apiData = await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${city.lat}&lon=${city.lon}&exclude=minutely&appid=cf002f1aa0c77c078ea36fbd977194d1`)

  cityData.value = {
    "city": city,
    "daily": apiData.data.daily,
    "hourly": apiData.data.hourly,
    "current": apiData.data.current,
  }

}

const saveInStorage = (city) => {
  let savedItemsArray = []
  if (localStorage.getItem('savedItems')) {
    savedItemsArray = JSON.parse(localStorage.getItem('savedItems'))
    savedItemsArray.unshift(city)
    localStorage.setItem('savedItems', JSON.stringify(savedItemsArray))
  } else {
    savedItemsArray.unshift(city)
    localStorage.setItem('savedItems', JSON.stringify(savedItemsArray))
  }
}

</script>