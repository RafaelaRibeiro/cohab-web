<template>
  <div class="w-[50vw]">
    <div class="h-20">
      <h1 class="text-2xl my-1">Eventos</h1>
      <p class="text-gray-600">
        Gerencie o status das notificações enviadas aos mutuários.
      </p>
    </div>
    <v-divider></v-divider>

    <div class="mt-4 flex justify-between">
      <div>
        <v-btn class="white--text" color="#323288" @click="newEvent = true">
          <v-icon small left> mdi-plus </v-icon>
          Novo Evento
        </v-btn>
      </div>

      <div class="w-1/2">
        <v-text-field
          v-model="search"
          color="#323288"
          dense
          append-icon="mdi-magnify"
          label="Pesquisar Evento"
          outlined
        ></v-text-field>
      </div>
    </div>

    <div class="flex flex-row" v-if="newEvent">
      <v-text-field
        color="#323288"
        dense
        outlined
        name="name"
        label="Nome do evento"
        id="id"
        class="mr-10"
      ></v-text-field>

      <v-btn class="white--text" color="#004011" @click="saveEvents">
        <v-icon small left> mdi-content-save-plus-outline </v-icon>
        Salvar
      </v-btn>
    </div>

    <div class="mt-4">
      <v-data-table
        :items="events"
        :headers="headers"
        :search="search"
        hide-default-footer
        :loading="loading"
        loading-text="Carregando eventos..."
        :items-per-page="99999"
        @click:row="openDetails"
        style="cursor: pointer"
      >
        <!-- eslint-disable-next-line vue/valid-v-slot -->
        <template v-slot:item.status="{ item }">
          <span v-if="item.status">Ativo</span>
          <span v-else>Inativo</span>
        </template>
      </v-data-table>
    </div>
    <v-dialog v-model="dialog" max-width="600px">
      <div class="w-full bg-white rounded-lg shadow p-6">
        <h1 class="font-medium text-xl mb-6">Editar Evento</h1>
        <label for="name" class="block text-gray-700 text-sm font-medium"
          >Descrição:</label
        >
        <div class="flex flex-row justify-center items-center">
          <input
            class="rounded px-4 py-2 mr-4 text-base block w-full text-gray-700 leading-tight bg-white border border-solid border-[#323288] focus:border-2 focus:outline-none focus:border-colorButton"
            type="text"
            v-model="selectedEvent.description"
          />
        </div>

        <div class="mt-4">
          <v-textarea
            v-model="selectedEvent.value"
            outlined
            name="input-7-1"
            label="Mensagem"
          ></v-textarea>
        </div>
        <div class="mt-4 flex justify-end">
          <v-btn class="mr-4" @click="saveEvents" dark color="#004011"
            >Salvar</v-btn
          >
          <v-btn @click="dialog = false" color="error">Cancelar</v-btn>
        </div>
      </div>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      newEvent: false,
      loading: false,
      search: '',
      selectedEvent: {},
      headers: [
        {
          text: 'Código',
          align: 'start',
          sortable: true,
          value: 'id',
          width: '7%',
        },
        {
          text: 'Mensagem',
          align: 'start',
          sortable: true,
          value: 'value',
          width: '30%',
        },
        {
          text: 'Descrição',
          align: 'start',
          sortable: true,
          value: 'description',
          width: '20%',
        },
      ],

      events: [],
    }
  },
  methods: {
    saveEvents() {
      this.newEvent = false
      this.selectedEvent = {}
    },

    openDetails(item) {
      this.selectedEvent = { ...item } // criar uma cópia do item para evitar mutações diretas
      this.dialog = true
    },

    async getEvents() {
      this.loading = true
      const API_KEY = process.env.VUE_APP_API_KEY
      const proxyUrl = 'https://cors-anywhere.herokuapp.com/'
      const url = 'https://conectawebhook.com.br/api/v1/webhook/bot_fields/'

      try {
        const response = await this.$axios.$get(proxyUrl + url, {
          headers: {
            accept: 'application/json',
            'api-key': API_KEY,
          },
        })
        this.events = response

        this.events = this.events.map((item) => ({
          ...item,
          description: `Descrição da mensagem com id ${item.id}`,
        }))
      } catch (error) {
        console.error('An error occurred while fetching events:', error)
      } finally {
        this.loading = false
      }
    },

    async saveEvents() {
      this.loading = true
      const API_KEY = process.env.VUE_APP_API_KEY
      const proxyUrl = 'https://cors-anywhere.herokuapp.com/'
      const url = `https://conectawebhook.com.br/api/v1/webhook/bot_fields/${this.selectedEvent.id}/`

      try {
        await this.$axios.$post(
          proxyUrl + url,
          {
            value: this.selectedEvent.value,
          },
          {
            headers: {
              accept: 'application/json',
              'api-key': API_KEY,
            },
          }
        )
        this.getEvents()
        this.dialog = false
      } catch (error) {
        console.error('An error occurred while saving the event:', error)
      } finally {
        this.loading = false
      }
    },
  },

  async mounted() {
    await this.getEvents()
  },
}
</script>

<style></style>
