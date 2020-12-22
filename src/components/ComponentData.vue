<template>
    <v-form>
        <v-card-title>
                <span class="headline">DATOS USUARIO</span>
            </v-card-title>
        <v-row>
            <v-col
            cols="12"
            sm="6"
            >
            <v-text-field
                :value="user.nombre"
                label="Nombre"
                solo
                disabled
            ></v-text-field>
            </v-col>
            <v-col
            cols="12"
            sm="6"
            >
            <v-text-field
                :value="user.apellido"
                label="Solo"
                solo
                disabled
            ></v-text-field>
            </v-col>
            <v-col
            cols="12"
            >
            <v-text-field
                :value="user.email"
                label="Solo"
                solo
                disabled
            ></v-text-field>
            </v-col>
        </v-row>
    </v-form>
</template>

<script>
    export default {
        name: "ComponentData",
        data() {
            return {
            user:{}
            };
        },
        methods: {
            getUserData(){
            // console.log(user);
            this.user = this.$store.state.usuario;
            // console.log(this.user);
            }
        },
        created(){
            this.getUserData();
        }

    }
</script>

<style scoped>

</style>