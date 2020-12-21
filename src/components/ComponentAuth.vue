<template>
    <v-app id="inspire">
        <v-app-bar-nav-icon :class="this.$store.state.token===null? 'd-flex d-sm-none' : false" @click="drawer = !drawer"></v-app-bar-nav-icon>
            <v-navigation-drawer
            v-model="drawer"
            fixed
            temporary
            :class="this.$store.state.token==null? 'd-flex d-sm-none' : false"
            >
            <v-list>
                <v-list-item>
                <v-icon x-large>
                    mdi-account-circle
                </v-icon>
                </v-list-item>
                <v-list-item-content v-if="this.$store.state.token!==null" class="ml-3">
                    <v-list-item-title  class="title">
                    {{this.$store.state.usuario.nombre}} {{this.$store.state.usuario.apellido}}
                    </v-list-item-title>
                    <v-list-item-subtitle>{{this.$store.state.usuario.email}}</v-list-item-subtitle>
                </v-list-item-content>
                <v-divider></v-divider>
                <v-list-group
                prepend-icon="mdi-head-minus"
                >
                <template v-slot:activator>
                    <v-list-item-title>Administrar</v-list-item-title>
                </template>
        
                <v-list-item
                    v-for="(item, key) in administrar"
                    :key="key"
                    :to="item.to"
                    link
                >
                    <v-list-item-title v-text="item.title"></v-list-item-title>
        
                    <v-list-item-icon>
                    <v-icon v-text="item.icon"></v-icon>
                    </v-list-item-icon>
                </v-list-item>
                </v-list-group>
        
                <v-list-group
                prepend-icon="mdi-shield-account"
                >
                <template v-slot:activator>
                    <v-list-item-title>Permisos</v-list-item-title>
                </template>
        
                <v-list-item
                    v-for="(item, key) in permisos"
                    :key="key"
                    :to="item.to"
                    link
                >
                    <v-list-item-title v-text="item.title"></v-list-item-title>
        
                    <v-list-item-icon>
                    <v-icon v-text="item.icon"></v-icon>
                    </v-list-item-icon>
                </v-list-item>
                </v-list-group>
            </v-list>
            </v-navigation-drawer>
        <v-main class="grey lighten-2">
            <v-container>
                <router-view></router-view>
            </v-container>
        </v-main>
    </v-app>
</template>

<script>
    export default {
        name: "ComponentAuth",
        data(){ 
        return{
            drawer: null ,
            administrar: [
            {
                title: 'Categorías', 
                icon:'mdi-shape',
                to: "/auth/categoria"
            },
            {
                title: 'Artículos', 
                icon:'mdi-file',
                to: "/auth/articulo"
            },
            ],
            permisos:[          
            {
                title: "Usuarios",
                icon: "mdi-account-circle",
                to: "/auth/usuario"
            }
            ],
        }
        },
    }
</script>

<style scoped>

</style>