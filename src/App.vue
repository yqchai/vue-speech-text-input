<template>
  <div id="app">
    <div  v-for="msg in messages">
      <p>{{ msg }}</p>
    </div>
    <speech-text-input :onSubmit="submitMsg"></speech-text-input>
  </div>
</template>

<script>
import SpeechTextInput from '@/components/SpeechTextInput.vue'
export default {
  name: 'app',
  data(){
    return {
      messages:[]
    }
  },
  components: {
    SpeechTextInput
  },
  methods:{
    submitMsg(value){
      this.messages.push(value)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
