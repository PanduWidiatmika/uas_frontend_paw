<template>
  <v-main class="list">
    <h3 class="text-h3" font-weight-medium mb-5>Transaksi</h3>

    <v-card>
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-cash-multiple"
          label="Search"
          single-line
          hide-details>
        </v-text-field>

        <v-spacer></v-spacer>

        <v-btn color="success" dark @click="dialog = true"> Tambah </v-btn>
      
      </v-card-title>
      <v-data-table :headers="headers" :items="transactions" :search="search">
        <template v-slot:[`item.actions`]="{ item }">
          <v-btn small class="mr-2" @click="editHandler(item)">edit</v-btn>
          <v-btn small @click="deleteHandler(item.id)">delete</v-btn>
        </template>
      </v-data-table>
    </v-card>

    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">{{ formTitle }} Transaksi</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <b-form-datepicker id="datepicker" v-model="form.tanggal" class="mb-2" :max="max" size="sm" placeholder="Pilih Tanggal"></b-form-datepicker>
            <p></p>
            <v-text-field v-model="form.waktu" label="Waktu" required hint="Format HH:ii (contoh 09:00, 13:00)" persistent-hint></v-text-field>
            <v-text-field v-model="form.biaya" label="Biaya" required></v-text-field>
            <v-select v-model="form.metode_pembayaran" :items=itemsMetodePembayaran label="Metode Pembayaran" required></v-select>
            <v-text-field v-model="form.note" label="Note" required hint="Opsional" persistent-hint></v-text-field>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="cancel">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="setForm">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogConfirm" persistent max-width="400px">
      <v-card>
        <v-card-title>
          <span class="headline">Warning!</span>
        </v-card-title>
        <v-card-text>Anda yakin ingin menghapus transaksi ini?</v-card-text>
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
// import Datepicker from 'vuejs-datepicker'

export default {
  name: "List",
  components: {
      // Datepicker
  },

  data() {
    const nowDate = new Date();
    // const nowTime = nowDate.getHours() + ":" + nowDate.getMinutes() + ":" + nowDate.getSeconds();

    return {
      inputType: 'Tambah',
      load: false,
      snackbar: false,
      error_message: '',
      color: '',
      search: null,
      dialog: false,
      dialogConfirm: false,
      itemsMetodePembayaran: [ "Tunai" , "Debit" ],
      headers: [
        { text: "Tanggal", align: "start", sortable: true, value: "tanggal"},
        { text: "Waktu", value: "waktu" },
        { text: "Biaya", value: "biaya" },
        { text: "Metode Pembayaran", value: "metode_pembayaran" },
        { text: "Note", value: "note" },
        { text: "Actions", value: "actions"},
      ],
      transaction: new FormData,
      transactions: [],
      form: {
        tanggal: null,
        waktu: null,
        biaya: null,
        metode_pembayaran: null,
        note: null,
      },
      deleteId: '',
      editId: '',
      // currentTime: {
      //   tanggal: nowDate,
      //   waktu: nowTime,
      // },
      max: nowDate
    };
  },

  methods: {
    setForm(){
      if(this.inputType !== 'Tambah'){
        this.update();
      }else{
        this.save();
      }
    },

    //Read Data Transactions
    readData(){
      var url = this.$api + '/transaction';
      this.$http.get(url, {
        headers: {
          'Authorization' : 'Bearer ' + localStorage.getItem('token')
        }
      }).then(response => {
        this.transactions = response.data.data;
      })
    },

    //Simpan data transaction
    save() {
      this.transaction.append('tanggal', this.form.tanggal);
      this.transaction.append('waktu', this.form.waktu);
      // this.transaction.append('tanggal', this.currentTime.tanggal);
      // this.transaction.append('waktu', this.currentTime.waktu);
      // this.transaction.append('tanggal', getCurrentDate());
      // this.transaction.append('waktu', getCurrentTime());
      this.transaction.append('biaya', this.form.biaya);
      this.transaction.append('metode_pembayaran', this.form.metode_pembayaran);
      if(this.form.note == null) {
        this.transaction.append('note', "-");
      } else {
        this.transaction.append('note', this.form.note);
      }

      var url = this.$api + '/transaction'
      this.load = true;
      this.$http.post(url, this.transaction, {
        headers: {
          'Authorization' : 'Bearer ' + localStorage.getItem('token'),
        }
      }).then(response => {
        this.error_message = response.data.message;
        this.color = "green";
        this.snackbar = true;
        this.load = true;
        this.close();
        this.readData();
        this.resetForm();
      }).catch(error => {
        this.error_message = error.response.data.message;
        this.color = "red";
        this.snackbar = true;
        this.load = false;
      });
    },

    //Ubah data transaction
    update() {
      let newData = {
        tanggal : this.form.tanggal,
        waktu : this.form.waktu,
        biaya : this.form.biaya,
        metode_pembayaran : this.form.metode_pembayaran,
        note : this.form.note
      };
      var url = this.$api + '/transaction/' + this.editId;
      this.load = true;
      this.$http.put(url, newData, {
        headers: {
          'Authorization' : 'Bearer ' + localStorage.getItem('token')
        }
      }).then(response => {
        this.error_message = response.data.message;
        this.color = 'green';
        this.snackbar = true;
        this.load = false;
        this.close();
        this.readData(); //baca data
        this.resetForm();
        this.inputType = 'Tambah';
      }).catch(error => {
        this.error_message = error.response.data.message;
        this.color = 'red';
        this.snackbar = true;
        this.load = false;
      });
    },

    //Hapus data transaction
    deleteData() {
      //menghapus data
      var url = this.$api + '/transaction/' + this.deleteId;
      this.load = true;
      this.$http.delete(url, {
        headers: {
          'Authorization' : 'Bearer ' + localStorage.getItem('token')
        }
      }).then(response => {
        this.error_message = response.data.message;
        this.color = "green";
        this.snackbar = true;
        this.load = false;
        this.close();
        this.readData(); //baca data
        this.resetForm();
        this.inputType = "Tambah";
      }).catch(error => {
        this.error_message = error.response.data.message;
        this.color = "red";
        this.snackbar = true;
        this.load = false;
      });
    },

    editHandler(item) {
      this.inputType = 'Ubah';
      this.editId = item.id;
      this.form.tanggal = item.tanggal;
      this.form.waktu = item.waktu;
      this.form.biaya = item.biaya;
      this.form.metode_pembayaran = item.metode_pembayaran;
      this.form.note = item.note;
      this.dialog = true;
    },

    deleteHandler(id) {
      this.deleteId = id;
      this.dialogConfirm = true;
    },

    close() {
      this.dialog = false;
      this.inputType = 'Tambah';
      this.dialogConfirm = false;
      this.readData();
    },

    cancel() {
      this.resetForm();
      this.readData();
      this.dialog = false;
      this.dialogConfirm = false;
      this.inputType = 'Tambah';
    },

    resetForm() {
      this.form = {
        tanggal: null,
        waktu: null,
        biaya: null,
        metode_pembayaran: null,
        note: null,
      };
    },

    // getCurrentDate() {
    //   return new Date().toLocaleDateString();
    // },

    // getCurrentTime() {
    //   return new Date().toLocaleTimeString();
    // }
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