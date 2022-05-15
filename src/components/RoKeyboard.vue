<template>
  <div class="keyboard">
    <div class="keyboard-row">
      <KeyCap
        v-for="(keyCap, keyIdx) in characters.slice(0, 12)"
        :key="keyIdx"
        :letter="keyCap"
        @click="keyClick(keyCap.key)"
      >
      </KeyCap>
    </div>
    <div class="keyboard-row">
      <KeyCap
        v-for="(keyCap, keyIdx) in characters.slice(12, 24)"
        :key="keyIdx"
        :letter="keyCap"
        @click="keyClick(keyCap.key)"
      >
      </KeyCap>
    </div>
    <div class="keyboard-row">
      <KeyCap @click="keyClick('enter')">
        <span  v-text="'ENTER'" />
      </KeyCap>
      <KeyCap
        v-for="(keyCap, keyIdx) in characters.slice(24, 30)"
        :key="keyIdx"
        :letter="keyCap"
        @click="keyClick(keyCap.key)"
      >
      </KeyCap>
      <KeyCap @click="keyClick('backspace')">
        <span v-text="'⌫'" />
      </KeyCap>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import type { PropType } from 'vue'

import type { KeyCap as KeyCapType } from "@/types/KeyCap"

import KeyCap from "@/components/KeyCap.vue"

export default defineComponent({
  name: 'RoKeyboard',
  components: {
    KeyCap
  },
  props: {
    characters: {
      type: Array as PropType<KeyCapType[]>,
      required: true
    }
  },
  data () {
    return {
    }
  },
  methods: {
    keyClick (keyCap: string) {
      if (keyCap === 'enter') {
        this.$emit('enter')
      } else if (keyCap === 'backspace') {
        this.$emit('backspace')
      } else {
        this.$emit('key', keyCap)
      }
    }
  }
})
</script>

<style scoped>
.keyboard {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  /* height: 100%; */
}
.keyboard-row {
  display: flex;
  flex-direction: row;
  justify-content: center;
  width: 100%;
}
.keyboard-row:nth-child(3) > button:first-child {
  font-size: 1.5rem;
  max-width: unset;
}
.keyboard-row:nth-child(3) > button:last-child {
  font-size: 2rem;
  max-width: unset;
}
</style>
