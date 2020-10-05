<template>
  <div
    class="square"
    :class="{ alive: this.isAlive }"
    v-on:click="clickedSquare"
  ></div>
</template>

<script>
export default {
  name: "Square",
  props: ["isAlive", "row", "col"],
  methods: {
    clickedSquare: function() {
      //console.log("clicked");
      this.$emit("clickedSquare", this.row, this.col);
    },
  },
};
</script>

<style scoped>
.square {
  width: 50px;
  height: 50px;
  display: inline-block;
}
.alive {
  background: black;
}

:not(.alive) {
  background: white;
}
</style>
