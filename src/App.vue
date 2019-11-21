<template>
  <div id="app">
    <tabs ref="tab" v-model="tab" :tabs="tabs"></tabs>
  </div>
</template>

<script>
import Tabs from "./components/Tabs.vue";

export default {
  name: "app",
  components: {
    Tabs
  },
  data() {
    return {
      tab: "google",
      tabs: [
        {
          label: "google",
          key: "google",
          favico: require("./assets/google.jpg")
        },
        {
          label: "facebook",
          key: "facebook",
          favico: require("./assets/fb.jpg")
        },
        {
          label: "facebook1",
          key: "facebook1",
          favico: require("./assets/fb.jpg")
        }
      ]
    };
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
