<template>
  <Scichart2d msg="2D" />
</template>

<script>
import { defineComponent } from "vue";
import Scichart2d from "./components/Scichart2d.vue";

export default defineComponent({
  name: "App",
  components: { Scichart2d },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
