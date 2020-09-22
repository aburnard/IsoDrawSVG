


<template>
  <div class="about">
    <b-field label="Message" :label-position="labelPosition">
      <b-input v-model="pixMatrixString" type="textarea"></b-input>
    </b-field>
    <section>
      <b-button @click="dumpButton">Dump</b-button>
      <b-button @click="loadButton">Load</b-button>
    </section>
    <div class="field">
      <b-switch v-model="allVisible">Default</b-switch>
    </div>
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

    <div class="container is-fluid" v-if="allVisible">
      <svg
        id="isoSvg"
        ref="isoSvg"
        :viewBox="computedViewBox"
        :width="xBias * Size * 1.2"
        :height="xBias * Size"
      >
        <!-- p in size is y coord -->
        <!-- n in size is x coord -->
        <g v-for="p in pixMatrix[1].length" :key="p" width="100%" :height="xBias * .65">
          <ComponentPoly
            :color="color"
            v-for="n in pixMatrix[0].length"
            :id="key * Math.floor(Math.random() * 10)* Math.floor(Math.random() * 10) * Math.floor(Math.random() * 10)"
            :key="n"
            :pixMatrix="pixMatrix"
            :xBias="n * xBias + (p%2 * xBias/2)"
            :yBias="p*xBias"
            :sideLength="xBias"
            :yCoord="p-1"
            :xCoord="n-1"
            @colorPixel="colorPixel"
          />
          <ComponentPolyUp
            :id="key * Math.floor(Math.random() * 10)* Math.floor(Math.random() * 10) * Math.floor(Math.random() * 10)"
            :color="color"
            v-for="n in pixMatrixUp[0].length"
            :key="n"
            :xBias="n * xBias + (p%2 * xBias/2)"
            :pixMatrixUp="pixMatrixUp"
            :yBias="p*xBias"
            :sideLength="xBias"
            :yCoord="p-1"
            :xCoord="n-1"
            @colorPixelUp="colorPixelUp"
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
import startPixMatrix from "@/assets/startPixMatrix.json";
export default {
  name: "About",
  components: {
    ComponentPoly,
    ComponentPolyUp,
    vswatches
  },

  data() {
    return {
      startPixMatrix: startPixMatrix,
      dataPixMat: {
        mat: [],
        matUp: []
      },
      fileSaveObject: {},
      pixMatrixString: "",
      color: "red",
      xBias: 98,
      Size: 5,
      allVisible: true,
      vbx: 75,
      vby: 405,
      vbWidth: 1368,
      vbHeight: 623,
      viewBoxArray: [],
      matrixPlaceHolder: [],
      pixMatrix: startPixMatrix.pixMatrix,
      pixMatrixUp: startPixMatrix.pixMatrixUp,

      pixMatrixReplace: [
        ["purple", "yellow", "purple", "purple", "purple", "brown"],
        ["yellow", "purple", "purple", "purple", "purple", "brown"],
        ["purple", "purple", "yellow", "purple", "purple", "brown"],
        ["purple", "purple", "purple", "yellow", "purple", "brown"],
        ["purple", "purple", "purple", "purple", "yellow", "brown"]
      ]
    };
  },
  computed: {
    // computedPixMatrixObject() {
    //   this.dataPixMat.mat = this.pixMatrix.slice(0);
    //    this.dataPixMat.matUp = this.pixMatrixUp.slice(0);
    //    return this.pixMatrix + this.pixMatrixUp;
    // },

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
    }
  },
  methods: {
    dumpIt() {
      let svgData = new Blob([this.$refs.isoSvg.outerHTML], {
        type: "text/plain"
      });

      FileSaver.saveAs(svgData, "newSvgData.svg");
    },
    inputTransfer() {
      this.matrixPlaceHolder = this.pixMatrix.slice(0);
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
    },
    colorPixel(recievedCoords) {
      //this.matrixPlaceHolder = this.pixMatrix.slice(0);
      this.pixMatrix[recievedCoords[0]][recievedCoords[1]] = this.color;
      //this.pixMatrix = this.matrixPlaceHolder.slice(0);
    },
    colorPixelUp(recievedCoords) {
      // this.matrixPlaceHolder = this.pixMatrixUp.slice(0);
      this.pixMatrixUp[recievedCoords[0]][recievedCoords[1]] = this.color;
      // this.pixMatrixUp = this.matrixPlaceHolder.slice(0);
    },
    dumpButton() {
      this.dataPixMat.mat = this.pixMatrix.slice(0);
      this.dataPixMat.matUp = this.pixMatrixUp.slice(0);
      //alert(this.computedPixMatrixObject);
      this.pixMatrixString = JSON.stringify(this.dataPixMat);
    },

    loadButton() {
      // this.dataPixMat = JSON.parse(this.pixMatrixString)
      //alert(this.computedPixMatrixObject);
      //this.pixMatrixString = JSON.stringify(this.dataPixMat);
      this.dataPixMat = JSON.parse(this.pixMatrixString);
      this.pixMatrix = this.dataPixMat.mat.slice(0);
      this.pixMatrixUp = this.dataPixMat.matUp.slice(0);
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