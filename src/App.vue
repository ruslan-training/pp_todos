<template>
  <div class="app" v-cloak>
    <div class="app__wrapper">
      <h1>TODOS list</h1>
      <vToDo></vToDo>
    </div>
  </div>
</template>

<script>
import vToDo from './components/v-todos.vue'

export default {
  name: 'App',
  components: {
    vToDo
  }
}
</script>

<style lang="scss">
body {
  background-color: #eee;
}
  .app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    padding-top: 15px;

    h1 {
      color: $mainColor;
      font-size: 1.5em; 
      margin-bottom: 1em; 
    }
    &__wrapper {
      max-width: 500px;
      margin: 0 auto;
    }
  }
  [v-cloak] {
    display: none;
  }
</style>
