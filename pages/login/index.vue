<template>
  <div class="relative min-h-screen">
    <img
      class="absolute inset-0 w-full h-full object-cover"
      src="../../assets/bg.jpg"
      alt=""
    />
    <div
      class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 bg-white p-6 shadow-xl shadow-gray-800/50 rounded-lg"
    >
      <div class="my-2">
        <img class="w-60 mx-auto" src="~/assets/logo.png" alt="" />
      </div>
      <div class="my-6">
        <h1 class="text-lg font-medium text-center">Olá, Bem-vindo(a)</h1>
        <h1 class="text-center text-gray-600">
          Faça seu login para acessar a plataforma
        </h1>
      </div>
      <v-form>
        <v-container>
          <v-text-field
            v-model="email"
            placeholder="E-mail"
            prepend-inner-icon="mdi-account"
            outlined
            color="indigo darken-3"
            dense
            @keydown.enter="signin"
          ></v-text-field>

          <v-text-field
            v-model="password"
            placeholder="Senha"
            prepend-inner-icon="mdi-lock"
            outlined
            :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
            color="indigo darken-3"
            dense
            :type="show ? 'text' : 'password'"
            @click:append="show = !show"
            @keydown.enter="signin"
          ></v-text-field>
          <v-row align="center" justify="space-around">
            <v-col class="d-flex justify-center">
              <v-btn dark block color="#042d65" elevation="4" @click="signin">
                Entrar
              </v-btn>
            </v-col>
          </v-row>
          <div class="text-center mt-5">
            <span class="text-gray-600"> Esqueci minha senha</span>
          </div>
        </v-container>
      </v-form>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'auth',
  middleware: [],

  data() {
    return {
      email: '',
      password: '',
      show: false,
    }
  },

  methods: {
    async signin() {
      try {
        await this.$auth.loginWith('local', {
          data: { email: this.email, password: this.password },
        })
        this.$router.push('/dashboard')
      } catch (error) {
        if (
          error.response &&
          error.response.data &&
          error.response.data.error
        ) {
          console.log(error.response.data.message)
          this.$toast.error(error.response.data.message, {
            position: 'top-center',
          })
        } else {
          this.$toast.error('Não foi possível conectar ao servidor.', {
            position: 'top-center',
          })
        }
      }
    },
  },
}
</script>

<style></style>
