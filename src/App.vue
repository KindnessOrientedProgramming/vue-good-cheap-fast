<template>
  <toggler :size="3" :checked="checked[0]" @updated="value => updatedHandler(0, value)">
    Good
  </toggler>
  <toggler :size="3" :checked="checked[1]" @updated="value => updatedHandler(1, value)">
    Cheap
  </toggler>
  <toggler :size="3" :checked="checked[2]" @updated="value => updatedHandler(2, value)">
    Fast
  </toggler>
</template>

<script setup>
import { ref } from 'vue'
import Toggler from './components/Toggler.vue'

const indexes = []
const checked = ref([false, false, false])

const updatedHandler = (index, value) => {
  if (value && !indexes.includes(index)) {
    indexes.push(index)
  }

  if (!value && indexes.includes(index)) {
    indexes.splice(indexes.indexOf(index), 1)
  }

  if (indexes.length === 3) {
    indexes.splice(0, 1)
  }

  const newChecked = [false, false, false]

  if (typeof indexes[0] !== 'undefined') {
    newChecked[indexes[0]] = true
  }

  if (typeof indexes[1] !== 'undefined') {
    newChecked[indexes[1]] = true
  }

  checked.value = newChecked
}

</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
}
</style>
