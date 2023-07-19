<template>
  <div>
    <div class="flex flex-col justify-center items-center m-0">
      <div class="w-4/5 flex">
        <v-text-field
          v-model="termoPesquisa"
          outlined
          placeholder="Pesquise pelo nome do mutuário ou o número do contrato"
          clearable
          class="mr-3"
          prepend-inner-icon="mdi-magnify"
        >
        </v-text-field>

        <v-btn class="mt-1.5" dark color="#1A237E" @click="filtrarUsuarios"
          >Buscar</v-btn
        >
      </div>
    </div>
    <div class="p-4">
      <v-data-table
        v-if="usuariosFiltrados.length > 0"
        :headers="headers"
        :items="usuariosFiltrados"
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
      usuariosFiltrados: [],
      termoPesquisa: '',
      headers: [
        {
          text: 'Contrato',
          align: 'start',
          sortable: true,
          value: 'contrato',
          width: '10%',
        },
        {
          text: 'Nome',
          align: 'start',
          sortable: true,
          value: 'nome',
          width: '30%',
        },
        {
          text: 'Telefone',
          align: 'start',
          sortable: true,
          value: 'telefone',
          width: '20%',
        },
        {
          text: 'E-mail',
          align: 'start',
          sortable: true,
          value: 'email',
          width: '25%',
        },
        {
          text: 'Assinatura',
          align: 'start',
          sortable: true,
          value: 'status assinatura',
          width: '25%',
        },
        {
          text: 'Notificações',
          align: 'start',
          sortable: true,
          value: 'status notificações',
          width: '25%',
        },
      ],
      usuarios: [
        {
          id: 1,
          contrato: '123456',
          nome: 'João Santos da Silva',
          telefone: '(11) 1111-1111',
          email: 'joao.santos@example.com',
          'status assinatura': 'assinado',
          'status notificações': 'enviado',
        },
        {
          id: 2,
          contrato: '789012',
          nome: 'Maria Oliveira Martins',
          telefone: '(11) 2222-2222',
          email: 'maria.oliveira@example.com',
          'status assinatura': 'não assinado',
          'status notificações': 'enviado',
        },
        {
          id: 3,
          contrato: '345678',
          nome: 'José Pereira Rodrigues',
          telefone: '(11) 3333-3333',
          email: 'jose.pereira@example.com',
          'status assinatura': 'não assinado',
          'status notificações': 'enviado',
        },
        {
          id: 4,
          contrato: '901234',
          nome: 'Ana Ferreira Cardoso',
          telefone: '(11) 4444-4444',
          email: 'ana.ferreira@example.com',
          'status assinatura': 'assinado',
          'status notificações': 'enviado',
        },
        {
          id: 5,
          contrato: '567890',
          nome: 'Pedro Silva Sousa',
          telefone: '(11) 5555-5555',
          email: 'pedro.silva@example.com',
          'status assinatura': 'assinado',
          'status notificações': 'enviado',
        },
        {
          id: 6,
          contrato: '234567',
          nome: 'João Ferreira',
          telefone: '(11) 6666-6666',
          email: 'joao.ferreira@example.com',
          'status assinatura': 'assinado',
          'status notificações': 'enviado',
        },
        {
          id: 7,
          contrato: '890123',
          nome: 'Maria Lima',
          telefone: '(11) 7777-7777',
          email: 'maria.lima@example.com',
          'status assinatura': 'não assinado',
          'status notificações': 'enviado',
        },
        {
          id: 8,
          contrato: '456789',
          nome: 'José Santos',
          telefone: '(11) 8888-8888',
          email: 'jose.santos@example.com',
          'status assinatura': 'assinado',
          'status notificações': 'enviado',
        },
        {
          id: 9,
          contrato: '012345',
          nome: 'Ana Pereira',
          telefone: '(11) 9999-9999',
          email: 'ana.pereira@example.com',
          'status assinatura': 'não assinado',
          'status notificações': 'enviado',
        },
        {
          id: 10,
          contrato: '678901',
          nome: 'Pedro Lima',
          telefone: '(11) 0000-0000',
          email: 'pedro.lima@example.com',
          'status assinatura': 'não assinado',
          'status notificações': 'enviado',
        },
      ],
    }
  },

  methods: {
    filtrarUsuarios() {
      if (!this.termoPesquisa) {
        this.usuariosFiltrados = this.usuarios // Exibe todos os usuários se o termo de pesquisa estiver vazio
      } else {
        const termo = this.termoPesquisa.toLowerCase().trim()

        this.usuariosFiltrados = this.usuarios.filter((usuario) => {
          const contrato = usuario.contrato.toLowerCase()
          const nome = usuario.nome.toLowerCase()

          return contrato.includes(termo) || nome.includes(termo)
        })
      }
    },
  },
}
</script>

<style></style>
