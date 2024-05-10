<template>
  <div id="app">
    <ExcelToYamlConverter />
  </div>
</template>

<script>
import ExcelToYamlConverter from './components/ExcelToYamlConverter.vue';

export default {
  components: {
    ExcelToYamlConverter,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  background-color: #5a6069;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
