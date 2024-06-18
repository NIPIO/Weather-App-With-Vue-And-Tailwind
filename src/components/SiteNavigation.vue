<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg" style="z-index: 1">
    <nav class="container flex flex-col sm:flex-row items-center gap-3 text-white py-6">
      <RouterLink :to="{name: 'home'}">
        <div class="flex items-center gap-3 flex-1">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl"> Pronóstico del Tiempo</p>
        </div>
      </RouterLink>
      <div class="flex gap-3 flex-1 justify-end">
        <i class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer" @click="toggleModal"></i>
      </div>
      <BaseModal :modalActive="modalActive" @cerrar-modal="toggleModal" />
    </nav>
  </header>
</template>


<script>
import BaseModal from "@/components/BaseModal.vue";
import {defineComponent, ref} from "vue";

export default defineComponent({
  name: "SiteNavigation",
  components: {
    BaseModal,
  },
  setup() {
    let modalActive = ref(false)

    const toggleModal = () => {
      modalActive.value = !modalActive.value
    }

    return {
      modalActive,
      toggleModal,
    }
  },

})
</script>