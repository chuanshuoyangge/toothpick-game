<template>
  <div class="toothpick" v-bind:class="{ picked: isPicked }"></div>
</template>

<script>
export default {
  name: "Toothpick",
  props: ["isPicked"],
};
</script>

<style scoped>
.toothpick {
  height: 50px;
  width: 10px;
  background-color: brown;
}
.picked {
  background-color: antiquewhite;
}
</style>