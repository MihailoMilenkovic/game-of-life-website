<template>
  <div>
    <button v-on:click="startSimulation">Start</button>
  </div>
</template>

<script>
export default {
  name: "StartButton",
  methods: {
    startSimulation() {
      this.$emit("startSimulation");
    },
  },
};
</script>
