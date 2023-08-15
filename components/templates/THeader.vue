<template>
  <header class="h-20 clipped-right">
    <nav class="px-2 h-full">
      <div class="grid grid-cols-12 h-full">
        <div class="col-span-2 flex items-center">
          <img src="~/assets/logo.png" alt="" class="w-44" />
        </div>
        <div class="col-span-7">
          <ul
            class="hidden md:flex md:items-center md:space-x-4 w-full h-full justify-start"
          >
            <li
              class="text-base hover:text-gray-30"
              v-for="item in items"
              :key="item.id"
            >
              <v-btn color="#323288" text link :to="item.to">
                <span class="font-semibold text-sm"> {{ item.title }}</span>
              </v-btn>
            </li>
          </ul>
        </div>
        <div class="col-start-10 col-span-3 flex justify-end items-center mr-5">
          <span
            class="mr-2 uppercase text-sm text-default leading-normal font-semibold tracking-wider whitespace-nowrap"
            >João da Silva</span
          >
          <img
            class="w-12 h-12 rounded-full mr-4"
            src="../../assets/avatar.png"
            alt="Avatar "
          />
          <v-menu transition="scale-transition" bottom>
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="#323288" icon v-bind="attrs" v-on="on">
                <v-icon>mdi-menu-down</v-icon>
              </v-btn>
            </template>

            <v-list>
              <v-list-item>
                <v-list-item-title>
                  <v-icon left>mdi-account</v-icon>
                  Minha Conta</v-list-item-title
                >
              </v-list-item>
              <v-list-item>
                <v-list-item-title>
                  <v-icon left>mdi-logout</v-icon>
                  Sair</v-list-item-title
                >
              </v-list-item>
            </v-list>
          </v-menu>
          <!-- <div class="mx-4">
            <v-badge color="error" overlap content="6">
              <v-btn dark icon> <v-icon dark>mdi-bell</v-icon></v-btn>
            </v-badge>
          </div> -->
        </div>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  data() {
    return {
      menu: [
        { title: 'Minha Conta', icon: 'mdi-account' },
        { title: 'Sair', icon: 'mdi-logout' },
      ],
      items: [
        {
          id: '1',
          icon: 'mdi-monitor-dashboard',
          title: 'Dashboard',
          to: '/dashboard',
          subItems: [{ id: '1-1', icon: 'mdi-account-cash', title: 'Coleta' }],
        },
        {
          id: '2',
          icon: 'mdi-home',
          title: 'Pesquisar',
          to: '/search',
        },

        {
          id: '3',
          icon: 'mdi-tray-arrow-up',
          title: 'Importação',
          to: '/upload',
        },

        {
          id: '4',
          icon: 'mdi-file-chart-outline',
          title: 'Relatórios',
          to: '/reports',
        },

        {
          id: '5',
          icon: 'mdi-file-chart-outline',
          title: 'Configurações',
          to: '/settings',
        },
      ],
    }
  },
}
</script>

<style></style>
