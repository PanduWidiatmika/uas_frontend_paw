<template>
  <div class="dashboard">
    <v-navigation-drawer v-model="drawer" class="fullheight" width="256" app>
      <v-list-item>
        <v-list-item-content>
          <v-avatar size="80"><img src="https://cdn.discordapp.com/attachments/864766942404804620/917618015716114512/unknown.png" alt="logo waroong"/></v-avatar>
          <v-list-item-title class="title"> Waroong </v-list-item-title>
          <v-list-item-subtitle>Kelompok 6</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
      <v-divider></v-divider>
      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" link tag="router-link" :to="item.to">
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app fixed height="75px">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"><v-icon>mdi-menu</v-icon></v-app-bar-nav-icon>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn text router><v-icon @click="logout">mdi-power</v-icon></v-btn>
      </v-toolbar-items>
    </v-app-bar>
    <div class="fullheight pa-5">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: "Dashboard",
  data() {
    return {
      drawer: true,
      items: [
        { title: "Dashboard", to: "/dashboard" },
        { title: "Menu", to: "/menu" },
        { title: "Transaksi", to: "/transaction"},
        { title: "Pegawai", to: "/pegawai"},
      ],
    };
  },
  methods:{
        logout(){
        localStorage.removeItem('token');
        this.$router.push({
            name: 'Login',
        });
    }
  }
};
</script>

<style scoped>
.fullheight {
  min-height: 100vh !important;
}

.router {
  text-decoration: none;
  color: black;
}
</style>
