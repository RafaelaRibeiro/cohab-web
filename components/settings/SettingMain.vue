<template>
  <div class="mt-3 min-h-[80vh]">
    <div>
      <h1 class="headline font-bold text-gray-700 mb-4">
        <i>
          <v-icon size="30" class="px-3">mdi-cog-outline</v-icon>Configurações
        </i>
      </h1>
    </div>
    <div class="bg-white min-h-[75vh] flex flex-row">
      <aside>
        <el-menu
          style="padding-left: 0"
          active-text-color=" #323288"
          class="py-2"
        >
          <el-menu-item
            v-for="item in items"
            :key="item.id"
            :index="item.id"
            @click.native="setCurrentComponent(item.to)"
          >
            <v-icon class="pr-6">{{ item.icon }} </v-icon>
            <span class="text-base" slot="title">{{ item.title }}</span>
          </el-menu-item>
        </el-menu>
      </aside>
      <main class="bg-white p-4">
        <component :is="currentComponent"></component>
      </main>
    </div>
  </div>
</template>

<script>
import Account from '~/pages/settings/account.vue'
import Permissions from '~/pages/settings/permissions.vue'
import Events from '~/pages/settings/events.vue'
import Users from '~/pages/settings/users.vue'

export default {
  components: {
    Account,
    Permissions,
    Events,
    Users,
  },
  data() {
    return {
      drawer: true,
      currentComponent: 'Account',
      items: [
        {
          id: '1',
          icon: 'mdi-account-box-outline',
          title: 'Minha Conta',
          to: 'Account', // lembre-se que o nome do componente é com letra maiúscula
        },
        {
          id: '2',
          icon: 'mdi-clipboard-list-outline',
          title: 'Eventos',
          to: 'Events', // lembre-se que o nome do componente é com letra maiúscula
        },
        {
          id: '2',
          icon: 'mdi-account-group-outline',
          title: 'Usuários',
          to: 'Users', // lembre-se que o nome do componente é com letra maiúscula
        },
        {
          id: '3',
          icon: 'mdi-shield-key-outline',
          title: 'Controle de Acesso',
          to: 'Permissions', // lembre-se que o nome do componente é com letra maiúscula
        },
      ],
    }
  },
  methods: {
    setCurrentComponent(componentName) {
      this.currentComponent = componentName
    },
  },
}
</script>

<style></style>
