<template>
        <v-container>
            <v-card
        elevation="24"
        rounded
        class="pa-4"
        >
            <v-card-title>
                <span class="headline">Ingresar</span>
            </v-card-title>
            <v-form
                ref="form"
                lazy-validation
            >
                <v-text-field
                v-model="login.email"
                type="email"
                :rules="emailRules"
                v-on:keyup.enter="loginUser"
                class="ma-4"
                label="E-mail"
                required
                ></v-text-field>

                <v-text-field
                v-model="login.password"
                type="password"
                :rules="passwordRules"
                v-on:keyup.enter="loginUser"
                class="ma-4"
                label="Password"
                required
                ></v-text-field>

                <v-btn
                color="primary"
                class="mr-4"
                @click="loginUser"
                rounded
                >
                Ingresar
                </v-btn>

                <v-btn
                color="accent"
                @click="goToRegister"
                rounded        
                >
                Registrarse
                </v-btn>
            </v-form>
        </v-card>
        </v-container>
</template>

<script>
import axios from "axios"
import swal from 'sweetalert';
    export default {
        name: "ComponentLogin",    
    data(){
        return{
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
            ],
            passwordRules: [
                v => !!v || 'Password is required',
            ],
            login:{
                email: "",
                password: "",
            }
        }

    },
    beforeCreate(){
        this.$store.state.token===null? true : this.$router.push("/auth")
    },
    methods: {
        loginUser(){
            if(this.$refs.form.validate()){
                axios
                    .post("http://localhost:3000/api/usuario/login", this.login)
                    .then(result => {
                        this.$store.dispatch("guardarToken", result.data.tokenReturn)                        
                        swal("Login Correcto", "Ingreso exitoso!!", "success");
                        this.login.email="";
                        this.login.password="";
                        this.$router.push("/auth");
                    })
                    .catch(error => {
                        swal("Oops!", "Email o Contraseña incorrectos", "error");
                        this.login.email="";
                        this.login.password="";
                        console.log(error);
                    });    
            }
        },
        goToRegister(){
            this.$router.push("/register")
        }
    }
    }
</script>

<style scoped>

</style>