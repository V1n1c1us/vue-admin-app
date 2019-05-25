<template>
  <v-app>
    <v-content>
      <v-toolbar>
        <v-toolbar-side-icon></v-toolbar-side-icon>
        <v-toolbar-title>Title</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items class="hidden-sm-and-down">
          <v-btn flat>{{ usuario.name }}</v-btn>
          <v-btn flat @click="logout()"><v-icon>exit_to_app</v-icon></v-btn>
        </v-toolbar-items>
      </v-toolbar>
      <vue-element-loading
        text="Bem-vindo"
        spinner="line-wave"
        color="#fff"
        size="64"
        :active="isActive"
        :is-full-screen="true"
        background-color="rgba(23,77,95, .9)"
      />
    </v-content>
  </v-app>
</template>
<script>
import VueElementLoading from "vue-element-loading";
export default {
  name: "navbar",
  components: { VueElementLoading },
  data() {
    return {
      usuario: false,
      isActive: false
    };
  },
  created() {
    let usuarioAuth = sessionStorage.getItem("usuario");
    if (usuarioAuth) {
      this.usuario = JSON.parse(usuarioAuth);
    } else {
      this.$router.push("/login");
    }
  },
  methods: {
    logout() {
      this.isActive = true;
      sessionStorage.clear();
      setTimeout(() => {
        this.isActive = false;
        this.usuario = false;
        this.$router.push("/login");
      }, 2000);
    }
  }
};
</script>

