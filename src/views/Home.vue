<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <IsoDrawSvgApp />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import IsoDrawSvgApp from "@/components/IsoDrawSvgApp.vue";
export default {
  name: "Home",
  components: {
    // HelloWorld,
    IsoDrawSvgApp,
  },
};
</script>
