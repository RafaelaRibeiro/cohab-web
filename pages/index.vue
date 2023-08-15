<template>
  <div class="min-h-[80vh] mt-3 bg-white flex items-center justify-center">
    <div class="w-4/5 pt-8">
      <div class="h-1/4 flex items-center justify-center">
        <h1 class="text-2xl text-center text-gray-700">
          Bem vindo à plataforma de importação da COHAB Minas
        </h1>
      </div>

      <div class="h-1/6 m-8">
        <search @filtered-users="filteredUsers" />
      </div>
      <div
        v-show="numberOfFilteredUsers === null"
        class="h-2/4 flex justify-center mx-10"
      >
        <a
          class="h-32 w-52 mr-8 rounded-lg border-t-4 border-[#394da1] shadow-lg shadow-gray-800/60 hover:shadow-gray-800/100 flex items-center justify-center"
        >
          <span class="text-gray-800 text-xl">Importação</span>
        </a>
        <a
          class="h-32 w-52 mr-8 rounded-lg border-t-4 border-[#394da1] shadow-lg shadow-gray-800/60 hover:shadow-gray-800/100 flex items-center justify-center"
        >
          <span class="text-gray-800 text-xl">Dashboard</span></a
        >
        <a
          class="h-32 w-52 mr-8 rounded-lg border-t-4 border-[#394da1] shadow-lg shadow-gray-800/60 hover:shadow-gray-800/100 flex items-center justify-center"
          ><span class="text-gray-800 text-xl">Relatórios</span></a
        >
      </div>
      <div class="flex justify-center" v-show="numberOfFilteredUsers === 0">
        <p class="text-lg">Mutuário não encontrado</p>
      </div>
    </div>
  </div>
</template>

<script>
import DivBase from '../components/UI/DivBase.vue'
import Search from '../components/UI/Search.vue'
export default {
  components: { DivBase, Search },
  data() {
    return {
      numberOfFilteredUsers: null,
    }
  },

  methods: {
    filteredUsers(filteredUsers) {
      this.numberOfFilteredUsers = filteredUsers.length
    },
  },
}
</script>

<style></style>
