<template>
  <div id="app">
    <TTS_reader />
  </div>
</template>

<script>
import TTS_reader from "@/components/TTS-reader.vue";

export default {
  components: {
    TTS_reader,
  },
};
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
