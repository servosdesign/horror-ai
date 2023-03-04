<template>
  <div>
    <PlayBox />
  </div>
</template>

<script>
import PlayBox from './components/PlayBox.vue'

/*
    Todo:
      - Make main screen where you can link to play mode
      - Add routes to play mode
      - Add back button in play mode
      - Make dialogue have a typed animation
      - Make footer
      - Make header
      - Disable responses (sending responses to api request) when AI request is loading
      - Login and logout requred to play
      - Login with your own openai api key
      - Signup and use mine for price
      - Show amount of token used and use that for daily / monthly limits
*/

export default {
  components: {
    PlayBox
  },
  data  () {
    return {}
  }
}
</script>

<style>
* {
  font-family: 'Helvetica', 'Arial', sans-serif;
  background-color: #F5F5FA;
}
</style>
