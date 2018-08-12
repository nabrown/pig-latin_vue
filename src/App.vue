<template>
  <div>
    <h1>Get Piggy With It</h1>
    <form>
      <textarea v-focus id="input" v-model="phrase" v-on:keyup="setPiggifiedPhrase"></textarea>
    </form>
    <div class="display">{{ piggifiedPhrase }}</div>
    <app-footer></app-footer>
  </div>
</template>

<script>
import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    'app-footer': Footer
  },
  data () {
    return {
      phrase: '',
      piggifiedPhrase: '',
      suffix: 'ay',
      suffixBuffer: 'w'
    }
  },
  computed: {

  },
  methods: {
    piggifyWord: function (word) {
      var firstVowelIndex = word.indexOf(word.match(/[aeiouyAEIOUY]/)[0])
      var wordStart = word.substr(0, firstVowelIndex)
      var remainder = word.substr(firstVowelIndex)
      var newWord = word

      if (wordStart.length === 0) {
        newWord = word + this.suffixBuffer + this.suffix
      } else {
        newWord = remainder + wordStart + this.suffix
      }

      return newWord
    },
    piggifyChunk: function (chunk) {
      var word
      var startPunc = ''
      var endPunc = ''
      // non-empty array is truthy, null is falsey
      var chunkHasNoPunctuation = !!(chunk.charAt(0).match(/\w/) && chunk.charAt(chunk.length - 1).match(/\w/))

      if (chunkHasNoPunctuation) {
        word = chunk
      } else {
        // if chunk has funky char at beginning
        if (!chunk.charAt(0).match(/\w/)) {
          startPunc = chunk.charAt(0)
          word = chunk.slice(1)
        }
        // if chunk has funky char at end
        if (!chunk.charAt(chunk.length - 1).match(/\w/)) {
          endPunc = chunk.charAt(chunk.length - 1)
          word = chunk.slice(0, -1)
        }
      }
      return startPunc + this.piggifyWord(word) + endPunc
    },
    piggifyPhrase: function () {
      var phrase = this.phrase
      var chunks = phrase.split(/\s/)

      chunks = chunks.filter(chunk => chunk.length > 0)

      for (let i = 0; i < chunks.length; i++) {
        chunks[i] = this.piggifyChunk(chunks[i])
      }
      return chunks.join(' ')
    },
    setPiggifiedPhrase: function () {
      this.piggifiedPhrase = this.piggifyPhrase()
    }
  },
  directives: {
    focus: {
      // directive definition
      inserted: function (el) {
        el.focus()
      }
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  }
body{
  border-top: .5rem solid rgb(242,172,174);
  font-family: 'Alef', sans-serif;
  font-size: 20px;
  background: rgb(221,204,188);
  color: rgb(122,112,102);
  }
form{
  background: rgb(140,128,116);
  padding: 1rem;
}
textarea{
  width: 100%;
  height: 8em;
  display: block;
  border: 1px solid rgb(232, 167, 169);
  background-color: rgb(244,224,217);
  box-shadow: 0 0 4px inset rgba(140,128,116,.8);
  color: inherit;
  caret-color: rgb(232, 167, 169);
  }
textarea:focus{
  outline: none;
}
textarea, .display{ 
  font-size: 1.8em;
  padding: 1rem;
 }
legend{ display: block }
h1{
  font-size: 2.2rem;
  margin: 0;
  line-height: 2;
  text-align: center;
  color: rgb(242,172,174);
  background-color: rgb(122,112,102);
  }
.display {
  background-color: rgb(122,112,102);
  color: rgb(244,224,217);
  }
footer{
  text-align: center;
  padding: 1rem;
}
</style>
