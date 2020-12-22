<template>
    <v-container>
        <v-card
        elevation="24"
        rounded
        class="pa-4"
        >
            <v-card-title>
                <span class="headline">Registrar</span>
                <v-spacer></v-spacer>
                <v-btn
                color="accent"
                @click="backToLogin"
                rounded        
                >
                <v-icon>mdi-arrow-left-circle</v-icon>
                <span class="ml-3 d-none d-sm-flex">Back to Login</span>
                </v-btn>
            </v-card-title>
            <v-card-text>
                <v-form
                ref="form"
                lazy-validation
                >
                    <v-container>
                        <v-row>
                            <v-col
                                cols="12"
                                md="6"
                                sm="6"
                            >
                                <v-text-field
                                label="Nombre *"
                                v-model="register.nombre"
                                :rules="nameRules"
                                required
                                ></v-text-field>
                            </v-col>
                            <v-col
                                cols="12"
                                md="6"
                                sm="6"
                            >
                                <v-text-field
                                label="Apellido *"
                                v-model="register.apellido"
                                :rules="nameRules"
                                required
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field
                                label="Email *"
                                v-model="register.email"
                                :rules="emailRules"
                                required
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field
                                label="Password *"
                                type="password"
                                v-model="register.password"
                                :rules="passwordRules"
                                required
                                ></v-text-field>
                            </v-col>
                        </v-row>
                    </v-container>
                    <small>* indicates required field</small>
                    <v-container>
                        <v-btn
                        color="primary"
                        class="mr-4"
                        @click="registerUser"
                        rounded
                        >
                        Registrarse
                        </v-btn>                    
                    </v-container>
                </v-form>
            </v-card-text>
        </v-card>
    </v-container>
</template>

<script>
import axios from "axios";
import swal from 'sweetalert';
    export default {
        name: "register",
        data() {
            return {
            register: {
                nombre: "",
                apellido: "",
                email: "",
                password: "",
            },
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
            ],
            passwordRules: [
                v => !!v || 'Password is required',
            ],
            nameRules: [
                v => !!v || 'Name is required',
            ],
            };
        },
        methods: {
            registerUser() {
                if(this.$refs.form.validate()){
                    axios
                    .post("http://localhost:3000/api/usuario/add", this.register)
                    .then(() => {
                        swal("Registro Correctamente", "Registro exitoso!!", "success");
                        this.$router.push("/login")
                    })
                    .catch(error => {
                        swal("Oops!", "Email ya registrado", "error");
                        this.register.nombre="";
                        this.register.apellido="";
                        this.register.email="";
                        this.register.password="";
                        console.log(error);
                    });
                }
            },
            backToLogin(){
                this.$router.push("/login")
            }
        }
    };
</script>