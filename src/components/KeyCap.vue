<template>
<button type="button" :class="letter?.status">
  <span v-if="letter" v-text="letter.key.toUpperCase()" class="key-text" />
  <slot v-else class="key-text" />
</button>
</template>

<script lang="ts">
import { defineComponent } from "vue"
import type { PropType } from 'vue'

import type { KeyCap as KeyCapType } from "@/types/KeyCap"


export default defineComponent({
  name: 'KeyCap',
  props: {
    letter: {
      type: Object as PropType<KeyCapType>
    }
  },
})
</script>

<style scoped>
button {
  display: flex;
  flex-grow: 1;
  justify-content: center;
  align-items: center;
  border: 1px solid rgb(46, 46, 46);
  background-color: #828385;
  border-radius: 4px;
  padding-left: 2px;
  padding-right: 2px;
  margin: 3px;
  height: 3.5rem;
  /* min-width: 25px; */
  max-width: calc(100% / 12 - 6px);
  color: #ddd;
  cursor: pointer;
  transition : background 0.5s;
}
button:active {
  background-color: rgba(40, 40, 40, 0.5);
  transition: background 0s;
}
.key-text {
  font-size: 20px;
  font-weight: bold;
}
.key-text > span {
  font-weight: bold;
}
.miss {
  background-color: #2c2c2e;
}
.hit {
  background-color: #447d3d;
}
.present {
  background-color: #a5902d;
}
</style>