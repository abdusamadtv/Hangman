<template>
  <div class="letters">
    <div
      v-for="(letter, index) in letters"
      :key="index"
      class="letter"
      :class="{ 'is-clicked': clickedLetters.includes(letter) }"
      @click="$emit('select', letter)"
    >
      {{ letter }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    letters: {
      type: Array,
      default: () => [],
    },
    clickedLetters: {
      type: Array,
      default: () => [],
    },
  },
}
</script>

<style lang="scss">
.letters {
  display: flex;
  flex-wrap: wrap;
  max-width: 555px;
  margin-top: 30px;
  justify-content: center;
  &.disabled {
    pointer-events: none;
  }
}
.letter {
  width: 30px;
  height: 30px;
  color: #fff;
  font-weight: 500;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  background-color: $primary-color;
  border: 2px solid $primary-color;
  margin-bottom: 5px;
  cursor: pointer;
  transition: 0.3s;
  &:hover {
    color: $primary-color;
    background-color: #fff;
  }
  &:not(:first-child) {
    margin-left: 5px;
  }
  &.is-clicked {
    opacity: 0.3;
    pointer-events: none;
  }
}
</style>
