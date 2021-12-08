<template>
    <v-main>
        <v-card
    class="mx-auto"
    max-width="344"
    elevation="2"
    outlined
    >
    <v-list-item three-line>
      <v-list-item-content>
        <v-avatar size="50px">
            <img src="https://cdn.discordapp.com/attachments/864766942404804620/917617052204826624/unknown.png" alt="userProfile">
        </v-avatar>
        <div class="text-overline mb-4">
          User Profile
        </div>
        <v-list-item-title class="text-h5 mb-1">
          {{ nama }}
        </v-list-item-title>
        <v-list-item-subtitle>{{ email }}</v-list-item-subtitle>
      </v-list-item-content>

      <!-- <v-list-item-avatar
        tile
        size="80"
        color="grey"
      ></v-list-item-avatar> -->
    </v-list-item>

    <v-card-actions>
      <v-btn
        outlined
        rounded
        text
        class="edit"
        @click="edit"
      >
        Edit
      </v-btn>
    </v-card-actions>
  </v-card>

  <v-dialog v-model="dialogConfirm" persistent max-width="400px">
      <v-card>
        <v-card-title>
          <span class="headline">Edit Profile</span>
        </v-card-title>
        <v-card-text>
            <v-container>
                <v-text-field v-model="nama" label="Name" required></v-text-field>
                <v-text-field v-model="email" label="Email" required></v-text-field>
            </v-container>
        </v-card-text>
        <v-card-action>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialogConfirm = false">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="update">Confirm</v-btn>
        </v-card-action>
      </v-card>
    </v-dialog>
    </v-main>
</template>

<script>
export default {
    data() {
        return {
            dialogConfirm: false,
            nama: localStorage.getItem("loginName"),
            email: localStorage.getItem("loginEmail"),
        }
    },
    methods: {
        edit() {
            this.dialogConfirm = true;

        },
        update() {
        let newData = {
        name: this.nama,
        email: this.email,
      };

      var url = this.$api + "/profile/" + localStorage.getItem('id');
      this.load = true;
      this.$http
        .put(url, newData, {
          headers: {
            'Authorization': "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.error_message = response.data.message;
          this.color = "green";
          this.snackbar = true;
          this.load = false;
          localStorage.setItem("loginName", newData.name);
          localStorage.setItem("loginEmail", newData.email);
          this.dialogConfirm = false;
        })
        .catch((error) => {
          this.error_message = error.response.data.message;
          this.color = "red";
          this.snackbar = true;
          this.load = false;
        });
    },
    }
}
</script>

<style>
.edit {
    margin-left: 40%;
}
</style>