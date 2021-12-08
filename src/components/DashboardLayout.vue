<template>
  <div class="dashboard">
    <v-navigation-drawer v-model="drawer" class="fullheight" width="256" app dark>
      <v-list-item>
        <v-list-item-content>
          <v-avatar size="80"><img src="https://cdn.discordapp.com/attachments/864766942404804620/917618015716114512/unknown.png" alt="logo waroong"/></v-avatar>
          <v-list-item-title class="title"> War<span style="color: #FFD700;">oo</span>ng </v-list-item-title>
          <v-list-item-subtitle>Kelompok 6</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
      <v-divider></v-divider>
      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" link tag="router-link" :to="item.to" class="text-left">
          <v-icon v-text="item.icon"></v-icon>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" class="ml-3"></v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app fixed height="75px" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"><v-icon>mdi-menu</v-icon></v-app-bar-nav-icon>
      <a class="navbar-brand" style="color: #FFFFFF">
					War<span style="color: #FFD700;">oo</span>ng
				</a>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn text router><v-icon @click="logout">mdi-logout-variant</v-icon></v-btn>
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
        { title: "Dashboard", to: "/dashboard" , icon: 'mdi-view-dashboard'},
        { title: "Menu", to: "/menu" , icon: 'mdi-food'},
        { title: "Transaksi", to: "/transaction", icon: 'mdi-cash-multiple'},
        { title: "Pegawai", to: "/pegawai", icon: 'mdi-account-box-outline'},
        { title: "Profile", to: "/profile", icon: 'mdi-face-profile'},
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
