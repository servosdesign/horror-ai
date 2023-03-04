<template>
  <div>
    <div
      id="style-1"
      ref="container"
      class="wrapper"
    >
      <div class="dialogue-wrapper">
        <div class="dialogue-line">
          <img
            class="master"
            src="../../../assets/images/master.png"
          > {{ aiDialogueValue[0] }}
          <div
            v-if="firstLoad"
            class="spinner"
          >
            <LoadingSpinner />
          </div>
        </div>
      </div>

      <div
        v-for="(userDialogue, index) in userDialogueValue"
        :key="userDialogue + index"
        class="dialogue-wrapper"
      >
        <div
          class="dialogue-line"
        >
          <img
            class="player"
            src="../../../assets/images/player.png"
          > {{ userDialogue }}
        </div>
        <div class="dialogue-line">
          <img
            class="master"
            src="../../../assets/images/master.png"
          > {{ aiDialogueValue[index + 1] }}
          <div
            v-if="loading && index === userDialogueValue.length - 1"
            :id="'spinner' + index"
            class="spinner"
          >
            <LoadingSpinner />
          </div>
        </div>
      </div>
    </div>
    <div>
      <DialogueBox @sendDialogue="sendDialogue" />
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import DialogueBox from '../../UI/DialogueBox.vue'
import LoadingSpinner from '../../UI/LoadingSpinner.vue'
import prompts from '../../../assets/configs/prompt-config'

const test = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer eleifend risus id elementum fermentum. Vivamus finibus, lacus id aliquet tincidunt, libero neque luctus quam, sit amet imperdiet nisl orci porttitor libero. Nam pulvinar ex neque, eget congue orci volutpat eleifend. Aenean fringilla velit vel malesuada aliquet. Aliquam nulla felis, semper eu sem non, placerat porta lacus. Nam vitae eros non augue ultricies blandit. Ut eget mauris eget turpis faucibus ornare. Duis pulvinar elementum dui vitae viverra. Praesent cursus porta nunc, vel ullamcorper erat scelerisque id.'

export default {
  components: {
    DialogueBox,
    LoadingSpinner
  },
  props: {
    themeChoice: {
      type: String,
      default: ''
    }
  },
  data  () {
    return {
      userDialogueValue: [],
      aiDialogueValue: [test],
      themePrompt: prompts,
      startingPrompt: '',
      loading: false
    }
  },
  created () {
    this.checkThemesSelected()
    // this.fetchAnswer('')
  },
  methods: {
    fetchAnswer (enteredValue) {
      const fetchAnswer = async () => {
        this.loading = true
        const formattedEnteredValue = this.addPeriod(enteredValue)
        const prompt = ref(formattedEnteredValue)
        try {
          const res = await fetch('http://localhost:8000', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              prompt: this.startingPrompt + this.aiDialogueValue.join(' ') + prompt.value
            })
          })
          const data = await res.json()
          const parsedData = data.bot.trim()
          this.aiDialogueValue.push(this.removeSentenceWithoutPunctuation(parsedData))
        } catch {
        }
        this.scrollToElement()
        this.loading = false
        this.firstLoad = false
      }
      fetchAnswer()
    },
    sendDialogue (enteredValue) {
      this.userDialogueValue.push(enteredValue)
      this.fetchAnswer(enteredValue)
      this.scrollToElement()
    },
    checkThemesSelected () {
      this.firstLoad = true
      if (this.themeChoice === 'Realistic') {
        this.startingPrompt = this.themePrompt.nonSupernaturalPrompt
      }
      if (this.themeChoice === 'Paranormal') {
        this.startingPrompt = this.themePrompt.paranormalPrompt
      }
      if (this.themeChoice === 'Psychological') {
        this.startingPrompt = this.themePrompt.psychologicalPrompt
      }
      if (this.themeChoice === 'Slasher') {
        this.startingPrompt = this.themePrompt.slasherPrompt
      }
    },
    addPeriod (string) {
      if ((string[string.length - 1] !== '.') || (string[string.length - 1] !== '?')) {
        return string + '.'
      }
      return string
    },
    removeSentenceWithoutPunctuation (str) {
      const sentences = str.split(/(?<=[.?!])\s+/)
      const lastSentence = sentences[sentences.length - 1]
      const lastChar = lastSentence[lastSentence.length - 1]

      if (lastChar === '.' || lastChar === '?') {
        return str
      } else {
        sentences.pop()
        return sentences.join(' ')
      }
    },
    scrollToElement () {
      const container = this.$refs.container
      this.$nextTick(
        () => (container.scrollTop = container.scrollHeight)
      )
    }
  }
}
</script>

<style scoped>
.wrapper {
  margin: 0 auto;
  background-color: rgba(128, 128, 128, 0);
  height: 800px;
  overflow: auto;
  overflow-x: hidden;
  word-break: break-word;
  overflow-wrap: break-word;
  width: 950px;
}

.dialogue-wrapper{
  margin: 0 auto;
  width: 850px;
}

.spinner {
  display: flex;
  flex-wrap: row;
  background-color: transparent;
  padding-top: 14px;
  margin-left: 20px;
  height: 20px;
  width: 109px;
}

img {
  display: flex;
  flex-wrap: row;
  padding-top: 2px;
  margin-right: 10px;
  height: 20px;
  width: 20px;
}

.dialogue-line {
  display: flex;
  padding-left: 40px;
  padding-right: 40px;
  padding-top: 20px;
  padding-bottom: 20px;
  text-align: left;
  font-size: 22px;
  width: 850px;
}

#style-1::-webkit-scrollbar-track
{
-webkit-box-shadow: inset 0 0 6px rgba(122, 122, 122, 0.3);
  border-radius: 10px;
  background-color: #F5F5F5;
}

#style-1::-webkit-scrollbar
{
  width: 12px;
  background-color: #F5F5F5;
}

#style-1::-webkit-scrollbar-thumb
{
  border-radius: 10px;
-webkit-box-shadow: inset 0 0 6px rgba(59, 59, 59, 0.3);
  background-color: rgb(241, 241, 241);
}

</style>
