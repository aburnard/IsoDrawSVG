<template>
  <div class="about">
    <h1>This is an about page</h1>
    <section>
      <b-field label="Zoom">
        <b-slider v-model="xBias"></b-slider>
      </b-field>
      <b-field label="Size">
        <b-slider v-model="Size"></b-slider>
      </b-field>
    </section>
    <section>
      <b-button @click="clickMe">Click Me</b-button>
    </section>
    <div class="container is-fluid" v-for="p in Size" :key="p">
      <svg width="100%" :height="xBias">
        <circle @click="clickMe" :cy="100" :cx="100" :r="100" />
        <ComponentPoly
          v-for="n in Size"
          :key="n"
          :xBias="n * xBias + (p%2 * xBias/2)"
          :yBias="0"
          :sideLength="xBias"
        />
        <ComponentPolyUp
          v-for="n in Size"
          :key="n"
          :xBias="n * xBias + (p%2 * xBias/2)"
          :yBias="0"
          :sideLength="xBias"
        />
      </svg>
    </div>
  </div>
</template>

<script>
import ComponentPoly from "@/components/ComponentPoly.vue";
import ComponentPolyUp from "@/components/ComponentPolyUp.vue";

export default {
  name: "About",
  components: {
    ComponentPoly,
    ComponentPolyUp
  },
  data() {
    return {
      xBias: 0,
      Size: 10,
      howMany: 10
    };
  },

  methods: {
    clickMe() {
      this.$buefy.notification.open("Clicked!!");
    }
  }
};
</script>

<style scoped>
svg {
  display: inline-block;
}

.container {
  font-size: 0;
}
</style>