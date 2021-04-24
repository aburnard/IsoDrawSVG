<template>
  <polygon
    @mouseenter="handleEnter(message, $event)"
    @click="handleClick"
    :points="computedPoints"
    :style="computedStyle"
  />
</template>

<script>
//mariana cammila gabrielle
export default {
  data() {
    return {
      polyPoints: [],
      localFill: "",
      fillColor: this.pixMatrixUp[this.yCoord][this.xCoord],
    };
  },

  props: [
    "id",
    "xBias",
    "yBias",
    "item",
    "index",
    "sideLength",
    "color",
    "xCoord",
    "yCoord",
    "pixMatrixUp",
    "eyeDropper",
  ],
  methods: {
    handleClick() {
      if (this.eyeDropper == true) {
        this.$emit("eyeDropSuck", this.computedFill);
      } else {
        this.localFill = this.color;
        this.$emit("colorPixel", this.pixelAddress);
      }
    },
    handleEnter(message, event) {
      if (event.buttons == 1) {
        this.localFill = this.color;
        this.$emit("colorPixelUp", this.pixelAddress);
        this.id++;
      }
    },
  },
  computed: {
    computedFill() {
      if (this.localFill == "") {
        return this.pixMatrixUp[this.yCoord][this.xCoord];
      } else return this.localFill;
    },
    computedStyle() {
      return "fill:" + this.computedFill + ";stroke:purple;stroke-width:.1";
    },
    pixelAddress: function () {
      return [this.yCoord, this.xCoord];
    },
    computedPoints() {
      let compPoints = [];
      return compPoints.concat(
        this.xBias + this.sideLength / 2,
        this.yBias + this.sideLength,
        this.xBias + this.sideLength * 1.5,
        this.yBias + this.sideLength,
        this.sideLength + this.xBias,
        this.yBias
      );
    },
    // pointA() {
    //   return this.xBias + this.sideLength / 2;
    // },
    // pointA2() {
    //   return this.yBias + this.sideLength;
    // },
    // pointB() {
    //   return this.xBias + this.sideLength * 1.5;
    // },
    // pointB2() {
    //   return this.yBias + this.sideLength;
    // },
    // pointC() {
    //   return this.sideLength + this.xBias;
    // },
    // pointC2() {
    //   return this.yBias;
    // },
  },
};
</script>

