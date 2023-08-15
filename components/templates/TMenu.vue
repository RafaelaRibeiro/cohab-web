<template>
  <div>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant.sync="mini"
      mini-variant-width="70"
      permanent
      app
    >
      <v-card
        flat
        height="10vh"
        class="d-flex justify-center my-4 mx-2"
        style="position: relative"
      >
        <transition name="fade">
          <v-img
            :key="imageSource"
            :src="imageSource"
            contain
            alt="Logo COHAB"
          ></v-img>
        </transition>
        <!-- <v-btn class="mt-4" v-if="!mini" icon @click.stop="mini = !mini">
          <v-icon>mdi-chevron-left</v-icon>
        </v-btn> -->
      </v-card>

      <v-divider class="ma-0"></v-divider>
      <el-menu
        style="padding-left: 0"
        router
        :collapse="mini"
        active-text-color=" #323288"
        class="py-2"
      >
        <el-menu-item
          v-for="item in items"
          :key="item.id"
          :index="item.id"
          :route="item.to"
        >
          <v-icon class="pr-6">{{ item.icon }} </v-icon>
          <span slot="title">{{ item.title }}</span>
        </el-menu-item>
        <el-menu-item>
          <v-icon class="pr-6"> mdi-logout </v-icon>
          <span slot="title">{{ exit }}</span>
        </el-menu-item>
      </el-menu>
    </v-navigation-drawer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      drawer: true,
      mini: true,

      exit: 'Sair',
      items: [
        {
          id: '1',
          icon: 'mdi-home',
          title: 'Início',
          to: '/',
        },
        {
          id: '2',
          icon: 'mdi-magnify',
          title: 'Pesquisa',
          to: '/search',
        },

        {
          id: '3',
          icon: 'mdi-tray-arrow-up',
          title: 'Importação',
          to: '/',
        },

        {
          id: '4',
          icon: 'mdi-monitor-dashboard',
          title: 'Dashboard',
          to: '/',
          subItems: [{ id: '1-1', icon: 'mdi-account-cash', title: 'Coleta' }],
        },
        {
          id: '5',
          icon: 'mdi-file-chart-outline',
          title: 'Relatórios',
          to: '/',
        },
      ],
    }
  },
  computed: {
    imageSource() {
      return this.mini
        ? require('@/assets/icon.png')
        : require('@/assets/logo.png')
    },
  },
  methods: {
    toggleMini() {
      this.mini = !this.mini
    },
  },
}
</script>

<style>
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.5s;
}

.slide-enter,
.slide-leave-to {
  transform: translateX(-100%);
}
</style>
