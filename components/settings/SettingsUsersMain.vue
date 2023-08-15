<template>
  <div class="w-[50vw]">
    <div class="h-20">
      <h1 class="text-2xl my-1">Usuários</h1>
      <p class="text-gray-600">Gerencie os usuários da plataforma.</p>
    </div>

    <v-divider></v-divider>

    <div class="mt-4 flex justify-between">
      <div>
        <v-btn class="white--text" color="#323288" @click="dialog = true">
          <v-icon small left> mdi-plus </v-icon>
          Novo Usuário
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

    <v-dialog v-model="dialog" max-width="600px">
      <div class="w-full bg-white rounded-lg shadow p-6">
        <h1 class="font-medium text-xl mb-6">Criar usuário</h1>

        <v-form ref="form">
          <v-container>
            <span>E-mail:</span>
            <v-text-field
              v-model="email"
              outlined
              dense
              :rules="[rules.required, rules.validEmail]"
            ></v-text-field>

            <div class="grid grid-cols-2 gap-4">
              <div class="col-span-1">
                <span>Nome:</span>
                <v-text-field
                  v-model="first_name"
                  outlined
                  dense
                  :rules="[rules.required]"
                ></v-text-field>
              </div>
              <div>
                <span>Sobrenome:</span>
                <v-text-field
                  v-model="last_name"
                  outlined
                  dense
                  :rules="[rules.required]"
                ></v-text-field>
              </div>
            </div>

            <div class="grid grid-cols-2 gap-4">
              <div class="col-span-1">
                <span>Senha:</span>
                <v-text-field
                  v-model="password"
                  outlined
                  dense
                  :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="showPassword ? 'text' : 'password'"
                  @click:append="showPassword = !showPassword"
                  :rules="[rules.required]"
                ></v-text-field>
              </div>
              <div>
                <span>Confirme sua senha:</span>
                <v-text-field
                  v-model="confirmPassword"
                  outlined
                  dense
                  :append-icon="showConfirmPassword ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="showConfirmPassword ? 'text' : 'password'"
                  @click:append="showConfirmPassword = !showConfirmPassword"
                  :rules="[rules.required, rules.passwordMatch]"
                ></v-text-field>
              </div>
            </div>

            <div class="flex flex-col password-rules">
              <span :class="passwordStatus(passwordLengthValid)">
                <v-icon small :color="iconColor(passwordLengthValid)">{{
                  getIcon(passwordLengthValid)
                }}</v-icon>
                Mínimo de 6 caracteres.
              </span>
              <span :class="passwordStatus(passwordUppercaseValid)">
                <v-icon small :color="iconColor(passwordUppercaseValid)">{{
                  getIcon(passwordUppercaseValid)
                }}</v-icon>
                Contém pelo menos uma letra maiúscula.
              </span>
              <span :class="passwordStatus(passwordLowercaseValid)">
                <v-icon small :color="iconColor(passwordLowercaseValid)">{{
                  getIcon(passwordLowercaseValid)
                }}</v-icon>
                Contém pelo menos uma letra minúscula.
              </span>
              <span :class="passwordStatus(passwordSpecialCharValid)">
                <v-icon small :color="iconColor(passwordSpecialCharValid)">{{
                  getIcon(passwordSpecialCharValid)
                }}</v-icon>
                Contém pelo menos um caractere especial.
              </span>
            </div>
            <div class="flex justify-end mt-6">
              <v-btn
                class="mr-4 white--text"
                color="#042d65"
                :disabled="!isFormValid"
                >Salvar</v-btn
              >
              <v-btn color="error" @click="onCancel">Cancelar</v-btn>
            </div>
          </v-container>
        </v-form>
      </div>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      confirmPassword: '',
      first_name: '',
      last_name: '',

      search: '',
      dialog: false,
      showPassword: false,
      showConfirmPassword: false,
      rules: {
        required: (value) => !!value || 'Campo obrigatório.',
        passwordMatch: () =>
          this.password === this.confirmPassword ||
          'As senhas não correspondem.',
        validEmail: (value) => {
          const pattern = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i
          return pattern.test(value) || 'Insira um e-mail válido'
        },
      },
    }
  },

  methods: {
    passwordStatus(isValid) {
      if (!this.password) return ''
      return isValid ? 'valid' : 'started'
    },
    iconColor(isValid) {
      if (!this.password) return 'black'
      return isValid ? 'green' : 'red'
    },
    getIcon(isValid) {
      if (!this.password) return '' // Adicione esta linha
      return isValid ? 'mdi-check' : 'mdi-close'
    },

    onCancel() {
      this.dialog = false
      this.email = ''
      this.password = ''
      this.confirmPassword = ''
      this.$refs.form.reset()
    },

    async createUser() {
      try {
        await this.$axios.$post('/users', {
          email: this.email,
          first_name: this.first_name,
          last_name: this.last_name,
          password: this.password,
          confirmPassword: this.confirmPassword,
        })

        this.$toast.success('Usuário cadastrado com sucesso')
        this.onCancel()
      } catch (error) {
        if (error.response && error.response.data) {
          const { data } = error.response
          this.$toast.error(data.message, { position: 'top-center' })
        } else {
          console.error('Erro de resposta:', error)
        }
      }
    },
  },

  computed: {
    passwordLengthValid() {
      return this.password && this.password.length >= 6
    },
    passwordUppercaseValid() {
      return /[A-Z]/.test(this.password)
    },
    passwordLowercaseValid() {
      return /[a-z]/.test(this.password)
    },
    passwordSpecialCharValid() {
      return /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]+/.test(this.password)
    },

    isFormValid() {
      const isEmailValid = this.rules.validEmail(this.email) === true
      const isValid =
        this.passwordLengthValid &&
        this.passwordUppercaseValid &&
        this.passwordLowercaseValid &&
        this.passwordSpecialCharValid &&
        this.password === this.confirmPassword &&
        isEmailValid &&
        this.email.length > 0 &&
        this.first_name.length > 0 &&
        this.last_name.length > 0

      return isValid
    },
  },
}
</script>

<style>
.password-rules span {
  color: black;
  font-size: 12px;
  display: flex;
  align-items: center;
}

.password-rules .started {
  color: red;
}

.password-rules .valid {
  color: green;
}

.password-rules v-icon {
  margin-right: 5px;
}
</style>
