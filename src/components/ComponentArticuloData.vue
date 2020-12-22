<template>
    <v-container class="pt-0 px-0">
            <v-row>
                <v-col
                v-for="(articulo, id) in articulos"
                :key="id"
                :class="articulo.estado==1? true: 'd-none'"
                cols ="12"
                md="6"
                sm="6">
                    <v-card
                    class="mx-auto"
                    max-width="100%"
                    >
                        <v-card-title>
                        {{articulo.codigo}}
                        </v-card-title>

                        <v-img
                        :src="articulo.imagen"
                        height="200px"
                        ></v-img>

                        <v-card-title>
                        {{articulo.nombre}}
                        </v-card-title>

                        <v-card-subtitle>
                        {{articulo.categoria.nombre}}
                        </v-card-subtitle>

                        <v-card-title>
                        Precio: {{articulo.precio}}
                        </v-card-title>

                        <v-card-actions>
                        <v-btn
                            color="accent"
                            text
                        >
                            Descripción
                        </v-btn>

                        <v-spacer></v-spacer>

                        <v-btn
                            icon
                            @click="show = !show"
                        >
                            <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
                        </v-btn>
                        </v-card-actions>

                        <v-expand-transition>
                        <div v-show="show">
                            <v-divider></v-divider>

                            <v-card-text>
                            {{articulo.descripcion}}
                            </v-card-text>
                        </div>
                        </v-expand-transition>
                </v-card>
                </v-col>
            </v-row>
        </v-container>
</template>

<script>
import axios from "axios"
    export default {
        name: "ComponentArticuloData",
        data(){
            return{
                show: false,
                articulos: {},
            }
        },
        mounted() {
        axios
            .get("https://cryptic-tor-85862.herokuapp.com/api/articulo/list")
            .then((response) => {
            (this.articulos = response.data)
            // console.log(this.servicios);
            });
        },
    }
</script>

<style lang="scss" scoped>

</style>