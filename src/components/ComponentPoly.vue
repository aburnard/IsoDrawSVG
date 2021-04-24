<template>
  <polygon
    @mouseenter="handleEnter(message, $event)"
    @click="handleClick"
    :points="computedPoints"
    :style="computedStyle"
  />
</template>

<script>
export default {
  data() {
    return {
      polyPoints: [],
      fillColor: this.pixMatrix[this.yCoord][this.xCoord],
      localFill: "",
      watchedColor: "blue",
    };
  },

  props: [
    "eyeDropper",
    "id",
    "xBias",
    "yBias",
    "item",
    "index",
    "sideLength",
    "color",
    "xCoord",
    "yCoord",
    "pixMatrix",
  ],
  methods: {
    handleEnter(message, event) {
      if (event.buttons == 1) {
        this.localFill = this.color;
        this.$emit("colorPixel", this.pixelAddress);
        this.id++;
      }
    },

    handleClick() {
      if (this.eyeDropper == true) {
        this.$emit("eyeDropSuck", this.computedFill);
      } else {
        this.localFill = this.color;
        this.$emit("colorPixel", this.pixelAddress);
      }
    },
  },

  computed: {
    pixelAddress: function () {
      return [this.yCoord, this.xCoord];
    },

    computedStyle() {
      return "fill:" + this.computedFill + ";stroke:purple;stroke-width:.5";
    },
    computedFill() {
      if (this.localFill == "") {
        return this.pixMatrix[this.yCoord][this.xCoord];
      } else return this.localFill;
    },

    computedPoints() {
      return this.polyPoints.concat(
        this.xBias,
        this.yBias,
        this.xBias + this.sideLength,
        this.yBias,
        this.sideLength / 2 + this.xBias,
        this.yBias + this.sideLength
      );
    },
    // pointA() {
    //   return this.xBias;
    // },
    // pointA2() {
    //   return this.yBias;
    // },
    // pointB() {
    //   return this.xBias + this.sideLength;
    // },
    // pointB2() {
    //   return this.yBias;
    // },
    // pointC() {
    //   return this.sideLength / 2 + this.xBias;
    // },
    // pointC2() {
    //   return this.yBias + this.sideLength;
    // },
  },
};
</script>

