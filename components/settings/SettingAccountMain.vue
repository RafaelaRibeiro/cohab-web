<template>
  <div class="flex flex-row">
    <div class="w-2/3">
      <div class="h-20">
        <h1 class="text-2xl my-1">Minha Conta</h1>
        <p class="text-gray-600">
          Aqui você pode gerenciar as configurações e informações da sua conta.
        </p>
      </div>
      <v-divider></v-divider>

      <div class="flex flex-row mt-4">
        <v-text-field
          color="#323288"
          class="mr-4"
          outlined
          dense
          label="Login"
        ></v-text-field>
        <v-btn class="mt-1.5" small dark color="#323288">
          <v-icon small left>mdi-key</v-icon> Alterar senha</v-btn
        >
      </div>
      <div class="flex flex-row">
        <v-text-field
          color="#323288"
          class="mr-4"
          outlined
          dense
          label="Nome"
        ></v-text-field>
        <v-text-field
          color="#323288"
          outlined
          dense
          label="Sobrenome"
        ></v-text-field>
      </div>
      <div class="flex flex-row">
        <v-text-field
          color="#323288"
          class="mr-4"
          outlined
          dense
          label="Telefone"
        ></v-text-field>
        <v-text-field
          color="#323288"
          outlined
          dense
          label="Sobrenome"
        ></v-text-field>
      </div>
    </div>
    <v-divider vertical class="pl-5"></v-divider>
    <div class="w-1/3">
      <div class="flex flex-col items-center justify-center">
        <img
          src="../../assets/avatar.png"
          alt="Imagem de perfil"
          class="w-20 h-20 rounded-full"
        />
        <v-btn small color="#323288" dark class="mt-4">Alterar imagem</v-btn>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style></style>
