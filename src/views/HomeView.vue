<template>
  <main>
    {{ `word is: ${word}` }}
  <WordsDisplay :word="word" :row="row" :atempts="atempts" :error="error" @reset-error="error = ''" />
  <RoKeyboard :characters="characters" @key="keyPress" @enter="enterPress" @backspace="delPress" />
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import RoKeyboard from '@/components/RoKeyboard.vue'
import WordsDisplay from '@/components/WordsDisplay.vue'

import type { KeyCap as KeyCapType } from "@/types/KeyCap"
// import type { PropType } from 'vue'

export default defineComponent({
  name: 'HomeView',
  components: {
    RoKeyboard,
    WordsDisplay
  },
  data () {
    return {
      characters: [
          { key: 'q', status: null },
          { key: 'w', status: null },
          { key: 'e', status: null },
          { key: 'r', status: null },
          { key: 't', status: null },
          { key: 'y', status: null },
          { key: 'u', status: null },
          { key: 'i', status: null },
          { key: 'o', status: null },
          { key: 'p', status: null },
          { key: 'ă', status: null },
          { key: 'î', status: null }, // end first row
          { key: 'a', status: null },
          { key: 's', status: null },
          { key: 'd', status: null },
          { key: 'f', status: null },
          { key: 'g', status: null },
          { key: 'h', status: null },
          { key: 'j', status: null },
          { key: 'k', status: null },
          { key: 'l', status: null },
          { key: 'ș', status: null },
          { key: 'ț', status: null },
          { key: 'â', status: null }, // end second row
          { key: 'z', status: null },
          { key: 'x', status: null },
          { key: 'c', status: null },
          { key: 'v', status: null },
          { key: 'b', status: null },
          { key: 'n', status: null },
          { key: 'm', status: null }, // end third row
      ] as KeyCapType[],
      row: 0,
      atempts: [
        [],
        [],
        [],
        [],
        [],
        []
      ] as Array<string>[],
      error: '',
      words: [
        'găsit',
        'găină',
        'șapcă',
        'baros',
        'front',
        'cântă',
        'gunoi',
        'bursă',
        'forjă',
        'fazan'
      ] as string[]
    }
  },
  computed: {
    word (): string {
      let startOfTime = new Date('2022-05-09')
      let now = new Date()
      return this.words[Math.floor((now.getTime() - startOfTime.getTime()) / (1000 * 3600 * 24) % this.words.length)]
    }
  },
  methods: {
    keyPress (key: string) {
      const currentRow = this.atempts[this.row]
      if (currentRow.length < 5) {
        currentRow.push(key)
        this.atempts.splice(this.row, 1, currentRow)
      }
      console.log(currentRow)
      // console.log(key)
    },
    enterPress () {
      if (this.word === this.atempts[this.row].join('')) {
        this.$emit('success')
        console.log('success')
        this.row++
        return
      }

      if (this.atempts[this.row].length < 5) { // TODO: replace 5 with variable
        this.error = 'Nu ați introdus destule caractere'
        console.log('invalid - too few characters')
        return
      }

      const wordExists = this.words.includes(this.atempts[this.row].join(''))
       if (wordExists) {
        // this.$emit('fail')
        console.log('miss')
        if (this.row < 7) {
          this.row++
        } else {
          console.log('fail - end game')
          this.error = 'Joc încheiat'
        }
      } else if (!wordExists) {
        console.log('invalid - word not found')
        this.error = 'Cuvânt inexistent'
      }
    },
    delPress () {
      const currentRow = this.atempts[this.row]
      if (currentRow.length > 0) {
        currentRow.pop()
        this.atempts.splice(this.row, 1, currentRow)
      }
      console.log(currentRow)
    },
  }
})
</script>

<style scoped>
main {
  user-select: none;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}
</style>
