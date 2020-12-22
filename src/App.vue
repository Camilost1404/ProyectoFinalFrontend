<template>
  <v-app id="inspire">
      <div>
        <v-toolbar dark src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg">

          <v-btn rounded color="#0f5194" to="/" dark class="mx-3">
            <v-icon >mdi-home</v-icon>
          </v-btn>         

          <v-spacer class="d-none d-sm-flex"></v-spacer>

          <v-toolbar-title class="d-none d-sm-flex">ENLACE HUMANO</v-toolbar-title>

          <v-spacer></v-spacer>

          <v-app-bar-nav-icon :class="this.$store.state.token==null? 'd-flex d-sm-none' : 'd-none'" @click="drawer = !drawer"></v-app-bar-nav-icon>

          <v-btn rounded color="accent" to="/login" dark :class="this.$store.state.token===null? 'mx-3 d-none d-sm-flex' : 'd-none'">
            Login
          </v-btn>

          <v-btn rounded color="primary" to="/register" dark :class="this.$store.state.token===null? 'd-none d-sm-flex' : 'd-none'">
            Register
          </v-btn>

          <v-btn :to="{path: '/auth/usuariodata'}" v-if="this.$store.state.token!==null" class="mr-3">
            <v-icon>mdi-account-circle</v-icon> <span class="ml-3 d-none d-sm-flex">Account</span>
          </v-btn>

          <v-btn rounded color="primary" @click="cerrarSesion" dark :class="this.$store.state.token!==null? true : 'd-none'">
            <v-icon>mdi-logout</v-icon>
            <span class="ml-3 d-none d-sm-flex">LogOut</span>
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

      <v-main class="main overflow-y-auto" max-height="600">
          <router-view></router-view>
      </v-main>
      <v-footer
      color="primary lighten-1"
      padless
    >
    <v-row
      justify="center"
      no-gutters
    >
      <v-btn
        v-for="link in links"
        :key="link.nombre"
        color="white"
        text
        rounded
        :href="link.link"
        target="_blank"
        class="my-2"
      >
        <v-icon medium>{{link.icon}}</v-icon>
        <span class="ml-3">{{ link.nombre }}</span>
      </v-btn>
      <v-col
        class="primary lighten-2 py-4 text-center white--text"
        cols="12"
      >
        <v-card-subtitle class="text-h6">Contactenos:</v-card-subtitle>
        <v-card-text class="text-h6">
          Cel. 3123457657 / Tel. 234573
        </v-card-text>
        <v-card-subtitle class="text-h6">Encuentranos en:</v-card-subtitle>
        <v-card-text class="text-h6">          
          <span>Ibague - </span>
          <span>Cartagena - </span>
          <span>Bogóta - </span>
          <span>Calí</span>
        </v-card-text>
        {{ new Date().getFullYear() }} — <strong>Colombia</strong>
      </v-col>
    </v-row>
  </v-footer>
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
    links: [
        {nombre:'GitHub BackEnd',icon: "mdi-github", link: "https://github.com/grupo196ciclo3/FinalBackend.git"},
        {nombre:'GitHub FrontEnd',icon: "mdi-github", link: "https://github.com/grupo196ciclo3/FinalFrontend.git"}
      ],
  }),
  created(){
    this.$store.dispatch("autoLogin")
  },
  methods: {
      cerrarSesion(){
        this.$store.dispatch("salir")
      },
    }
};
</script>

<style>
.main{
  background-color: #B3E5FC;
}
</style>