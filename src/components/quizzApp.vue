<template>
  <div class="hello">
    <h1>{{ currentWord.word }}</h1>
    <p>{{ currentWord.definition }}</p>
    <p>{{ currentWord.ipa }}</p>
    {{ currentWord[0] }}
    <hr>
    <!-- <div :class="['result', this.correct ? 'correct' : 'wrong']" v-if="!isDisabled">{{ correct ? 'correct' : 'wrong' }}</div> -->
    <span class="syllables"  :disabled="!isDisabled" @click="checkAnswer($event)" v-for="(x,i) in currentWord.syllables " :data-id="i" :key="x">{{ x }}</span>
  </div>
  <div :class="['resultBox', this.correct ? 'correct' : 'wrong']" v-if="!isDisabled">
    <p>{{ correct ? 'correct' : 'wrong' }}</p>
    <span>[</span>
    <span :class="i == currentWord.stress ? 'underline' : ''  " v-for="(x,i) in currentWord.syllables" :key="i"> {{ i == currentWord.stress ? "'" + x: x }}</span>
    <span>]</span>
    <br>
    <button :disabled='isDisabled' @click="pickWord()">next question</button>
  </div>
</template>

<script>
export default {
  name: 'quizzApp',
  data() {
    return {
      currentWord: '',
      currentIndex:0,
      correct: false,
      isDisabled: true,
      words: [
        {
          word: 'brasil',
          syllables: ['bra','sil'],
          ipa: '/brazil/',
          definition: 'brazil',
          audio: '',
          stress: 1
        },
        {
          word: 'você',
          syllables: ['vo','cê'],
          audio: '',
          stress: 1
        },
        {
          word: 'cego',
          syllables: ['ce','go'],
          ipa: '/sεgu/',
          definition: 'blind',
          audio: '',
          stress: 0
        },
        
      ],
      shuffledWords: []
    }
  },
  methods: {
    shuffled() {
      let arr = this.words
      while(arr.length > 0) {
        let random = Math.floor(Math.random()*arr.length)
        this.shuffledWords.push(arr[random])
        arr.splice(random,1)
      }
      // console.log(this.shuffledWords)
},
    pickWord() {
      if(!this.shuffledWords[this.currentIndex]) this.currentIndex = 0
      this.isDisabled = true
      this.currentWord = this.shuffledWords[this.currentIndex]
      this.currentIndex ++
    },
    checkAnswer(event) {
      if(this.isDisabled) {
        if(event.target.getAttribute('data-id') == this.currentWord.stress) {
        this.correct = true
      } else {this.correct = false}
      this.isDisabled = false
    }
      }
      
  },
  mounted() {
    this.shuffled()
    this.pickWord()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
span{
  font-size: 2em;
  font-weight: 500;
  margin: 2px;
}

.syllables:hover {
  text-decoration: underline;
}

.selected {
  background: #42b983;
}

.result{
  position: absolute;
  z-index:1;
  border: 1px solid grey;
  width: 60px;
  top:100px;
  left: 0; 
  right: 0; 
  margin-left: auto; 
  margin-right: auto;
  margin: 0 auto;
}
.correct{
  background-color: #42b983;
}
.wrong{
  background-color: red;
}

.resultBox{
  position:absolute;
  bottom: 0px;
  width: 100%;
  height: 300px;
}
button {
  margin-top: 10px;
}
.underline{
  text-decoration: underline;
}
</style>
