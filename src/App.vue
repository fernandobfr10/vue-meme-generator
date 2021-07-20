<template>
  <div id="app">
    <MemeGenerator />
  </div>
</template>

<script>
import MemeGenerator from './components/MemeGenerator'

export default {
  name: 'App',
  components: {
    MemeGenerator
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #eee;
}

button, input {
  outline: none;
}

button {
  cursor: pointer
}

</style>
