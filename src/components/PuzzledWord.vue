<template>
  <div class="puzzled-word">
    <div
      v-for="(letter, index) in puzzledWord"
      :key="index"
      class="dash"
      :class="{ 'is-guessed': guessedLetters.includes(letter) }"
    >
      {{ letter }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    puzzledWord: {
      type: String,
      default: '',
    },
    guessedLetters: {
      type: Array,
      default: () => [],
    },
  },
}
</script>

<style lang="scss" scoped>
.puzzled-word {
  display: flex;
  flex-wrap: wrap;
  margin-top: 30px;
}
.dash {
  color: #fff;
  width: 30px;
  height: 30px;
  display: flex;
  font-size: 30px;
  font-weight: bold;
  align-items: center;
  padding-bottom: 10px;
  border-bottom: 3px solid $primary-color;
  justify-content: center;
  text-transform: uppercase;
  transition: border-color 0.3s;
  &.is-guessed {
    color: $primary-color;
    border-color: #fff;
  }
  &:not(:first-child) {
    margin-left: 30px;
  }
}
</style>
