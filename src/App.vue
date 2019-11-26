<template>
  <v-app>
    <!-- TOP BAR -->
    <v-app-bar app color="blue darken-3" height="80">
      <h2 class="white text-uppercase">Search Jobs App</h2>
    </v-app-bar>
    <!-- END TOP BAR -->
    <!-- CONTENT -->
    <v-content style="background-color:white;">
      <router-view />
    </v-content>
    <!-- END CONTENT -->
    <!-- FOOTER -->
    <v-footer dark height="60">
      <v-flex text-center xs12>&copy; Fernando Poli {{ new Date().getFullYear() }}</v-flex>
    </v-footer>
    <!-- END FOOTER -->
  </v-app>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    //
  })
};
</script>
