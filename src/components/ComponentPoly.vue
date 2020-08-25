<template>
  <polygon @click="handleClick" :points="computedPoints" :style="computedStyle" />
</template>

<script>
export default {
  data() {
    return {
      //   sideLength: 100,
      apb: [0, 0, 100, 100, 0, 100],
      polyPoints: [],
      fillColor: "blue"
    };
  },

  props: ["xBias", "yBias", "item", "index", "sideLength"],
  methods: {
    handleClick() {
      this.fillColor = "red";
    }
  },
  computed: {
    computedStyle() {
      return "fill:" + this.fillColor + ";stroke:purple;stroke-width:1";
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

