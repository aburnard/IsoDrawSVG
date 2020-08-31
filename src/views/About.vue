<template>
  <div class="about">
    <h1>IsoPixelDraw</h1>
    <b-field>
      <vswatches v-model="color" swatches="text-advanced"></vswatches>
    </b-field>

    <section>
      <b-field label="Zoom">
        <b-slider v-model="xBias"></b-slider>
      </b-field>
      <b-field label="Size">
        <b-slider v-model="Size"></b-slider>
      </b-field>
    </section>
    <div class="container is-fluid" v-for="p in Size" :key="p">
      <svg id="maingrid" width="100%" :height="xBias" overflow="auto">
        <ComponentPoly
          :color="color"
          :downer="downer"
          v-for="n in Size"
          :key="n"
          :xBias="n * xBias + (p%2 * xBias/2)"
          :yBias="0"
          :sideLength="xBias"
        />
        <ComponentPolyUp
          :downer="downer"
          :color="color"
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
import vswatches from "vue-swatches";
import "vue-swatches/dist/vue-swatches.css";

import ComponentPoly from "@/components/ComponentPoly.vue";
import ComponentPolyUp from "@/components/ComponentPolyUp.vue";

export default {
  name: "About",
  components: {
    ComponentPoly,
    ComponentPolyUp,
    vswatches
  },
  data() {
    return {
      color: "red",
      xBias: 40,
      Size: 40,
      howMany: 40,
      downer: false
    };
  },

  methods: {
    onMouseInSvg(message, event) {
      if (event.buttons == 1) {
        this.downer = true;
      }
    },
    applied() {
      this.downer = "true";
    },
    released() {
      this.downer = "false";
    }
  }
};
</script>

<style scoped>
/* svg {
  border: 1px solid red;
} */
.container {
  font-size: 0;
}
</style>