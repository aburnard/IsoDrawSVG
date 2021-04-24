<template>
  <div class="about">
    <b-collapse :open="false" aria-id="contentIdForA11y1">
      <button
        class="button is-primary"
        slot="trigger"
        aria-controls="contentIdForA11y1"
      >
        Click for Load/Save
      </button>

      <div class="notification">
        <div class="content">
          <h3>FileSaving</h3>
          <section>
            <b-field
              label="Copy and Paste Textarea"
              :label-position="labelPosition"
            >
              <b-input v-model="pixMatrixString" type="textarea"></b-input>
            </b-field>
          </section>

          <section>
            <b-button @click="dumpButton">Dump To Textarea</b-button>
            <b-button @click="loadButton">Load from Textarea</b-button>

            <b-button @click="saveareadialog"> ? </b-button>
          </section>

          <section>
            <b-button @click="dumpIt()">Download as SVG</b-button>
          </section>
        </div>
      </div>
    </b-collapse>

    <!-- <h1>
      IsoPixelDraw Zoom {{ xBias }} Size {{ Size }} vbx {{ vbx }} vby
      {{ vby }} vbWidth {{ vbWidth }} vbHeight {{ vbHeight }} sizebyzoom
      {{ Size * xBias }}
    </h1> -->

    <hr />
    <div class="columns is-centered is-mobile">
      <div class="column"></div>
      <div class="column">
        <section>
          <b-field label="Zoom">
            <b-numberinput
              controls-rounded
              v-model="xBias"
              step="5"
            ></b-numberinput>
          </b-field>
        </section>
      </div>
      <div class="column"></div>
    </div>
    <div class="column">
      <b-field>
        <vswatches v-model="color" swatches="text-advanced"></vswatches>
      </b-field>
      <div class="field">
        <b-switch v-model="eyeDropper">
          Eyedropper is {{ eyeDropper }}
        </b-switch>
      </div>
    </div>

    <div class="columns is-centered">
      <div class="column five-eigths">
        <div class="container">
          <div>
            <svg id="isoSvg" ref="isoSvg" :viewBox="computedViewBox">
              <!-- p in size is y coord -->
              <!-- n in size is x coord -->
              <g v-for="(item, index) in noRows" :key="'ycoord' + index">
                <ComponentPoly
                  :color="color"
                  v-for="n in rowwidth"
                  :id="key"
                  :key="'xcoord' + n"
                  :pixMatrix="pixMatrix"
                  :xBias="n * xBias + ((index % 2) * xBias) / 2"
                  :yBias="index * xBias"
                  :sideLength="xBias"
                  :eyeDropper="eyeDropper"
                  :yCoord="index - 1"
                  :xCoord="n - 1"
                  @colorPixel="colorPixel"
                  @eyeDropSuck="eyeDropSuck"
                />
                <ComponentPolyUp
                  :id="key"
                  :color="color"
                  v-for="(item, n) in rowwidth"
                  :key="item.id"
                  :xBias="n * xBias + ((index % 2) * xBias) / 2"
                  :pixMatrixUp="pixMatrixUp"
                  :yBias="index * xBias"
                  :sideLength="xBias"
                  :eyeDropper="eyeDropper"
                  :yCoord="index - 1"
                  :xCoord="n - 1"
                  @colorPixelUp="colorPixelUp"
                  @eyeDropSuck="eyeDropSuck"
                />
              </g>
            </svg>
          </div>
        </div>
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
    vswatches,
  },

  data() {
    return {
      startPixMatrix: startPixMatrix,
      dataPixMat: {
        mat: [],
        matUp: [],
      },
      eyeDropper: false,
      fileSaveObject: {},
      pixMatrixString: "",
      color: "red",
      xBias: 30,
      Size: 29,
      noRows: 20,
      rowwidth: 40,
      allVisible: true,
      vbx: 86,
      vby: 20,
      vbWidth: 1000,
      vbHeight: 800,
      viewBoxArray: [],
      matrixPlaceHolder: [],
      pixMatrix: startPixMatrix.pixMatrix,
      pixMatrixUp: startPixMatrix.pixMatrixUp,
      isOpen: false,
      pixMatrixReplace: [
        ["purple", "yellow", "purple", "purple", "purple", "brown"],
        ["yellow", "purple", "purple", "purple", "purple", "brown"],
        ["purple", "purple", "yellow", "purple", "purple", "brown"],
        ["purple", "purple", "purple", "yellow", "purple", "brown"],
        ["purple", "purple", "purple", "purple", "yellow", "brown"],
      ],
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
    },
  },
  // created() {
  //   if (window.innerWidth > 768) {
  //     this.landscape();
  //   }
  // },
  methods: {
    saveareadialog() {
      this.$buefy.dialog.alert(
        "DUMP To Textarea? dump image data, copy for later use. LOAD from Textarea? paste some previous data to restore your work. DOWNLOAD as SVG? Save your image as an SVG."
      );
    },
    landscape() {
      this.vbWidth = 1000;
      this.rowwidth = 40;
      this.xBias = 25;
      this.vbx = 86;
      this.vby = 20;
    },

    dumpIt() {
      let svgData = new Blob([this.$refs.isoSvg.outerHTML], {
        type: "text/plain",
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
    eyeDropSuck(recievedColor) {
      this.color = recievedColor;
      this.eyeDropper = false;
    },
    colorPixel(recievedCoords) {
      this.pixMatrix[recievedCoords[0]][recievedCoords[1]] = this.color;
    },
    colorPixelUp(recievedCoords) {
      this.pixMatrixUp[recievedCoords[0]][recievedCoords[1]] = this.color;
    },
    dumpButton() {
      this.dataPixMat.mat = this.pixMatrix.slice(0);
      this.dataPixMat.matUp = this.pixMatrixUp.slice(0);

      this.pixMatrixString = JSON.stringify(this.dataPixMat);
    },

    loadButton() {
      this.dataPixMat = JSON.parse(this.pixMatrixString);
      this.pixMatrix = this.dataPixMat.mat.slice(0);
      this.pixMatrixUp = this.dataPixMat.matUp.slice(0);
    },
  },
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