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
      localFill: "",
      watchedColor: "blue"
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
    "pixMatrix"
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
      this.localFill = this.color;

      this.$emit("colorPixel", this.pixelAddress);
    }
  },

  computed: {
    pixelAddress: function() {
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

