<template>
  <v-app id="inspire">
    <v-system-bar app>
      <v-spacer></v-spacer>

      <v-icon>mdi-square</v-icon>

      <v-icon>mdi-circle</v-icon>

      <v-icon>mdi-triangle</v-icon>
    </v-system-bar>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      fixed
      temporary
    >
      <v-card
        class="mx-auto"
        width="300"
      >
        <v-list>
          <v-list-item to="/">
            <v-list-item-icon>
              <v-icon>mdi-home</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item>
    
            <v-list-group
              prepend-icon="mdi-head-minus"
            >
              <template v-slot:activator>
                  <v-list-item-title>Administrar</v-list-item-title>
              </template>
    
              <v-list-item
                v-for="(item, key) in administrar"
                :key="key"
                link
              >
                <v-list-item-title v-text="item.title" :to="item.to"></v-list-item-title>
    
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
                link
              >
                <v-list-item-title v-text="item.title" :to="item.to"></v-list-item-title>
    
                <v-list-item-icon>
                  <v-icon v-text="item.icon"></v-icon>
                </v-list-item-icon>
              </v-list-item>
            </v-list-group>
        </v-list>
      </v-card>
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
    data()
    { 
      return{
        drawer: null ,
        administrar: [
          {
            title: 'Categorías', 
            icon:'mdi-shape',
            to: "/categoria"
          },
          {
            title: 'Artículos', 
            icon:'mdi-file',
            to: "/categoria"
          },
        ],
        permisos:[          
          {
            title: "Usuarios",
            icon: "mdi-account-circle",
            to: "/usuario"
          }
        ],
      }
    },
  }
</script>