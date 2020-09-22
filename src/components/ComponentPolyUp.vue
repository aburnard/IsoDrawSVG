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
      //   sideLength: 100,
      apb: [0, 0, 100, 100, 0, 100],
      polyPoints: [],
      localFill: "",
      fillColor: this.pixMatrixUp[this.yCoord][this.xCoord]
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
    "pixMatrixUp"
  ],
  methods: {
    handleClick() {
      //this.fillColor = this.color;
      this.localFill = this.color;
      this.$emit("colorPixelUp", this.pixelAddress);
      this.id++;
    },
    handleEnter(message, event) {
      if (event.buttons == 1) {
        this.localFill = this.color;
        this.$emit("colorPixelUp", this.pixelAddress);
        this.id++;
      }
    }
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
    pixelAddress: function() {
      return [this.yCoord, this.xCoord];
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
      return this.xBias + this.sideLength / 2;
    },
    pointA2() {
      return this.yBias + this.sideLength;
    },
    pointB() {
      return this.xBias + this.sideLength * 1.5;
    },
    pointB2() {
      return this.yBias + this.sideLength;
    },
    pointC() {
      return this.sideLength + this.xBias;
    },
    pointC2() {
      return this.yBias;
    }
  }
};
</script>

