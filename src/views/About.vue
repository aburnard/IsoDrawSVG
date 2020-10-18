


<template>
  <div class="about">
    <b-field label="Message" :label-position="labelPosition">
      <b-input v-model="pixMatrixString" type="textarea"></b-input>
    </b-field>
    <section>
      <b-button @click="dumpButton">Dump</b-button>
      <b-button @click="loadButton">Load</b-button>
      <b-button @click="newCanvas">newCanvas</b-button>
    </section>
    <div class="field">
      <b-switch v-model="allVisible">Default</b-switch>
    </div>
    <section>
      <b-button @click="dumpIt()">Download as SVG</b-button>
    </section>
    <button class="button is-medium is-dark" @click="promptNumber">
      Launch prompt (number)
    </button>
    <h1>
      IsoPixelDraw Zoom {{ xBias }} Size {{ Size }} vbx {{ vbx }} vby
      {{ vby }} vbWidth {{ vbWidth }} vbHeight {{ vbHeight }} sizebyzoom
      {{ Size * xBias }}
    </h1>
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
    <div class="box">
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
          <g v-for="p in 30" :key="p" width="100%" :height="xBias * 0.65">
            <ComponentPoly
              :color="color"
              v-for="n in 30"
              :id="
                key *
                  Math.floor(Math.random() * 10) *
                  Math.floor(Math.random() * 10) *
                  Math.floor(Math.random() * 10)
              "
              :key="n"
              :pixMatrix="pixMatrix"
              :xBias="n * xBias + ((p % 2) * xBias) / 2"
              :yBias="p * xBias"
              :sideLength="xBias"
              :yCoord="p - 1"
              :xCoord="n - 1"
              @colorPixel="colorPixel"
            />
            <ComponentPolyUp
              :id="
                key *
                  Math.floor(Math.random() * 10) *
                  Math.floor(Math.random() * 10) *
                  Math.floor(Math.random() * 10)
              "
              :color="color"
              v-for="n in 30"
              :key="n"
              :xBias="n * xBias + ((p % 2) * xBias) / 2"
              :pixMatrixUp="pixMatrixUp"
              :yBias="p * xBias"
              :sideLength="xBias"
              :yCoord="p - 1"
              :xCoord="n - 1"
              @colorPixelUp="colorPixelUp"
            />
          </g>
        </svg>
      </div>
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
      xBias: 42,
      Size: 29,
      allVisible: true,
      vbx: 0,
      vby: 230,
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
    promptNumber() {
      this.$buefy.dialog.prompt({
        message: `What's your age?`,
        inputAttrs: [
          {
            type: "number",
            placeholder: "Type your age",
            value: "18",
            maxlength: 2,
            min: 18
          },
          {
            type: "number",
            placeholder: "Type your age",
            value: "18",
            maxlength: 2,
            min: 18
          }
        ],

        trapFocus: true,
        onConfirm: value => this.$buefy.toast.open(`Your age is: ${value}`)
      });
    },
    applied() {
      this.downer = "true";
    },
    released() {
      this.downer = "false";
    },
    newCanvas() {
      var A = [];
      for (var i = 0; i < 40; i++) {
        A[i] = [];
        for (var j = 0; j < 50; j++) {
          A[i][j] = "blue";
        }
      }
      this.pixMatrixUp = A.slice(0);
      this.pixMatrix = A.slice(0);
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