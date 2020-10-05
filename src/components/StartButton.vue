<template>
  <div>
    <button v-on:click="startSimulation" class="start-button">Start</button>
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

<style scoped>
.start-button {
  margin-top: 2rem;
  padding: 1rem 1.5rem;
  border: 1px solid black;
  border-radius: 5px;
}
</style>
