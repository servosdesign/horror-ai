<template>
  <div class="input-wrapper">
    <div class="textbox">
      <textarea
        v-model="dialogueValue"
        maxlength="280"
        rows="1"
        placeholder="..."
        @input="updateDialogueValue($event.target.value)"
        @keyup.enter.exact="sendDialogue"
        @keydown.enter.exact.prevent
        @keydown.enter.shift.exact="newline"
      />
      <i
        class="fa fa-send"
        @click="sendDialogue"
      />
    </div>
  </div>
</template>

<script>
export default {
  data  () {
    return {
      dialogueValue: ''
    }
  },
  methods: {
    sendDialogue () {
      this.$emit('sendDialogue', this.dialogueValue)
      this.dialogueValue = ''
    },
    updateDialogueValue (value) {
      this.dialogueValue = value
    }
  }
}
</script>

<style scoped>

.input-wrapper {

  display: flex;
  justify-content: center;
  margin: 0 auto;
  width: 950px;
}

.textbox {
  margin-left: 83px;
  vertical-align: middle;
  width: 100%;
  height: auto;
  border: 2px solid black;
  border-radius: 8px;
}

textarea {
  background: transparent;
  border: none;
  outline: none;
  resize: none;
  margin-left: 13px;
  font-size: 20px;
  padding-top: 15px;
  width: 94%;
  height: auto;
}

textarea::placeholder{
  font-family: monospace;
  padding-top: 8px;
  padding-left: 0px;
  font-size: 12px;
}

i {
  font-size: 17px;
  margin-top: 13px;
  padding-left: 5px;
  position: absolute;
}

i:hover {
  cursor: pointer;
}
</style>
