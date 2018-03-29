<template>
  <div id="app">
    <app-todo></app-todo>
  </div>
</template>

<script>
import Todo from './components/Todo.vue';

export default {
  name: 'App',
  components: {
    'app-todo':Todo
  },
  data() {
    return {
      testData: 'Hello World'
    }
  },
  beforeCreate() {
    console.log("1. BeforeCreate hook created was called, no constructor was created yet", this.testData); 
  },
  created() {
    console.log("2. Created hook was called, like a constructor", this.testData);
  },
  beforeMount() {
    console.log("3. beforeMount hook was called before we the DOM is rendered.");
  },
  mounted() {
    console.log("4. Mounted hook was called after the DOM is rendered");
  },
  beforeUpdate() {
    console.log("5. beforeUpdate hook was called whenever a data has changed before it updates the component DOM");
  },
  updated() {
    console.log("6. updated hook was called whenever the DOM component has already updated data");
  },
  beforeDestroy() {
    console.log("7. beforeDestroy hook was called before the DOM will  be destroyed");
  },
  destroyed() {
    console.log("8. destroyed hook was called after the DOM has been destroyed");
  }
}
</script>

<style>
body{
  margin:0;
  padding:0;
  background-color: #f0f0f0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0;
}
</style>

