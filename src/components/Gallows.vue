<template>
  <div class="gallows">
    <div class="sticks">
      <div ref="topStick" class="left" :class="{ active: tries < 7, hang: tries === 0 }">
        <div ref="victim" class="victim">
          <div class="head animate" :class="{ active: tries < 6 }"></div>
          <div class="body animate" :class="{ active: tries < 5 }">
            <div ref="victimLeftHand" class="left-hand" :class="{ active: tries < 4 }"></div>
            <div ref="victimRightHand" class="right-hand" :class="{ active: tries < 3 }"></div>
            <div ref="victimLeftLeg" class="left-leg" :class="{ active: tries < 2 }"></div>
            <div ref="victimRightLeg" class="right-leg" :class="{ active: tries < 1 }"></div>
          </div>
        </div>
      </div>
      <div class="top animate" :class="{ active: tries < 8 }"></div>
      <div class="right animate" :class="{ active: tries < 9 }"></div>
    </div>
    <div class="floor animate" :class="{ active: tries < 10 }"></div>
  </div>
</template>

<script>
import { TimelineLite, Power2, Bounce } from 'gsap'

export default {
  props: {
    tries: {
      type: Number,
      default: 0,
    },
  },
  watch: {
    tries(value) {
      if (value === 0) {
        this.animateHanging()
      }
    },
  },
  methods: {
    animateHanging() {
      const tl = new TimelineLite({
        delay: 1,
        onComplete: () => {
          this.$emit('game-over')
        },
      })
      const { victim, victimLeftHand, victimRightHand, victimLeftLeg, victimRightLeg } = this.$refs

      tl.to(victim, 0.25, { y: 40, ease: Power2.easeOut }, 0)
      tl.to(victim, 0.5, { y: 20, ease: Bounce.easeOut, delay: 0.25 }, 0)

      tl.to([victimLeftHand, victimLeftLeg], 0.25, { rotation: 30, ease: Power2.easeOut }, 0)
      tl.to([victimLeftHand, victimLeftLeg], 0.5, { rotation: -70, ease: Bounce.easeOut, delay: 0.25 }, 0)

      tl.to([victimRightHand, victimRightLeg], 0.25, { rotation: -30, ease: Power2.easeOut }, 0)
      tl.to([victimRightHand, victimRightLeg], 0.5, { rotation: 70, ease: Bounce.easeOut, delay: 0.25 }, 0)
    },
  },
}
</script>

<style scoped lang="scss">
$gallows-bg: #aaa;
$victim-color: #ffa93e;

.animate {
  transition: 1s;
}

.gallows {
  padding: 15px 0;
  position: relative;
  min-width: 290px;
  min-height: 250px;
  background-color: #eee;
}
.sticks {
  position: relative;
  padding-right: 30px;
  .left {
    width: 10px;
    height: 0;
    position: absolute;
    left: 90px;
    background-color: $gallows-bg;
    transition-duration: 0.25s;
    &.active {
      height: 30px;
    }
    &.hang {
      height: 70px;
      transition-delay: 1s;
    }
  }
  .top {
    width: 0;
    height: 10px;
    margin-left: auto;
    background-color: $gallows-bg;
    &.active {
      width: 170px;
    }
  }
  .right {
    width: 10px;
    height: 0;
    margin-left: auto;
    background-color: $gallows-bg;
    &.active {
      height: 200px;
    }
  }
}
.victim {
  position: absolute;
  left: 50%;
  top: 25px;
  width: 30px;
  transform: translate(-50%);
  .head {
    width: 30px;
    height: 30px;
    opacity: 0;
    border-radius: 100%;
    background-color: $victim-color;
    &.active {
      opacity: 1;
    }
  }
  .body {
    width: 6px;
    height: 0;
    left: 50%;
    transform: translate(-50%);
    position: absolute;
    background-color: $victim-color;
    &.active {
      height: 50px;
    }
  }
  .left-hand,
  .right-hand,
  .left-leg,
  .right-leg {
    width: 40px;
    height: 6px;
    opacity: 0;
    position: absolute;
    transition: opacity 1s;
    background-color: $victim-color;
    &.active {
      opacity: 1;
    }
  }
  .left-hand {
    top: 10px;
    right: 5px;
    transform: rotate(-45deg);
    transform-origin: top right;
  }
  .left-leg {
    right: 5px;
    bottom: -2px;
    transform: rotate(-45deg);
    transform-origin: top right;
  }
  .right-hand {
    top: 10px;
    left: 5px;
    transform: rotate(45deg);
    transform-origin: top left;
  }
  .right-leg {
    left: 5px;
    bottom: -2px;
    transform: rotate(45deg);
    transform-origin: top left;
  }
}
.floor {
  width: 0;
  height: 10px;
  position: absolute;
  left: 0;
  bottom: 15px;
  background-color: $gallows-bg;
  &.active {
    width: 290px;
  }
}
</style>
