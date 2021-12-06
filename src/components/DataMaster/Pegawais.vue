<template>
    <v-main class="list">
        <h3 class="text-h3 font-weight-medium mb-5"> Pegawai </h3>

        <v-card>
            <v-card-title>
                <v-text-field
                    v-model="search"
                    append-icon="mdi-magnify"
                    label="Search"
                    single-line
                    hide-details>
                </v-text-field>

                <v-spacer></v-spacer>

                <v-btn color="success" dark @click="dialog = true">Tambah</v-btn>  

            </v-card-title>
            <v-data-table :headers="headers" :items="courses" :search="search">
                <template v-slot:[`item.actions`]="{ item }">
                    <v-btn small class="mr-2" @click="editHandler(item)">edit</v-btn>
                    <v-btn small @click="deleteHandler(item.id)">delete</v-btn>
                </template>
            </v-data-table>
        </v-card>

        <v-dialog v-model="dialog" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline">{{ formTitle }} Pegawai </span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-text-field v-model="form.nama" label="Nama Pegawai" required></v-text-field>
                        <v-text-field v-model="form.nip" label="NIP" required></v-text-field>
                        <v-select v-model="form.jenis_kelamin" :items=itemsJenisKelamin label="Jenis Kelamin" required></v-select>
                        <datepicker v-model="form.tanggal_lahir" format="yyyy-MM-dd" label="Tgl Lahir" required></datepicker>
                        <v-text-field v-model="form.alamat" label="Alamat" required></v-text-field>
                        <v-text-field v-model="form.jabatan" label="Jabatan" required></v-text-field>
                        <datepicker v-model="form.tanggal_masuk" format="yyyy-MM-dd" label="Tgl Masuk" required></datepicker>
                        <v-text-field v-model="form.gaji" label="Gaji" required></v-text-field>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-l" text @click="cancel">Cancel</v-btn>
                    <v-btn color="blue darken-l" text @click="setForm">Save</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

        <v-dialog v-model="dialogConfirm" persistent max-width="400px">
            <v-card>
                <v-card-title>
                    <span class="headline">Warning!</span>
                </v-card-title>
                <v-card-text>Anda Yakin ingin menghapus pegawai ini?</v-card-text>
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
import Datepicker from 'vuejs-datepicker'
    export default {
        name: "List",
        components: {
            Datepicker
        },
        data() {
            return {
                jenis_kelamin: '',
                tanggal_lahir: new Date(),
                tanggal_masuk: new Date(),
                inputType: 'Tambah',
                load: false,
                snackbar: false,
                error_message: '',
                color: '',
                search: null,
                dialog: false,
                dialogConfirm: false,
                itemsJenisKelamin: [ "Laki-laki" , "Perempuan" ],
                headers: [
                    { text: "Nama Pegawai", align: "start", sortable: true, value: "nama"},
                    { text: "NIP", value: "nip"},
                    { text: "Jenis Kelamin", value: "jenis_kelamin"},
                    { text: "Tgl Lahir", value: "tanggal_lahir"},
                    { text: "Alamat", value: "alamat"},
                    { text: "Jabatan", value: "jabatan"},
                    { text: "Tgl Masuk", value: "tanggal_masuk"},
                    { text: "Gaji", value: "gaji"},
                    { text: "Actions", value: "actions"},
                ],
                course: new FormData,
                courses: [],
                form: {
                    nama: null,
                    nip: null,
                    jenis_kelamin: null,
                    tanggal_lahir: null,
                    alamat: null,
                    jabatan: null,
                    tanggal_masuk: null,
                    gaji: null,
                },
                deleteId: '',
                editId: ''
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

            readData(){
                var url = this.$api + '/pegawai';
                this.$http.get(url, {
                    headers: {
                        'Authorization' : 'Bearer ' + localStorage.getItem('token')
                    }
                }).then(response => {
                    this.pegawais = response.data.data;
                })
            },

            save(){
                this.course.append('nama', this.form.nama);
                this.course.append('nip', this.form.nip);
                this.course.append('jenis_kelamin', this.form.jenis_kelamin);
                this.course.append('tanggal_lahir', this.form.tanggal_lahir);
                this.course.append('alamat', this.form.alamat);
                this.course.append('jabatan', this.form.jabatan);
                this.course.append('tanggal_masuk', this.form.tanggal_masuk);
                this.course.append('gaji', this.form.gaji);

                var url = this.$api + '/pegawai/'
                this.load = true;
                this.$http.post(url, this.pegawai, {
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

            update() {
                let newData = {
                    nama : this.form.nama,
                    nip : this.form.nip,
                    jenis_kelamin : this.form.jenis_kelamin,
                    tanggal_lahir : this.form.tanggal_lahir,
                    alamat : this.form.alamat,
                    jabatan : this.form.jabatan,
                    tanggal_masuk : this.form.tanggal_masuk,
                    gaji : this.form.gaji
                };
                var url = this.$api + '/pegawai/' + this.editId;
                this.load = true;
                this.$http.put(url, newData, {
                    headers : {
                        'Authorization' : 'Bearer ' + localStorage.getItem('token')
                    }
                }).then(response => {
                    this.error_message = response.data.message;
                    this.color = "green";
                    this.snackbar = true;
                    this.load = false;
                    this.close();
                    this.readData();
                    this.resetForm();
                    this.inputType = 'Tambah';
                }).catch(error => {
                    this.error_message = error.response.data.message;
                    this.color = "red";
                    this.snackbar = true;
                    this.load = false;
                });
            },

            deleteData() {
                var url = this.$api + '/pegawai/' + this.deleteId;
                this.load = true;
                this.$http.delete(url, {
                    headers : {
                        'Authorization' : 'Bearer ' + localStorage.getItem('token')
                    }
                }).then(response => {
                    this.error_message = response.data.message;
                    this.color = "green";
                    this.snackbar = true;
                    this.load = false;
                    this.close();
                    this.readData();
                    this.resetForm();
                    this.inputType = 'Tambah';
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
                this.form.nama = item.nama;
                this.form.nip = item.nip;
                this.form.jenis_kelamin = item.jenis_kelamin;
                this.form.tanggal_lahir = item.tanggal_lahir;
                this.form.alamat = item.alamat;
                this.form.jabatan = item.jabatan;
                this.form.tanggal_masuk = item.tanggal_masuk;
                this.form.gaji = item.gaji;
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
                    nama: null,
                    nip: null,
                    jenis_kelamin: null,
                    tanggal_lahir: null,
                    alamat: null,
                    jabatan: null,
                    tanggal_masuk: null,
                    gaji: null
                };
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
