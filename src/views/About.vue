


<template>
  <div class="about">
    <section>
      <b-button @click="dumpIt()">Download as SVG</b-button>
    </section>
    <h1>IsoPixelDraw Zoom {{xBias}} Size {{Size}} vbx {{vbx}} vby {{vby}} vbWidth {{vbWidth}} vbHeight {{vbHeight}}</h1>
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
    <section>
      <b-field label="vbx">
        <b-slider :max="10000" v-model="vbx"></b-slider>
      </b-field>
      <b-field label="vby">
        <b-slider :max="10000" v-model="vby"></b-slider>
      </b-field>
    </section>
    <section>
      <b-field label="vbWidth">
        <b-slider :min="400" :max="5000" v-model="vbWidth"></b-slider>
      </b-field>
      <b-field label="vbHeight">
        <b-slider :min="500" :max="5000" v-model="vbHeight"></b-slider>
      </b-field>
    </section>

    <div class="container is-fluid">
      <svg
        id="isoSvg"
        ref="isoSvg"
        :viewBox="computedViewBox"
        :width="xBias * Size *1.2"
        :height="xBias * Size"
      >
        <g v-for="p in Size" :key="p" width="100%" :height="xBias * .65">
          <ComponentPoly
            :color="color"
            v-for="n in Size"
            :key="n"
            :xBias="n * xBias + (p%2 * xBias/2)"
            :yBias="p*xBias"
            :sideLength="xBias"
          />
          <ComponentPolyUp
            :color="color"
            v-for="n in Size"
            :key="n"
            :xBias="n * xBias + (p%2 * xBias/2)"
            :yBias="p*xBias"
            :sideLength="xBias"
          />
        </g>
      </svg>
    </div>
  </div>
</template>

<script>
//import saveAs from "file-saver";
import vswatches from "vue-swatches";
import "vue-swatches/dist/vue-swatches.css";
import FileSaver from "file-saver";
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
      xBias: 37,
      Size: 40,

      vbx: 75,
      vby: 405,
      vbWidth: 1368,
      vbHeight: 623,
      viewBoxArray: []
    };
  },
  computed: {
    computedViewBox() {
      return (
        "" +
        this.viewBoxArray.concat(
          this.vbx,
          this.vby,
          this.vbWidth,
          this.vbHeight
        )
      );
      //return "0,0,400,400";
    }
  },
  methods: {
    dumpIt() {
      //console.log(this.$refs.isoSvg.outerHTML);
      let svgData = new Blob([this.$refs.isoSvg.outerHTML], {
        type: "text/plain"
      });

      FileSaver.saveAs(svgData, "newSvgData.svg");
    },
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
svg {
  border: 1px solid red;
}
.container {
  font-size: 0;
}
</style>