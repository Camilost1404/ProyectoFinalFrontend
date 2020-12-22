<template>
        <v-container class="pt-0 px-0">
            <v-row>
                <v-col
                v-for="(categoria, id) in categorias"
                :key="id"
                :class="categoria.estado==1? true: 'd-none'"
                cols ="12"
                md="6"
                sm="6">
                    <v-card
                    class="mx-auto"
                    max-width="100%"
                    >
                        <v-img
                        :src="categoria.imagen"
                        height="200px"
                        ></v-img>

                        <v-card-title>
                        {{categoria.nombre}}
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
                            {{categoria.descripcion}}
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
        name: "ComponentCategoriaData",
        data(){
            return{
                show: false,
                categorias: {},
            }
        },
        mounted() {
        axios
            .get("https://cryptic-tor-85862.herokuapp.com/api/categoria/list")
            .then((response) => {
            (this.categorias = response.data)
            // console.log(this.servicios);
            });
        },
    }
</script>

<style scoped>

</style>