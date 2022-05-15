<template>
  <div id="toast" :class="toastVisible ? 'visible' : ''">
    <span v-text="error" />
  </div>
  <div class="word-display">
    <div class="words">
      <div v-for="(displayRow, idx) in parsedAtempts" :key="idx" class="word" :class="{shake: idx == row && error, complete: idx < row}">
        <div v-for="(letter, letterIndex) in displayRow" :key="letterIndex" class="letter" :class="letterClass(letter, letterIndex)">
        <span v-text="letter.toUpperCase()" />
        </div>
        </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import type { PropType } from 'vue'
export default defineComponent({
  name: 'WordsDisplay',
  props: {
    word: {
      type: String,
      required: true
    },
    row: {
      type: Number,
      required: true
    },
    atempts: {
      type: Array as PropType<Array<string>[]>,
      required: true
    },
    error: {
      type: String
    }
  },
  emits: ['reset-error'],
  data () {
    return {
      toastVisible: false
    }
  },
  watch: {
    error (newError) {
      if (newError) {
        console.log('error', newError)
        this.showToast()
      }
    }
  },
  computed: {
    parsedAtempts () {
      return this.atempts.map(row => row.concat(Array(this.word.length - row.length).fill('')))
    }
  },
  methods: {
    showToast () {
      this.toastVisible = true
      setTimeout (() => {
        this.toastVisible = false
      }, 2500) // the difference between timeout values is the transition duration
      setTimeout(() => {
        this.$emit('reset-error')
      }, 3000)
    },
    letterClass (letter: string, idx: number) {
      return {
        filled: letter,
        hit: this.word[idx] === letter,
        miss: !this.word.includes(letter),
        present: this.word[idx] !== letter && this.word.includes(letter)
      }
    }
  }
})
</script>

<style scoped>
#toast {
  position: fixed;
  top: 60px;
  /* margin-left: ; */
  width: fit-content;
  height: fit-content;
  background: rgb(255,255,255);
  color: black;
  padding: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  pointer-events: none;
  box-shadow: black 0 0px 2px 1px;
  border: 1px solid rgb(104, 104, 104);
  border-radius: 5px;
  opacity: 0;
  transition: opacity 0.5s ease-in-out;
}
#toast.visible {
  opacity: 1;
  transition: opacity 0.5s;
}
.shake {
  animation: shake 0.82s cubic-bezier(.36,.07,.19,.97) both;
  /* transform: translate3d(0, 0, 0); */
  /* backface-visibility: hidden; */
  /* perspective: 1000px; */
}
@keyframes shake {
  0% {
    transform: translateX(0);
  }
  10% {
    transform: translateX(-10px);
  }
  20% {
    transform: translateX(10px);
  }
  30% {
    transform: translateX(-10px);
  }
  40% {
    transform: translateX(10px);
  }
  50% {
    transform: translateX(-10px);
  }
  60% {
    transform: translateX(10px);
  }
  70% {
    transform: translateX(-10px);
  }
  80% {
    transform: translateX(10px);
  }
  90% {
    transform: translateX(-10px);
  }
  100% {
    transform: translateX(0);
  }
}
.word-display {
  justify-self: center;
  justify-items: center;
  align-items: center;
  height: 100%;
  width: 100%;
}
.words {
  display: flex;
  flex-direction: column;
}
.word {
  display: flex;
  flex-grow: 1;
}
.letter::before {
  content: "";
  padding-bottom: 100%;
  display: block;
}
.letter {
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid rgb(54, 54, 54);
  border-radius: 3px;
  margin: 4px;
  width: calc(100% / 5 - 4px);
  height: calc(100% / 5 - 4px);
  font-size: 4rem;
  color: #fff;
}
.filled {
    border: 2px solid rgb(78, 78, 78);
    animation: small-scale 0.25s;
}

@keyframes small-scale {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}
.letter > span {
  position: absolute;
}
.complete > .miss {
  background-color: #2c2c2e;
}
.complete > .hit {
  background-color: #447d3d;
}
.complete > .present {
  background-color: #a5902d;
}
.complete > .miss, .complete > .hit, .complete > .present {
  border-color: #2c2c2e;
  transform: rotateX(360deg);
  transition: transform 0.5s;
  backface-visibility: visible;
}
</style>