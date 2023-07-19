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
            placeholder="Login"
            prepend-inner-icon="mdi-account"
            outlined
            color="indigo darken-3"
            dense
          ></v-text-field>

          <v-text-field
            placeholder="Senha"
            prepend-inner-icon="mdi-lock"
            outlined
            color="indigo darken-3"
            dense
          ></v-text-field>
          <v-row align="center" justify="space-around">
            <v-col class="d-flex justify-center">
              <v-btn dark block color="#042d65" elevation="4">
                <!-- <span v-if="loading">
                      <i class="el-icon-loading"></i> Aguarde...
                    </span v-else>Entrar</span>
                    <span> -->
                <span>Entrar</span>
              </v-btn>
            </v-col>
          </v-row>
          <div class="text-center mt-4">
            <button type="text" class="text-gray-800">
              <span class="text-sm font-semibold">Esqueci minha senha</span>
            </button>
          </div>
        </v-container>
      </v-form>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'auth',
}
</script>

<style></style>
