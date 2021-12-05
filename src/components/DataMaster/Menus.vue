<template>
  <v-main class="list">
    <h3 class="text-h3" font-weight-medium mb-5>Menu</h3>

    <v-card>
      <v-card-title>
        <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details> </v-text-field>

        <v-spacer></v-spacer>

        <v-btn color="success" dark @click="dialog = true">Tambah</v-btn>
      </v-card-title>
      <v-data-table :headers="headers" :items="menus" :search="search">
        <template v-slot:[`item.actions`]="{ item }">
          <v-btn small class="mr-2" @click="editHandler(item)">edit</v-btn>
          <v-btn small @click="deleteHandler(item.id)">delete</v-btn>
        </template>
      </v-data-table>
    </v-card>

    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">{{ formTitle }} Menu</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-text-field v-model="form.nama_menu" label="Nama Menu" required></v-text-field>
            <v-text-field v-model="form.deskripsi" label="Deskripsi" required></v-text-field>
            <v-text-field v-model="form.harga" label="Harga" required></v-text-field>
            <!-- <v-text-field v-model="form.kategori" label="Kategori" required></v-text-field> -->
            <!-- <v-select :items="['Makanan', 'Minuman']" label="Kategori" required v-model="kategori">
              <template v-slot:item="{ item, attrs, on }">
                <v-list-item v-bind="attrs" v-on="on">
                  <v-list-item-title :id="attrs['aria-labelledby']" v-text="item"></v-list-item-title>
                </v-list-item>
              </template>
            </v-select> -->
            <v-select filled rounded append-icon="mdi-shape" v-model="kategori" :items="items" item-text="name" item-value="id" label="Pilih Kategori" persistent-hint return-object single-line></v-select>
          </v-container>
        </v-card-text>
        <v-card-action>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="cancel">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="setForm">Save</v-btn>
        </v-card-action>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogConfirm" persistent max-width="400px">
      <v-card>
        <v-card-title>
          <span class="headline">Warning!</span>
        </v-card-title>
        <v-card-text> Anda Yakin ingin menghapus menu ini? </v-card-text>
        <v-card-action>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialogConfirm = false">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="deleteData">Delete</v-btn>
        </v-card-action>
      </v-card>
    </v-dialog>
    <v-snackbar v-model="snackbar" :color="color" timeout="2000" bottom>{{ error_message }}</v-snackbar>
  </v-main>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      kategori: "",
      inputType: "Tambah",
      load: false,
      snackbar: false,
      error_message: "",
      color: "",
      search: null,
      dialog: false,
      dialogConfirm: false,
      name: [],
      items: [{ name: "Makanan" }, { name: "Minuman" }],
      headers: [
        {
          text: "Nama Menu",
          align: "start",
          sortable: true,
          value: "nama_menu",
        },

        { text: "Deskripsi", value: "deskripsi" },
        { text: "Kategori", value: "kategori" },
        { text: "Harga", value: "harga" },
        { text: "Actions", value: "actions" },
      ],
      menu: new FormData(),
      menus: [],
      form: {
        nama_menu: null,
        deskripsi: null,
        harga: null,
        kategori: null,
      },
      deleteId: "",
      editId: "",
    };
  },
  methods: {
    setForm() {
      if (this.inputType !== "Tambah") {
        this.update();
      } else {
        this.save();
      }
    },
    // Read Data Menus
    readData() {
      var url = this.$api + "/menu";
      this.$http
        .get(url, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.menus = response.data.data;
        });
    },
    //Simpan data Menu
    save() {
      this.menu.append("nama_menu", this.form.nama_menu);
      this.menu.append("deskripsi", this.form.deskripsi);
      this.menu.append("harga", this.form.harga);
      this.menu.append("kategori", this.kategori.name);

      var url = this.$api + "/menu/";
      this.load = true;
      this.$http
        .post(url, this.menu, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.error_message = response.data.message;
          this.color = "green";
          this.snackbar = true;
          this.load = true;
          this.close();
          this.readData(); // Baca Data
          this.resetForm();
        })
        .catch((error) => {
          this.error_message = error.response.data.message;
          this.color = "red";
          this.snackbar = true;
          this.load = false;
        });
    },
    // Ubah data Menu
    update() {
      let newData = {
        nama_menu: this.form.nama_menu,
        deskripsi: this.form.deskripsi,
        harga: this.form.harga,
        kategori: this.form.kategori,
      };

      var url = this.$api + "/menu/" + this.editId;
      this.load = true;
      this.$http
        .put(url, newData, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.error_message = response.data.message;
          this.color = "green";
          this.snackbar = true;
          this.load = false;
          this.close();
          this.readData();
          this.resetForm();
          this.inputType = "Tambah";
        })
        .catch((error) => {
          this.error_message = error.response.data.message;
          this.color = "red";
          this.snackbar = true;
          this.load = false;
        });
    },
    // Hapus data Produk
    deleteData() {
      var url = this.$api + "/menu/" + this.deleteId;
      this.load = true;
      this.$http
        .delete(url, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.error_message = response.data.message;
          this.color = "green";
          this.snackbar = true;
          this.load = false;
          this.close();
          this.readData();
          this.resetForm();
          this.inputType = "Tambah";
        })
        .catch((error) => {
          this.error_message = error.response.data.message;
          this.color = "red";
          this.snackbar = true;
          this.load = false;
        });
    },
    editHandler(item) {
      this.inputType = "Ubah";
      this.editId = item.id;
      this.form.nama_menu = item.nama_menu;
      this.form.deskripsi = item.deskripsi;
      this.form.harga = item.harga;
      this.form.kategori = item.kategori;
      this.dialog = true;
    },
    deleteHandler(id) {
      this.deleteId = id;
      this.dialogConfirm = true;
    },
    close() {
      this.dialog = false;
      this.inputType = "Tambah";
      this.dialogConfirm = false;
      this.readData();
    },
    cancel() {
      this.resetForm();
      this.readData();
      this.dialog = false;
      this.dialogConfirm = false;
      this.inputType = "Tambah";
    },
    resetForm() {
      this.form = { nama_menu: null, deskripsi: null, harga: null, kategori: null };
    },
  },
  computed: {
    formTitle() {
      return this.inputType;
    },
  },
  mounted() {
    this.readData();
  },
};
</script>
