<template>
            <v-data-table
            :headers="headers"
            :items="articulos"
            sort-by="nombre"
            class="elevation-1"
            :loading="cargando"
            loading-text="Cargando... Por Favor Espere"
            >
                <template v-slot:top>
                    <v-toolbar
                    flat
                    >
                        <v-toolbar-title>Artículos</v-toolbar-title>
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
                                            <v-col
                                            cols="12"
                                            >
                                                <v-text-field
                                                v-model="editedItem.imagen"
                                                label="Link imagén"
                                                ></v-text-field>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            md="6"
                                            sm="6"
                                            >
                                                <v-text-field
                                                v-model="editedItem.codigo"
                                                label="Código"
                                                ></v-text-field>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            md="6"
                                            sm="6"
                                            >
                                                <v-select
                                                v-model="categoria"
                                                label="Categoría"
                                                :items="categorias"
                                                item-text="nombre"
                                                item-value="id"
                                                return-object
                                                ></v-select>
                                            </v-col>
                                            <v-col
                                            cols="12"
                                            >
                                                <v-text-field
                                                v-model="editedItem.nombre"
                                                label="Nombre Artículo"
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
                                            <v-col
                                            cols="12"
                                            >
                                                <v-text-field
                                                v-model="editedItem.precio"
                                                label="Precio"
                                                ></v-text-field>
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
    export default {
        name: "ComponentArticuloTable",
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
                { text: 'Código', value: 'codigo' },
                { text: 'Categoría', value: 'categoria.nombre' },
                {
                    text: 'Nombre Artículo',
                    align: 'start',
                    value: 'nombre',
                },
                { text: 'Descripción', value: 'descripcion' },
                { text: 'Precio', value: 'precio' },
                { text: 'Estado', value: 'estado' },
                { text: 'Actions', value: 'actions', sortable: false },
            ],
            articulos: [],
            categorias: [],
            categoria: "",
            editedIndex: -1,
            editedItem: {
                imagen: null,
                codigo: "",
                categoria: {
                    id: 0,
                    nombre: "",
                    estado: 0
                },
                nombre: '',
                descripcion: '',
                precio: 0,
            },
            defaultItem: {
                imagen: null,
                codigo: "",
                categoria: {
                    id: 0,
                    nombre: "",
                    estado: 0
                },
                nombre: '',
                descripcion: '',
                precio: 0,
            },
        }),

        computed: {
            formTitle () {
            return this.editedIndex === -1 ? 'Nueva Artículo' : 'Editar Artículo'
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
            this.listCategoria()
        },

        methods: {
            list () {
                axios
                .get("http://localhost:3000/api/articulo/list")
                .then(response => {
                    this.articulos = response.data
                    this.cargando = false
                })
                .catch(error => {
                    return error
                })
            },
            listCategoria() {
                axios
                .get("http://localhost:3000/api/categoria/list")
                .then(response => {
                    this.categorias = response.data
                })
                .catch(error => {
                    return error
                })
            },
            editItem (item) {
            // this.editedIndex = this.categorias.indexOf[indexOf(item)].id
            this.editedIndex = item.id
            this.categoria = item? item.categoria : ""
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
                .put("http://localhost:3000/api/articulo/deactivate", {
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
                .put("http://localhost:3000/api/articulo/activate", {
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
                .put("http://localhost:3000/api/articulo/update", {
                    id: this.editedItem.id,
                    imagen: this.editedItem.imagen,
                    codigo: this.editedItem.codigo,
                    nombre: this.editedItem.nombre,
                    categoriaId: this.categoria.id,
                    descripcion: this.editedItem.descripcion,
                    precio: this.editedItem.precio,}
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
                .post("http://localhost:3000/api/articulo/add", {
                    imagen: this.editedItem.imagen,
                    codigo: this.editedItem.codigo,
                    nombre: this.editedItem.nombre,
                    categoriaId: this.categoria.id,
                    descripcion: this.editedItem.descripcion,
                    precio: this.editedItem.precio,
                    }
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