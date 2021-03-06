<template>
  <div
    class="white-container hidden-landscape hidden-md-and-up flex-column align-center justify-space-between"
  >
    <div ref="swipe-container" class="full-width">
      <div>
        <header class="text-center py-2">
          <div class="title">{{ params.leftTitle }}</div>
          <div v-show="params.showDailyIdeaSubTitle" class="daily-idea">
            <span class="daily">DAILY</span><span class="idea">IDEA</span>
          </div>
        </header>
        <div class="yellow-bg text-center d-flex flex-column align-center pb-3">
          <img :src="params.rightImage" />
          <div class="sub-image-text">{{ params.rightText }}</div>
        </div>
      </div>
      <div class="bullet-points mt-7">
        <div
          v-for="(bullet, index) in params.bulletPoints"
          :key="index"
          class="bullet-point py-2"
        >
          <span class="bullet w-25"></span>
          <span class="text">{{ bullet }}</span>
        </div>
      </div>
    </div>
    <footer class="d-flex flex-column text-center">
      <div v-show="!hideNextBtn">
        <v-btn class="my-2" @click="emitNext">Next</v-btn>
      </div>
      <div v-show="params.showBrowseIdeasBtn">
        <v-btn class="my-2" @click="emitMarkAsWelcomed">Browse Ideas</v-btn>
      </div>
      <div v-show="params.showWriteIdeasBtn">
        <v-btn class="my-2" @click="writeOwnIdea">Write my own idea</v-btn>
      </div>
      <div
        v-show="!params.hideExploreLink"
        class="explore-on-own"
        @click="emitMarkAsWelcomed"
      >
        I'll explore on my own
      </div>
      <div class="d-flex flex-row justify-center mt-5">
        <div
          :class="{ filled: params.one }"
          class="circle mx-2"
          @click="emitGoToOne"
        ></div>
        <div
          :class="{ filled: params.two }"
          class="circle mx-2"
          @click="emitGoToTwo"
        ></div>
        <div
          :class="{ filled: params.three }"
          class="circle mx-2"
          @click="emitGoToThree"
        ></div>
      </div>
    </footer>
    <div></div>
    <div></div>
    <div></div>
  </div>
</template>
<script>
export default {
  name: 'WelcomeMobilePortrait',
  props: {
    hideNextBtn: Boolean,
    params: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      touchStartPos: 0,
      touchEndPos: 0
    }
  },
  mounted() {
    this.setupSwipeEventListener()
  },
  methods: {
    emitGoToOne() {
      this.$emit('go-to-one')
    },
    emitGoToTwo() {
      this.$emit('go-to-two')
    },
    emitGoToThree() {
      this.$emit('go-to-three')
    },
    emitNext() {
      this.$emit('next')
    },
    emitMarkAsWelcomed() {
      this.$emit('mark-as-welcomed')
    },
    writeOwnIdea() {
      this.$emit('mark-as-welcomed')
      this.$router.push('/ideas/me')
    },
    emitRightClicked() {
      this.$emit('right-clicked')
    },
    emitLeftClicked() {
      this.$emit('left-clicked')
    },
    handleTouchStart(event) {
      const touch = event.touches[0].clientX
      this.touchStartPos = touch
    },
    handleTouchMove(event) {
      const touch = event.touches[0].clientX
      this.touchEndPos = touch
    },
    handleTouchEnd(event) {
      if (Math.abs(this.touchStartPos - this.touchEndPos) > 100) {
        if (this.touchStartPos < this.touchEndPos) {
          this.emitLeftClicked()
        } else {
          this.emitRightClicked()
        }
      }
    },
    setupSwipeEventListener() {
      const swipeContainer = this.$refs['swipe-container']
      swipeContainer.addEventListener('touchstart', this.handleTouchStart)
      swipeContainer.addEventListener('touchmove', this.handleTouchMove)
      swipeContainer.addEventListener('touchend', this.handleTouchEnd)
    }
  }
}
</script>
<style lang="scss" scoped>
@media (orientation: landscape) {
  .hidden-landscape {
    display: none !important;
  }
}

.full-width {
  width: 100vw;
}

.circle {
  width: 15px;
  height: 15px;
  border: 1px solid gray;
  border-radius: 100%;
}

.filled {
  background-color: gray;
}

.white-container {
  height: 100vh;
  background-color: white;
  display: flex;
}

.yellow-bg {
  background-color: $welcome-background-yellow;
  border-radius: 0 5px 5px 0;

  width: 100%;

  img {
    width: 50%;
  }

  .sub-image-text {
    font-weight: bold;
    font-size: 1.2rem;
    width: 90%;
  }
}

.title {
  font-size: 1.4rem;
  font-weight: bold;
}

.bullet-point {
  position: relative;
  margin: 0 auto;
  width: 80%;

  text-align: left;
  display: flex;

  .bullet {
    display: inline-block;
    width: 19px;
    position: absolute;
    height: 19px;
    border-radius: 100%;
    border: 2px solid $color-muted-grey;
    margin-right: 1rem;
    margin-top: 0.25rem;
  }

  .text {
    font-size: 1rem;
    margin-left: 2rem;
  }
}

.daily {
  color: $primary-color;
  font-size: 1.3rem;
  font-weight: bold;
}

.idea {
  color: $secondary-color;
  font-size: 1.3rem;
  font-weight: bold;
}

.v-btn {
  color: black !important;
  width: 100%;
  height: 50px !important;
  font-size: 0.9rem;
}

.explore-on-own {
  text-decoration: underline;
  color: $primary-color;
  font-size: 1.1rem;
  cursor: pointer;
  font-weight: 600;
  margin-top: 1rem;
}
</style>
