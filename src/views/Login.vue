<template>
  <v-app id="inspire">
    <v-content>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4>
            <v-card class="elevation-12">
              <v-toolbar dark color="primary">
                <v-toolbar-title>Login form</v-toolbar-title>
              </v-toolbar>
              <v-card-text>
                <v-form>
                  <v-text-field
                    prepend-icon="email"
                    name="email"
                    v-model="usuario.email"
                    label="Email"
                    type="text"
                  ></v-text-field>
                  <v-text-field
                    prepend-icon="lock"
                    name="password"
                    v-model="usuario.password"
                    label="Password"
                    type="password"
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" @click="login()">Login</v-btn>
              </v-card-actions>
            </v-card>
            <vue-element-loading
              text="Bem-vindo"
              spinner="line-wave"
              color="#fff"
              size="64"
              :active="isActive"
              :is-full-screen="true"
              background-color="rgba(23,77,95, .9)"
            />
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>
<script>
import VueElementLoading from "vue-element-loading";
import axios from "axios";

export default {
  name: "login",
  components: { VueElementLoading },
  data() {
    return {
      usuario: {
        email: "",
        password: ""
      },
      isActive: false
    };
  },
  methods: {
    login() {
      const URL_API = "http://register.test/api/login";

      axios
        .post(URL_API, {
          email: this.usuario.email,
          password: this.usuario.password
        })
        .then(response => {
          console.log(response);
          if (response.data.token) {
            this.isActive = true;
            setTimeout(() => {
              this.$router.push("/admin");
            }, 2000);
            sessionStorage.setItem("usuario", JSON.stringify(response.data));
            console.log("Login com sucesso");
          } else if (response.data.status == false) {
            //login não existe
            console.log("Login não existe");
            /*this.$notify({
              title: "Warning",
              message: "This is a warning message",
              type: "warning"
            });*/
          } else {
            //erro validação
            let erros = "";
            for (let erro of Object.values(response.data)) {
              erros += erro + " ";
            }
            alert(erros);
            console.log("erro validação");
          }
        })
        .catch(e => {
          console.log(e);
          /*this.$notify({
            title: "Ops!!",
            message: "Servidor fora do ar",
            type: "info"
          });*/
        });
    }
  }
};
</script>