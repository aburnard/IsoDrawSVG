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
      fillColor: "blue"
    };
  },

  props: ["xBias", "yBias", "item", "index", "sideLength", "color", "downer"],
  methods: {
    handleClick() {
      this.fillColor = this.color;
    },
    handleEnter(message, event) {
      if (event.buttons == 1) {
        this.fillColor = this.color;
      }
    }
  },
  computed: {
    computedStyle() {
      return "fill:" + this.fillColor + ";stroke:purple;stroke-width:.1";
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

