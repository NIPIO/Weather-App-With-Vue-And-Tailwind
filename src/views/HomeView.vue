<template class="d-flex">
  <main class="container text-white">
    <InputSearch/>
  </main>
</template>


<script>
import InputSearch from "@/components/InputSearch.vue";
export default {
  name: "HomeView",
  components: {
    InputSearch
  }
}
</script>