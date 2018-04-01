<template>
  <div id="app">
    <Nav/>
    <NewInput/>
  </div>
</template>

<script>
import Nav from './components/Nav'
import NewInput from './components/NewInput'

export default {
  name: 'App',
  components: {
    Nav,
    NewInput
  }
}
</script>

<style lang="scss">
// Import fonts
@font-face {
  font-family: 'Proxima Nova';
  src: url('./assets/fonts/proximanova.ttf');
}

// Variables
$main-font: 'Proxima Nova', sans-serif;
$white: #fff;
$main-font-color: #3d3d3d;
$secondary-font-color: #41676c;






body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: $main-font;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  h1 {
    font-weight: normal;
  }
  h2, h3 {
    font-weight: bold;
  }
  h1, h2, h3 {
    color: $main-font-color;
    margin: 0;
    padding: 0;
  }
  p {
    color: $secondary-font-color;
  }
  a, a:visited, a:hover {
    color: $white;
  }
}
</style>
