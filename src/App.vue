<template>
  <v-app id="inspire">
      <div>
        <v-toolbar dark  src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg">

          <v-btn rounded color="#0f5194" to="/" dark class="mx-3">
            <v-icon >mdi-home</v-icon>
          </v-btn>         

          <v-spacer class="d-none d-sm-flex"></v-spacer>

          <v-toolbar-title class="d-none d-sm-flex">Nombre</v-toolbar-title>

          <v-spacer></v-spacer>

          <v-app-bar-nav-icon :class="this.$store.state.token==null? 'd-flex d-sm-none' : false" @click="drawer = !drawer"></v-app-bar-nav-icon>

          <v-btn rounded color="accent" to="/login" dark :class="this.$store.state.token===null? 'mx-3 d-none d-sm-flex' : 'd-none'">
            Login
          </v-btn>

          <v-btn rounded color="primary" to="/register" dark :class="this.$store.state.token===null? 'd-none d-sm-flex' : 'd-none'">
            Register
          </v-btn>

          <v-btn rounded color="primary" @click="cerrarSesion" dark :class="this.$store.state.token!==null? true : 'd-none'">
            Logout
          </v-btn>

        </v-toolbar>

        <v-navigation-drawer
          v-model="drawer"
          fixed
          temporary
          :class="this.$store.state.token==null? 'd-flex d-sm-none' : false"
        >
            <v-list-group
              :value="true"
              prepend-icon="mdi-account-circle"
              :class="this.$store.state.token===null? true : 'd-none'"
            >
              <template v-slot:activator>
                <v-list-item-title>Users</v-list-item-title>
              </template>              

              <v-list-item
                v-for="item in items"
                :key="item.title"
                :to="item.to"
                link                
              >

                <v-list-item-icon>
                  <v-icon>{{item.icon}}</v-icon>
                </v-list-item-icon>

                <v-list-item-title>{{item.title}}</v-list-item-title>

              </v-list-item>
            </v-list-group>
        </v-navigation-drawer>
      </div>

      <v-main class="overflow-y-auto" max-height="600">
          <router-view></router-view>
      </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    drawer: null,
    items: [
        { title: 'Login', icon: 'mdi-login', to: "/login"},
        { title: 'Register', icon: 'mdi-account-plus', to: "/register" },
      ],
  }),
  methods: {
      cerrarSesion(){
        this.$store.dispatch("salir")
      },
    }
};
</script>