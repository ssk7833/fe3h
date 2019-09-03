<template>
  <div id="app">
    <MealFilter/>
  </div>
</template>

<script>
import MealFilter from './components/MealFilter';

export default {
  name: 'App',
  components: {
    MealFilter,
  },
};
</script>

<style>
#app {
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 10px; */
  text-align: center;
}
</style>
