<template>
  <div id="app">
    <h1>Hangman</h1>
    <div class="row">
      <Gallows :tries="tries" @game-over="setLoseState()" />
      <Letters
        :class="{ disabled: currentPlayer === 'computer' }"
        :letters="letters"
        :clicked-letters="clickedLetters"
        @select="selectLetter"
      />
      <PuzzledWord :guessed-letters="guessedLetters" :puzzled-word="puzzledWord" />
      <div class="actions">
        <button @click="reset()">Reset</button>
        <button @click="playWithComputer()">Torment computer</button>
      </div>
    </div>
  </div>
</template>

<script>
import words from '@/data/words'
import letters from '@/data/letters'
import isAlpha from 'validator/lib/isAlpha'

const TRIES_AMOUNT = 10

export default {
  name: 'App',
  components: {
    Gallows: () => import('@/components/Gallows'),
    Letters: () => import('@/components/Letters'),
    PuzzledWord: () => import('@/components/PuzzledWord'),
  },
  data: () => ({
    words,
    letters,
    tries: TRIES_AMOUNT,
    currentPlayer: 'human',
    puzzledWord: '',
    userWord: {
      time: new Date().getTime(),
      word: '',
    },
    computerInterval: {},
    guessedLetters: [],
    clickedLetters: [],
  }),
  computed: {
    isGuessed() {
      return this.puzzledWord.split('').every(letter => this.guessedLetters.includes(letter))
    },
  },
  watch: {
    isGuessed(value) {
      value && this.setWonState()
    },
    userWord: {
      deep: true,
      handler(value) {
        if (value.word.length < 3 || !isAlpha(value.word)) {
          this.getUsersWord('Please insert word with length more than 3 which contains only letters')
        }
      },
    },
  },
  created() {
    this.pickWord()
  },
  methods: {
    pickWord() {
      const randomIndex = Math.floor(Math.random() * words.length)

      this.puzzledWord = this.words[randomIndex]
    },
    pickLetter() {
      const randomIndex = Math.floor(Math.random() * letters.length)

      this.selectLetter(letters[randomIndex])
    },
    selectLetter(letter) {
      this.clickedLetters.push(letter)

      if (this.puzzledWord.includes(letter)) {
        this.guessedLetters.push(letter)
      } else {
        this.fail()
      }
    },
    fail() {
      if (this.tries !== 0) {
        this.tries -= 1
      }
    },
    reset() {
      this.pickWord()
      this.tries = TRIES_AMOUNT
      this.guessedLetters = []
      this.clickedLetters = []
      this.currentPlayer = 'human'
      clearInterval(this.computerInterval)
    },
    playWithComputer() {
      this.reset()
      this.getUsersWord('Insert your word')
      this.currentPlayer = 'computer'
      this.puzzledWord = this.userWord.word
      this.computerInterval = setInterval(() => {
        this.pickLetter()
      }, 500)
    },
    getUsersWord(message) {
      this.userWord.time = new Date().getTime()
      this.userWord.word = prompt(message)
    },
    setWonState() {
      const message = this.currentPlayer === 'computer' ? 'Computer won!' : 'You won!'

      setTimeout(() => {
        alert(message)
        this.reset()
      }, 300)
    },
    setLoseState() {
      const message = this.currentPlayer === 'computer' ? 'Computer has lost :(' : 'Game over :('

      alert(message)
      this.reset()
    },
  },
}
</script>

<style lang="scss">
#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: $primary-color;
}
body {
  margin: 0;
}
div {
  box-sizing: border-box;
}
.row {
  display: flex;
  height: 1000px;
  padding: 0 15px;
  flex-direction: column;
  align-items: center;
}
.actions {
  display: flex;
  margin-top: 30px;
}
button {
  color: #fff;
  border: 2px solid $primary-color;
  border-radius: 5px;
  cursor: pointer;
  padding: 10px 15px;
  transition: 0.3s;
  background-color: $primary-color;
  &:hover {
    color: $primary-color;
    background-color: #fff;
  }
  &:not(:first-child) {
    margin-left: 15px;
  }
}
</style>
