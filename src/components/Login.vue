<template>
  <v-main class="background">
    <v-container fluid fill-height class="posisinya">
      <v-layout flex align-center justify-center>
        <v-flex xs12 sm6 elevation-6>
          <v-toolbar class="grey lighten-2">
            <v-toolbar-title class="grey--text">
              <h1>Login</h1>
            </v-toolbar-title>
          </v-toolbar>
          <v-card>
            <v-card-text class="pt-4">
              <div>
                <v-form v-model="valid" ref="form">
                  <v-text-field label="E-mail" v-model="email" :rules="emailRules" required></v-text-field>
                  <v-text-field label="Password" v-model="password" type="password" min="8" :rules="passwordRules" counter required></v-text-field>
                  <v-layout justify-end>
                    <v-btn @click="register" class="pink darken-3 white--text ">Register</v-btn>
                    <v-spacer></v-spacer>
                    <v-btn class="mr-2" @click="submit" :class="{ 'blue darken-1 white--text': valid, disabled: !valid }">Go</v-btn>
                    <v-btn @click="clear" class="grey darken-3 white--text">Clear</v-btn>
                  </v-layout>
                </v-form>
              </div>
            </v-card-text>
          </v-card>
          <v-snackbar v-model="snackbar" :color="color" timeout="5000" bottom>{{ error_message }}</v-snackbar>
        </v-flex>
      </v-layout>
    </v-container>
  </v-main>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@300;500&family=Playfair+Display&display=swap");
.grey--text {
  font-family: "Hind Siliguri";
}

.posisinya {
  position: absolute;
  top: 20px;
  left: 0;
  right: 0;
}

.background {
  background-image: url(https://wallpapercave.com/wp/wp1874156.jpg);
  background-repeat: no-repeat;
  background-size: 100%;
  margin-top: -60px;
}

</style>

<script>
export default {
  name: "Login",
  data() {
    return {
      load: false,
      snackbar: false,
      error_message: "",
      color: "",
      valid: false,
      password: "",
      passwordRules: [(v) => !!v || "Password tidak boleh kosong :("],
      email: "",
      emailRules: [(v) => !!v || "E-mail tidak boleh kosong :("],
    };
  },

  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        // Cek Validasi Data Yang Terkirim
        this.load = true;
        this.$http
          .post(this.$api + "/login", {
            email: this.email,
            password: this.password,
          })
          .then((response) => {
            // Simpan Data ID User Yang Diinput
            localStorage.setItem("id", response.data.user.id);
            localStorage.setItem("loginName", response.data.user.name);
            localStorage.setItem("loginEmail", response.data.user.email);
            localStorage.setItem("token", response.data.access_token);
            this.error_message = response.data.message;
            this.color = "green";
            this.snackbar = true;
            this.load = false;
            this.clear();
            this.$router.push({
              name: "Dashboard",
            });
          })
          .catch((error) => {
            this.error_message = error.response.data.message;
            this.color = "red";
            this.snackbar = true;
            localStorage.removeItem("token");
            this.load = false;
          });
      }
    },

    register() {
      this.$router.push({
        name: "Register",
      })
    },

    clear() {
      this.$refs.form.reset(); // Clear Form Login
    },
  },
};
</script>
