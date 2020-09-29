<template>
  <v-app>
    <!-- <v-app-bar 
      app
      hide-on-scroll
      color="grey lighten-3"
    >
      <v-toolbar-title>
        <router-link to="/" class="toolbar-title">
          #Vuetify
        </router-link>
      </v-toolbar-title>
    </v-app-bar> -->

    <v-content class="grey lighten-3">
      <router-view/>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
    }
  }
}
</script>

<style scoped>
</style>