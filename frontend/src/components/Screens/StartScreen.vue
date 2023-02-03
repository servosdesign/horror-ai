<template>
  <div class="wrapper">
    <div v-if="themeChoice === ''">
      <div class="header">
        <img
          class="master"
          src="../../assets/images/master.png"
        ><h1>Please pick a main theme for your Horror.ai experience.</h1>
      </div>
      <ul>
        <li>
          <button @click="toggleThemeChoice('Realistic')">
            <h2>Realistic</h2>
          </button>
        </li>
        <li>
          <button @click="toggleThemeChoice('Paranormal')">
            <h2>Paranormal</h2>
          </button>
        </li>
        <li>
          <button @click="toggleThemeChoice('Psychological')">
            <h2>Psychological</h2>
          </button>
        </li>
        <li>
          <button @click="toggleThemeChoice('Slasher')">
            <h2>Slasher</h2>
          </button>
        </li>
        <li>
          <button @click="randomizeThemeChoice()">
            <h2>Random</h2>
          </button>
        </li>
      </ul>
    </div>
    <div v-else>
      <MainScreen :theme-choice="themeChoice" />
    </div>
  </div>
</template>

<script>
import MainScreen from './MainScreen.vue'

export default {
  components: {
    MainScreen
  },
  data  () {
    return {
      themeChoice: ''
    }
  },
  methods: {
    toggleThemeChoice (choiceName) {
      this.themeChoice = choiceName
    },
    randomizeThemeChoice () {
      const themeChoices = ['Realistic', 'Paranormal', 'Psychological', 'Slasher']
      let randomizedThemeChoice = this.shuffleArray(themeChoices)
      randomizedThemeChoice = randomizedThemeChoice[Math.floor(Math.random() * randomizedThemeChoice.length)]
      this.themeChoice = randomizedThemeChoice
    },
    shuffleArray (array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]]
      }
      return array
    }
  }
}
</script>

<style scoped>
.wrapper {
  margin-top: 50px;
}

h1 {
  text-align: center;
  font-weight: normal;
  font-size: 22px;
  width: 548px;
  margin: 0;
}

.header {
  display: flex;
  justify-content: center;
  margin-bottom: 50px;
}

img {
  padding-top: 2px;
  margin-right: 3px;
  height: 20px;
  width: 20px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0 auto;
  width: 600px;
}

li{
  display:flex;
  justify-content: center;
  margin-bottom:25px;
  height: 48px;
  width: 600px;
}

h2 {
  font-weight: normal;
}

button{
  text-align: center;
}

/* CSS */
button {
  align-items: center;
  background: #f5f5fa;
  border: 0;
  border-radius: 8px;
  box-shadow: -10px -10px 30px 0 #fff,10px 10px 30px 0 #1d0dca17;
  box-sizing: border-box;
  color: #2a1f62;
  cursor: pointer;
  display: flex;
  font-family: "Cascadia Code",Consolas,Monaco,"Andale Mono","Ubuntu Mono",monospace;
  font-size: 1rem;
  justify-content: center;
  line-height: 1.5rem;
  padding: 15px;
  position: relative;
  text-align: left;
  transition: .2s;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  white-space: pre;
  width: 170px;
  word-break: normal;
  word-spacing: normal;
}

button:hover {
  background: #f8f8ff;
  box-shadow: -15px -15px 30px 0 #fff, 15px 15px 30px 0 #1d0dca17;
}

</style>
