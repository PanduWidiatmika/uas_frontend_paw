<template>
  <main>
    <v-container fluid fill-height class="posisinya">
      <v-layout flex align-center justify-center>
        <v-flex xs12 sm6 elevation-6>
          <v-toolbar class="grey lighten-2">
            <v-toolbar-title class="grey--text">
              <h1>Register</h1>
            </v-toolbar-title>
          </v-toolbar>
          <v-card>
            <v-card-text class="pt-4">
              <div>
                <v-form v-model="valid" ref="form">
                  <v-text-field label="Name" v-model="name" required></v-text-field>
                  <v-text-field label="E-mail" v-model="email" :rules="emailRules" required></v-text-field>
                  <v-text-field label="Password" v-model="password" type="password" min="8" :rules="passwordRules" counter required></v-text-field>
                  <v-layout justify-end>
                    <v-btn @click="login" class="pink darken-3 white--text ">Login</v-btn>
                    <v-spacer></v-spacer>
                    <v-btn class="mr-2" @click="submit" :class="{ 'blue darken-1 white--text': valid, disabled: !valid }">Go</v-btn>
                    <v-btn @click="clear" class="grey darken-3 white--text">Clear</v-btn>
                  </v-layout>
                </v-form>
              </div>
            </v-card-text>
          </v-card>
          <v-snackbar v-model="snackbar" :color="color" timeout="2000" bottom>{{ error_message }}</v-snackbar>
        </v-flex>
      </v-layout>
    </v-container>
  </main>
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
</style>

<script>
export default {
  name: "Register",
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
      name: "",
    };
  },

  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        // Cek Validasi Data Yang Terkirim
        this.load = true;
        this.$http
          .post(this.$api + "/register", {
            name: this.name,
            email: this.email,
            password: this.password,
          })
          .then((response) => {
            // Simpan Data ID User Yang Diinput
            this.error_message = response.data.message;
            this.color = "green";
            this.snackbar = true;
            this.load = false;
            this.clear();
            this.$router.push({
              name: "Login",
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

    login() {
      this.$router.push({
        name: "Login",
      })
    },

    clear() {
      this.$refs.form.reset(); // Clear Form Login
    },
  },
};
</script>
