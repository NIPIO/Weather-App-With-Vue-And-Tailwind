<template>
  <div class="pt-4 mb-8 relative">
    <div>
      <HistoryCities v-if="getStorageCities()" :citiesStorage="getStorageCities()" @search-city="searchCityWeather"  />
    </div>
    <input type="text" placeholder="Buscá por ciudad o provincia"
           class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0_1px_0_0_#FFFFFF]"
           v-model="cityToSearch" @input="getCities">
    <ul v-if="citiesSearched.length > 0"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
      <li v-for="city in citiesSearched" :key="city.code" class="py-2 px-3 cursor-pointer hover:bg-weather-primary"
          @click="searchCityWeather(city)">
        {{ city.name }}, {{ city.state ?? ' - ' }}, {{ city.country }}
      </li>
    </ul>

    <CityView v-if="cityData" :cityData="cityData"/>
  </div>
  <Alert :msg="reportError.msg" v-if="reportError.status"/>

</template>

<script>
import {defineComponent, ref} from "vue";
import axios from "axios";
import CityView from "@/views/CityView.vue";
import Alert from "@/components/Alert.vue";
import HistoryCities from "@/components/HistoryCities.vue";

export default defineComponent({
  name: "InputSearch",
  components: {HistoryCities, Alert, CityView},
  setup() {
    let cityToSearch = ref("");
    let citiesSearched = ref([]);
    let cityData = ref(null);
    let reportError = ref({status: false, msg: "Ocurrió un error en la conexión"});

    const getCities = async () => {
      try {
        if (cityToSearch.value !== "" && cityToSearch.value.length > 2) {
          let result = await axios.get(`https://api.thecompaniesapi.com/v1/locations/cities?search=${cityToSearch.value}`)
          citiesSearched.value = result.data.cities.map(city => {
            return {
              "name": city.name,
              "state": city.state?.name,
              "country": city.country.name,
              "lat": city.latitude,
              "lon": city.longitude,
              "code": city.code
            }
          })
          reportError.value.status = false
        } else {
          citiesSearched.value = []
        }
      } catch (e) {
        reportError.value.status = true
        citiesSearched.value = []
      }

    }

    const searchCityWeather = async (city) => {
      cityToSearch.value = `${city.name}, ${city.state}, ${city.country}`

      try {
        citiesSearched.value = []
        let apiData = await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${city.lat}&lon=${city.lon}&exclude=minutely&appid=cf002f1aa0c77c078ea36fbd977194d1`)

        cityData.value = {
          "city": city,
          "daily": apiData.data.daily,
          "hourly": apiData.data.hourly,
          "current": apiData.data.current,
        }

        //guardo en el localStorage la anterior (en caso de que exista).
        saveInStorage(city)

        reportError.value.status = false

      } catch (e) {
        reportError.value.status = true
      }
    }

    const saveInStorage = (city) => {
      let savedItemsArray = []
      if (localStorage.getItem('savedItems')) {
        savedItemsArray = getStorageCities()

        if(!city.hasOwnProperty('history')) { //If it isn't a new search don't save it again in LS
          if (savedItemsArray[0] !== city) {
            savedItemsArray.unshift(city)
            localStorage.setItem('savedItems', JSON.stringify(savedItemsArray))
          }
        }
      } else {
        savedItemsArray.unshift(city)
        localStorage.setItem('savedItems', JSON.stringify(savedItemsArray))
      }
    }

    const getStorageCities = () => {
      return JSON.parse(localStorage.getItem('savedItems'))
    }

    return {
      searchCityWeather,
      getCities,
      getStorageCities,
      cityToSearch,
      citiesSearched,
      cityData,
      reportError
    }
  },

})
</script>