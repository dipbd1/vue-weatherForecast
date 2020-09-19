<template>
  <div id="app">
    <helloText></helloText>
  </div>
</template>

<script>

import helloText from './views/helloText.vue';

export default {
  components: {
    helloText,
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
  margin: 0;
  padding: 0;
}
</style>
