<template>
  <svg>
    <ComponentPoly
      v-for="n in 10"
      :key="n"
      :xBias="n * xBias"
      :yBias="n*xBias"
      :sideLength="sideLength"
    />
    <ComponentPolyUp
      v-for="n in 10"
      :key="n"
      :xBias="n * xBias"
      :yBias="0"
      :sideLength="sideLength"
    />
  </svg>
</template>
<script>
export default {
  props: ["xBias", "yBias", "item", "index", "sideLength"],
  data() {}
};
</script>