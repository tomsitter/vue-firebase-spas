<template>
  <div id="app">
    <Navigation />
    <router-view class="container" :user="user" />
  </div>
</template>

<script>
import Navigation from "@/components/Navigation.vue"
// import Firebase from "firebase"
import db from "./db.js";
export default {
    name: "App",
    data: function() {
      return {
          user: null
      };
    },
    mounted() {
      console.log(this);
      db.collection("users")
      .doc("testuser")
      .get()
      .then((snapshot) => {
          this.user = snapshot.data().name;
      });
    },
    components: {
        Navigation
    }
}
</script>

<style lang="scss">
$primary: #05b2dd;
@import "node_modules/bootstrap/scss/bootstrap";
</style>
