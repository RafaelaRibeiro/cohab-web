<template>
  <div>
    <div class="flex flex-col justify-center items-center m-0">
      <div class="w-4/5 flex">
        <v-text-field
          v-model="searchTerm"
          outlined
          placeholder="Pesquise pelo name do mutuário ou o número do contrato"
          clearable
          class="mr-3"
          prepend-inner-icon="mdi-magnify"
        >
        </v-text-field>

        <v-btn class="mt-1.5" dark color="#1A237E" @click="filterUsers"
          >Buscar</v-btn
        >
      </div>
    </div>
    <div class="p-4">
      <v-data-table
        v-if="filteredUsers.length > 0"
        :headers="headers"
        :items="filteredUsers"
        hide-default-footer
        :items-per-page="99999"
      >
      </v-data-table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filteredUsers: [],
      searchTerm: '',
      headers: [
        {
          text: 'Contrato',
          align: 'start',
          sortable: true,
          value: 'contract',
          width: '10%',
        },
        {
          text: 'Nome',
          align: 'start',
          sortable: true,
          value: 'name',
          width: '30%',
        },
        {
          text: 'Telefone',
          align: 'start',
          sortable: true,
          value: 'phone',
          width: '20%',
        },
        {
          text: 'Email',
          align: 'start',
          sortable: true,
          value: 'email',
          width: '25%',
        },
        {
          text: 'Assinatura',
          align: 'start',
          sortable: true,
          value: 'signature status',
          width: '25%',
        },
        {
          text: 'Notificações',
          align: 'start',
          sortable: true,
          value: 'notifications status',
          width: '25%',
        },
      ],
      users: [
        {
          id: 1,
          contract: '123456',
          name: 'João Santos da Silva',
          phone: '(11) 1111-1111',
          email: 'joao.santos@example.com',
          'signature status': 'assinado',
          'notifications status': 'enviado',
        },
        {
          id: 2,
          contract: '789012',
          name: 'Maria Oliveira Martins',
          phone: '(11) 2222-2222',
          email: 'maria.oliveira@example.com',
          'signature status': 'não assinado',
          'notifications status': 'enviado',
        },
        {
          id: 3,
          contract: '345678',
          name: 'José Pereira Rodrigues',
          phone: '(11) 3333-3333',
          email: 'jose.pereira@example.com',
          'signature status': 'não assinado',
          'notifications status': 'enviado',
        },
        {
          id: 4,
          contract: '901234',
          name: 'Ana Ferreira Cardoso',
          phone: '(11) 4444-4444',
          email: 'ana.ferreira@example.com',
          'signature status': 'assinado',
          'notifications status': 'enviado',
        },
        {
          id: 5,
          contract: '567890',
          name: 'Pedro Silva Sousa',
          phone: '(11) 5555-5555',
          email: 'pedro.silva@example.com',
          'signature status': 'assinado',
          'notifications status': 'enviado',
        },
        {
          id: 6,
          contract: '234567',
          name: 'João Ferreira',
          phone: '(11) 6666-6666',
          email: 'joao.ferreira@example.com',
          'signature status': 'assinado',
          'notifications status': 'enviado',
        },
        {
          id: 7,
          contract: '890123',
          name: 'Maria Lima',
          phone: '(11) 7777-7777',
          email: 'maria.lima@example.com',
          'signature status': 'não assinado',
          'notifications status': 'enviado',
        },
        {
          id: 8,
          contract: '456789',
          name: 'José Santos',
          phone: '(11) 8888-8888',
          email: 'jose.santos@example.com',
          'signature status': 'assinado',
          'notifications status': 'enviado',
        },
        {
          id: 9,
          contract: '012345',
          name: 'Ana Pereira',
          phone: '(11) 9999-9999',
          email: 'ana.pereira@example.com',
          'signature status': 'não assinado',
          'notifications status': 'enviado',
        },
        {
          id: 10,
          contract: '678901',
          name: 'Pedro Lima',
          phone: '(11) 0000-0000',
          email: 'pedro.lima@example.com',
          'signature status': 'não assinado',
          'notifications status': 'enviado',
        },
      ],
    }
  },

  methods: {
    filterUsers() {
      if (!this.searchTerm) {
        this.filteredUsers = this.users // Show all users if the search term is empty
      } else {
        const term = this.searchTerm.toLowerCase().trim()

        this.filteredUsers = this.users.filter((user) => {
          const contract = user.contract.toLowerCase()
          const name = user.name.toLowerCase()

          return contract.includes(term) || name.includes(term)
        })
      }
    },
  },

  watch: {
    filteredUsers(newValue) {
      this.$emit('filtered-users', newValue)
    },
  },
}
</script>

<style></style>
