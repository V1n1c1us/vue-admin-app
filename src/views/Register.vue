<template>
  <v-app id="inspire">
    <v-content>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4>
            <v-card class="elevation-12">
              <v-toolbar dark color="primary">
                <v-toolbar-title>Register</v-toolbar-title>
              </v-toolbar>
              <v-card-text>
                <v-form>
                  <v-text-field
                    prepend-icon="account_circle"
                    name="name"
                    v-model="usuario.name"
                    label="Name"
                    type="text"
                  ></v-text-field>
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
                  <v-text-field
                    prepend-icon="lock"
                    name="password"
                    v-model="usuario.password_confirmation"
                    label="Password Confirmation"
                    type="password"
                  ></v-text-field>
                  <input type="file" name="imgprofile" @change="saveImageProfile"/>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary">
                  <router-link to="/login">Login</router-link>
                </v-btn>
                <v-btn color="success" @click="register()">Register</v-btn>
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
import vue2Dropzone from "vue2-dropzone";
import "vue2-dropzone/dist/vue2Dropzone.min.css";
import axios from "axios";
export default {
  name: "register",
  components: { VueElementLoading, vueDropzone: vue2Dropzone },
  data() {
    return {
      usuario: {
        name: "",
        email: "",
        password: "",
        password_confirmation: "",
        imgprofile: ""
      },
      isActive: false,
    }
  },
  methods: {
    saveImageProfile(e) {
      const file = e.target.files || e.dataTransfer.files
      if (!files.length)
        return;

      this.createImage(files[0]);
    },
    createImage(file) {
      const reader = new FileReader()
      reader.onload = (e) => {
        this.imgprofile = e.target.result
      }
      reader.readAsDataURL(file)
    },
    register() {
      axios
        .post("http://laravel-register-app.test/api/create", {
          name: this.usuario.name,
          email: this.usuario.email,
          password: this.usuario.password,
          password_confirmation: this.usuario.password_confirmation,
          imgprofile: this.usuario.imgprofile
        })
        .then(response => {
          console.log(response.data);
        });
    }
  }
};
</script>

