<template>
  <nav>
    <v-app-bar dark elevate-on-scroll color="blue-grey darken-2" scroll-target="#content">
      <v-img
        src="logo.png"
        max-height="90%"
        max-width="125px"
        @click="$router.push('/')"
        style="cursor: pointer;"
      ></v-img>
      <v-toolbar-title
        class="text-h4 text-uppercase font-weight-black"
        @click="$router.push('/')"
        style="cursor: pointer;"
      >Quiz</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
    </v-app-bar>
    <NavDrawer />
  </nav>
</template>

<script>
import VueCookies from "vue-cookies";
const NavDrawer = () =>
  import(/* webpackChunkName: "Navigation" */ "./NavigationDrawer");
export default {
  components: {
    NavDrawer
  },
  created() {
    VueCookies.config("30d");
    if (VueCookies.isKey("Theme")) {
      this.darkMode = VueCookies.get("Theme") === "true";
    }
  },
  computed: {
    drawer: {
      get() {
        return this.$store.getters.navDrawer;
      },
      set(value) {
        this.$store.dispatch("setNavDrawerState", value);
      }
    },
    darkMode: {
      get() {
        return this.$vuetify.theme.dark;
      },
      set(value) {
        this.$vuetify.theme.dark = value;
        VueCookies.set("Theme", value);
      }
    }
  }
};
</script>