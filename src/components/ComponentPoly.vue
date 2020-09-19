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
      //   sideLength: 100,
      apb: [0, 0, 100, 100, 0, 100],
      polyPoints: [],
      fillColor: this.pixMatrix[this.yCoord][this.xCoord],
      watchedColor: "blue"
    };
  },

  props: [
    "xBias",
    "yBias",
    "item",
    "index",
    "sideLength",
    "color",
    "xCoord",
    "yCoord",
    "pixMatrix"
  ],
  methods: {
    handleClick() {
      this.$emit("colorPixel");
    },
    handleEnter(message, event) {
      if (event.buttons == 1) {
        this.fillColor = this.color;
      }
    }
  },
  watch: {
    pixMatrix: function() {
      alert("really");
      this.watchedColor = this.pixMatrix[this.yCoord][this.xCoord];
    }
  },
  computed: {
    computedStyle() {
      return "fill:" + this.watchedColor + ";stroke:purple;stroke-width:.5";
    },
    computedFill() {
      return this.pixMatrix[this.yCoord][this.xCoord];
    },
    computedPoints() {
      //return this.apb;
      return this.polyPoints.concat(
        this.pointA,
        this.pointA2,
        this.pointB,
        this.pointB2,
        this.pointC,
        this.pointC2
      );
    },
    pointA() {
      return this.xBias;
    },
    pointA2() {
      return this.yBias;
    },
    pointB() {
      return this.xBias + this.sideLength;
    },
    pointB2() {
      return this.yBias;
    },
    pointC() {
      return this.sideLength / 2 + this.xBias;
    },
    pointC2() {
      return this.yBias + this.sideLength;
    }
  }
};
</script>

