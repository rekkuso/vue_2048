<template lang="pug">
#app
    #title
      img(alt="Vue logo" src="./assets/logo.png")
      label 2048
    Game
</template>

<script>
import Game from "../src/components/Game.vue"
export default {
  components: {
    Game
  }
}
</script>

<style lang="scss">
#title {
  text-align: center;
  font-size: 4em;
  font-family: sans-serif;
  font-weight: bold;
  img {
    height: 1em;
    vertical-align: bottom;
  }
}
</style>
