<template>
  <div id="app">
    <Animation/>
  </div>
</template>

<script>
import Animation from './components/Animation.vue'

export default {
  name: 'App',
  components: {
    Animation
  }
}
</script>

<style>
@font-face {
  font-family: "gibson-regular";
  src: url(./fonts/GibsonRegular.woff);
}
@font-face {
  font-family: "gibson-bold";
  src: url(./fonts/GibsonBoldIt.woff);
}
#app {
  text-align: center;
  color: #2c3e50;
  background-color: rgb(69, 246, 108);
}
* {
  margin: 0;
  padding: 0;
  overflow-y: hidden;
  overflow-x: hidden;
}
h1 {
  font-family: "gibson-bold";
  color: #ffffff;
}
h2,
h3 {
  font-family: "gibson-regular";
  color: #ffffff;
}
</style>
