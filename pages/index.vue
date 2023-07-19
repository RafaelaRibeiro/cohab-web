<template>
  <div class="h-[95vh] mt-3 bg-white">
    <div class="h-1/4 flex items-center justify-center">
      <h1 class="text-2xl text-center text-gray-700">
        Bem vindo à plataforma de importação da COHAB Minas
      </h1>
    </div>

    <div class="h-1/4">
      <search-main />
    </div>
    <div class="h-2/4 flex justify-center items-center mx-16 pb-20">
      <a
        class="h-32 w-52 mr-8 rounded-lg shadow-md bg-[#394da1] flex items-center justify-center"
      >
        <span class="text-white text-xl">Importação</span>
      </a>
      <a
        class="h-32 w-52 mr-8 rounded-lg shadow-md bg-[#394da1] flex items-center justify-center"
      >
        <span class="text-white text-xl">Dashboard</span></a
      >
      <a
        class="h-32 w-52 mr-8 rounded-lg shadow-md bg-[#394da1] flex items-center justify-center"
        ><span class="text-white text-xl">Relatórios</span></a
      >
    </div>
  </div>
</template>

<script>
import SearchMain from '../components/search/SearchMain.vue'
import DivBase from '../components/UI/DivBase.vue'
export default {
  components: { SearchMain, DivBase },
}
</script>

<style></style>
