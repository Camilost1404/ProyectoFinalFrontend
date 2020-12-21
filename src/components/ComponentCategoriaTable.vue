<template>
    <div id="app">
        <v-app id="inspire">
            <v-data-table
            :headers="headers"
            :items="categorias"
            sort-by="nombre"
            class="elevation-1"
            :loading="cargando"
            loading-text="Cargando... Por Favor Espere"
            >
                <template v-slot:top>
                    <v-toolbar
                    flat
                    >
                        <v-toolbar-title>Categorías</v-toolbar-title>
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
                                Nueva Categoría
                                </v-btn>
                            </template>
                            <v-card>
                                <v-card-title>
                                    <span class="headline">{{ formTitle }}</span>
                                </v-card-title>
                                <v-card-text>
                                    <v-container>
                                        <v-row>
                                            <v-col
                                            cols="12"
                                            >
                                                <v-text-field
                                                v-model="editedItem.imagen"
                                                label="Subir imagén"
                                                type="file"
                                                ></v-text-field>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            >
                                                <v-text-field
                                                v-model="editedItem.nombre"
                                                label="Nombre Categoría"
                                                ></v-text-field>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            >
                                                <v-textarea
                                                v-model="editedItem.descripcion"
                                                label="Descripción"
                                                counter="254"
                                                no-resize
                                                auto-grow
                                                ></v-textarea>
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
        </v-app>
    </div>
</template>

<script>
    export default {
        name: "ComponentCategoriaTable",
        data: () => ({
            dialog: false,
            dialogDelete: false,
            cargando: true,
            headers: [
                {
                    text: 'Imagén',
                    align: 'start',
                    value: 'imagen',
                    sortable: false
                },
                {
                    text: 'Nombre Categoría',
                    align: 'start',
                    value: 'nombre',
                },
                { text: 'Descripción', value: 'descripcion' },
                { text: 'Estado', value: 'estado' },
                { text: 'Actions', value: 'actions', sortable: false },
            ],
            categorias: [],
            editedIndex: -1,
            editedItem: {
                imagen: null,
                nombre: '',
                descripcion: '',
            },
            defaultItem: {
                imagen: null,
                nombre: '',
                descripcion: '',
            },
        }),

        computed: {
            formTitle () {
            return this.editedIndex === -1 ? 'Nueva Categoría' : 'Editar Categoría'
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
                .get("http://localhost:3000/api/categoria/list")
                .then(response => {
                    this.categorias = response.data
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
                .put("http://localhost:3000/api/categoria/deactivate", {
                    id: this.editedItem.id,
                    })
                .then(response => {
                    this.list()
                })
                .catch(error => {
                    return error
                })
                // Object.assign(this.desserts[this.editedIndex], this.editedItem)
            }else{
                axios
                .put("http://localhost:3000/api/categoria/activate", {
                    id: this.editedItem.id,
                    })
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
                .put("http://localhost:3000/api/categoria/update", {
                    id: this.editedItem.id,
                    imagen: this.editedItem.imagen,
                    nombre: this.editedItem.nombre,
                    descripcion: this.editedItem.descripcion}
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
                .post("http://localhost:3000/api/categoria/add", {
                    imagen: this.editedItem.imagen,
                    nombre: this.editedItem.nombre,
                    descripcion: this.editedItem.descripcion}
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