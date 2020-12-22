<template>
            <v-data-table
            :headers="headers"
            :items="usuarios"
            sort-by="id"
            class="elevation-1"
            :loading="cargando"
            loading-text="Cargando... Por Favor Espere"
            >
                <template v-slot:top>
                    <v-toolbar
                    flat
                    >
                        <v-toolbar-title>Usuarios</v-toolbar-title>
                        <v-divider
                        class="mx-4"
                        inset
                        vertical
                        ></v-divider>
                        <v-spacer></v-spacer>
                        <v-dialog
                        v-model="dialog"
                        max-width="500px"
                        >
                            <template v-slot:activator="{ on, attrs }">
                                <v-btn
                                color="primary"
                                dark
                                class="mb-2"
                                v-bind="attrs"
                                v-on="on"
                                >
                                Nueva Artículo
                                </v-btn>
                            </template>
                            <v-card>
                                <v-card-title>
                                    <span class="headline">{{ formTitle }}</span>
                                </v-card-title>
                                <v-card-text>
                                    <v-container>
                                        <v-row>
                                            <!-- <v-col
                                            cols="12"
                                            >
                                                <v-text-field
                                                v-model="editedItem.id"
                                                label="Id"
                                                ></v-text-field>
                                            </v-col> -->
                                            <v-col
                                            cols="12"
                                            md="6"
                                            sm="6"
                                            >
                                                <v-text-field
                                                v-model="editedItem.nombre"
                                                label="Nombré"
                                                ></v-text-field>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            md="6"
                                            sm="6"
                                            >
                                            <v-text-field
                                                v-model="editedItem.apellido"
                                                label="Apellido"
                                                ></v-text-field>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            >
                                                <v-text-field
                                                v-model="editedItem.email"
                                                label="E-mail"
                                                ></v-text-field>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            v-if="editedIndex == -1"
                                            >
                                                <v-text-field
                                                v-model="editedItem.password"
                                                label="Password"
                                                type="password"
                                                ></v-text-field>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            v-if="editedIndex>-1"
                                            >
                                                <v-select
                                                v-model="editedItem.rol"
                                                label="Rol"
                                                :items="rol"
                                                item-text="rol"
                                                item-value="value"
                                                return-object
                                                ></v-select>
                                            </v-col>
                                        </v-row>
                                    </v-container>
                                </v-card-text>
                                <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn
                                    color="blue darken-1"
                                    text
                                    @click="close"
                                    >
                                    Cancelar
                                    </v-btn>
                                    <v-btn
                                    color="blue darken-1"
                                    text
                                    @click="save"
                                    >
                                    Guardar
                                    </v-btn>
                                </v-card-actions>
                            </v-card>
                        </v-dialog>
                        <v-dialog v-model="dialogDelete" max-width="500px">
                            <v-card>
                                <v-card-title class="headline">Seguro que quieres cambiar el estado ?</v-card-title>
                                <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="blue darken-1" text @click="closeDelete">Cancelar</v-btn>
                                    <v-btn color="blue darken-1" text @click="deleteItemConfirm">Aceptar</v-btn>
                                    <v-spacer></v-spacer>
                                </v-card-actions>
                            </v-card>
                        </v-dialog>
                    </v-toolbar>
                </template>
                <template v-slot:[`item.actions`]="{ item }">
                    <v-icon
                    medium
                    class="mr-2"
                    @click="editItem(item)"
                    >
                    mdi-pencil
                    </v-icon>
                    <v-icon 
                    v-if="item.estado"
                    medium
                    @click="deleteItem(item)"
                    >
                    mdi-toggle-switch
                    </v-icon>
                    <v-icon 
                    v-else
                    medium
                    @click="deleteItem(item)"
                    >
                    mdi-toggle-switch-off-outline              
                    </v-icon>
                </template>
                <template v-slot:[`item.estado`]="{ item }">
                    <div v-if="item.estado">
                        <span class="green--text">Activo</span>
                    </div>
                    <div v-else>
                        <span class="red--text">Inactivo</span>
                    </div>
                </template>
                <template v-slot:no-data>
                    <v-btn
                    color="primary"
                    @click="list"
                    >
                    Reset
                    </v-btn>
                </template>
            </v-data-table>
</template>

<script>
import axios from "axios";
    export default {
        name: "ComponentUsuarioTable",
        data: () => ({
            dialog: false,
            dialogDelete: false,
            cargando: true,
            headers: [
                { text: 'Id', value: 'id' },
                { text: 'Nombre', value: 'nombre' },
                {text: 'Apellido', value: 'apellido'},
                { text: 'E-mail', value: 'email' },
                { text: 'Rol', value: 'rol' },
                { text: 'Estado', value: 'estado' },
                { text: 'Actions', value: 'actions', sortable: false },
            ],
            rol: [
                { rol: 'Ninguno', value: ""},
                { rol: 'Administrador', value: "Administrador"},
                { rol: 'Almacenero', value: "Almacenero"},
                { rol: 'Vendedor', value: "Vendedor"},
            ],
            usuarios: [],
            editedIndex: -1,
            editedItem: {
                nombre: '',
                apellido: "",
                password: '',
                email: '',
                rol: '',
            },
            defaultItem: {
                nombre: '',
                apellido: "",
                password: '',
                email: '',
                rol: '',
            },
        }),

        computed: {
            formTitle () {
            return this.editedIndex === -1 ? 'Nueva Usuario' : 'Editar Usuario'
            },
        },

        watch: {
            dialog (val) {
            val || this.close()
            },
            dialogDelete (val) {
            val || this.closeDelete()
            },
        },

        created () {
            this.list()
        },

        methods: {
            list () {
                axios
                .get("https://cryptic-tor-85862.herokuapp.com/api/usuario/list", {headers: { "token": this.$store.state.token}},)
                .then(response => {
                    this.usuarios = response.data
                    this.cargando = false
                })
                .catch(error => {
                    return error
                })
            },
            editItem (item) {
            // this.editedIndex = this.categorias.indexOf[indexOf(item)].id
            this.editedIndex = item.id
            this.editedItem = Object.assign({}, item)
            this.dialog = true
            },

            deleteItem (item) {
            this.editedIndex = item.id
            this.editedItem = Object.assign({}, item)
            this.dialogDelete = true
            },

            deleteItemConfirm () {
            if (this.editedItem.estado === 1) {
                // Editar
                axios
                .put("https://cryptic-tor-85862.herokuapp.com/api/usuario/deactivate", {
                    id: this.editedItem.id,
                    },
                    {headers: { "token": this.$store.state.token}})
                .then(response => {
                    this.list()
                })
                .catch(error => {
                    return error
                })
                // Object.assign(this.desserts[this.editedIndex], this.editedItem)
            }else{
                axios
                .put("https://cryptic-tor-85862.herokuapp.com/api/usuario/activate", {
                    id: this.editedItem.id,
                    },{headers: { "token": this.$store.state.token}})
                .then(response => {
                    this.list()
                })
                .catch(error => {
                    return error
                })
            }
            this.closeDelete()
            },

            close () {
            this.dialog = false
            this.$nextTick(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
                this.categoria = ""
                this.editedIndex = -1
            })
            },

            closeDelete () {
            this.dialogDelete = false
            this.$nextTick(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
                this.editedIndex = -1
            })
            },

            save () {
            if (this.editedIndex > -1) {
                // Editar
                axios
                .put("https://cryptic-tor-85862.herokuapp.com/api/usuario/update", {
                    id: this.editedItem.id,
                    nombre: this.editedItem.nombre,
                    apellido: this.editedItem.apellido,
                    email: this.editedItem.email,
                    rol: this.editedItem.rol.value,
                    },{headers: { "token": this.$store.state.token}}
                    )
                .then(response => {
                    this.list()
                })
                .catch(error => {
                    return error
                })
                // Object.assign(this.desserts[this.editedIndex], this.editedItem)
            } else {
                // Agregar
                axios
                .post("https://cryptic-tor-85862.herokuapp.com/api/usuario/add", {
                    id: this.editedItem.id,
                    nombre: this.editedItem.nombre,
                    apellido: this.editedItem.apellido,
                    password: this.editedItem.password,
                    email: this.editedItem.email,
                    },{headers: { "token": this.$store.state.token}}
                    )
                .then(response => {
                    this.list()
                })
                .catch(error => {
                    return error
                })
                // this.categorias.push(this.editedItem)
            }
            this.close()
            },
        },
    }
</script>

<style scoped>

</style>