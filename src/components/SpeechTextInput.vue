<template>
  <div>
    <form class="sc-user-input" :class="{active: inputActive}">
      <div
        role="button"
        tabIndex="0"
        @focus="setInputActive(true)"
        @blur="setInputActive(false)"
        @keydown="handleKey"
        contentEditable="true"
        class="sc-user-input--text"
        ref="userInput"
        :style="{color: color}"
      >
        <div>
            <p v-for="word in transcription" v-bind:key="word">{{ word }}</p>
            <p>{{ runtimeTranscription }}</p>
        </div>
      </div>
      <div class="start-record"><mic-icon class="sc-user-input--button" :onClick="record" :color="mic_color"></mic-icon></div>
        <div class="sc-user-input--button">
          <SendIcon :onClick="_submitText" />
        </div>
    </form>
  </div>
</template>


<script>
import MicIcon from './MicIcon.vue'
import SendIcon from './SendIcon.vue'
export default {
  components: {
    SendIcon,
    MicIcon
  },
  props: {
    onSubmit: {
      type: Function,
      required: true
    },
    placeholder: {
      type: String,
      default: 'Write a reply'
    },
    color: {
      type: String,
      default: 'black'
    },
    lang:{
      type:String
    }
  },
  data () {
    return {
      inputActive: false,
       is_start:false,
    recognition:null,
    runtimeTranscription: '',
    transcription: []
    }
  },
  computed:{
    mic_color(){
      return this.is_start? 'red':'black'
    }
  },
  methods: {
    record(){
      if (this.is_start){
        console.log('already started')
      } else {
        this.checkApi()
        this.is_start = true
      }
    },
    setInputActive (onoff) {
      this.inputActive = onoff
    },
    handleKey (event) {
      if (event.keyCode === 13 && !event.shiftKey) {
        this._submitText(event)
        event.preventDefault()
      }
    },
    _submitText () {
      const text = this.$refs.userInput.textContent
        if (text && text.length > 0) {
          this.onSubmit(text)
          this.runtimeTranscription = ''
          this.transcription = []
          this.is_start = false
          this.$refs.userInput.childNodes[0].nodeValue = ''
            this.$refs.userInput.childNodes[0].innerHTML = ''
        }
    },
    checkApi () {
      window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition
      this.recognition = new window.webkitSpeechRecognition()
      this.recognition.lang = this.lang
      this.recognition.interimResults = true
      this.recognition.addEventListener('result', event => {
        const text = Array.from(event.results)
          .map(result => result[0])
          .map(result => result.transcript)
          .join('')
        this.runtimeTranscription = text
      })
      this.recognition.addEventListener('end', () => {
        if (this.runtimeTranscription !== '') {
          this.transcription.push(this.runtimeTranscription)
          this.$emit('onTranscriptionEnd', {
            transcription: this.transcription,
            lastSentence: this.runtimeTranscription
          })
        }
        this.runtimeTranscription = ''
        this.is_start= false
        return
      })
      this.recognition.start()
    }
  }
}
</script>

<style>
.sc-user-input {
  min-height: 55px;
  margin: 0px;
  position: relative;
  bottom: 0;
  display: flex;
  background-color: #FAFAFA;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  transition: background-color 0.2s ease, box-shadow 0.2s ease;
}

.sc-user-input--text {
  width: 300px;
  resize: none;
  border: none;
  outline: none;
  border-bottom-left-radius: 10px;
  box-sizing: border-box;
  padding: 18px;
  font-size: 15px;
  font-weight: 400;
  line-height: 1.33;
  white-space: pre-wrap;
  word-wrap: break-word;
  color: #565867;
  -webkit-font-smoothing: antialiased;
  max-height: 200px;
  overflow: scroll;
  bottom: 0;
  overflow-x: hidden;
  overflow-y: auto;
}

.sc-user-input--text:empty:before {
  content: attr(placeholder);
  display: block; /* For Firefox */
  /* color: rgba(86, 88, 103, 0.3); */
  /*filter: contrast(15%);*/
  outline: none;
}

.sc-user-input--buttons {
  width: 100px;
  position: absolute;
  right: 30px;
  height: 100%;
  display: flex;
}

.sc-user-input--button:first-of-type {
  width: 40px;
}

.sc-user-input--button {
  width: 30px;
  height: 55px;
  margin-left: 2px;
  margin-right: 2px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.sc-user-input.active {
  box-shadow: none;
  background-color: white;
  box-shadow: 0px -5px 20px 0px rgba(150, 165, 190, 0.2);
}

.sc-user-input--button label {
  position: relative;
  height: 24px;
  padding-left: 3px;
  cursor: pointer;
}

.sc-user-input--button label:hover path {
  fill: rgba(86, 88, 103, 1);
}

.sc-user-input--button input {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  z-index: 99999;
  height: 100%;
  opacity: 0;
  cursor: pointer;
  overflow: hidden;
}

.icon-file-message {
  margin-right: 5px;
}
  .start-record{
 position: absolute;
  left:0px;
  top: 25px;
  z-index: 10;
}

</style>
