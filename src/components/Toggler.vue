<script setup>
import { ref, toRefs, defineProps, defineEmits, computed } from 'vue'

const buttonWidth = ref(50)
const buttonHeight = ref(30)
const toggleDiameter = ref(26)
const toggleWider = ref(34)

const props = defineProps({
  size: {
    type: Number,
    default: 1
  },
  checked: {
    type: Boolean,
    default: false
  }
})

const { size, checked } = toRefs(props)

const emits = defineEmits(['updated'])

const buttonSizeStyles = computed(() => {
  return {
    '--button-width': buttonWidth.value * size.value + 'px',
    '--button-height': buttonHeight.value * size.value + 'px',
    '--toggle-diameter': toggleDiameter.value * size.value + 'px',
    '--toggle-wider': toggleWider.value * size.value + 'px'
  }
})

const updateHandler = event => {
  emits('updated', event.target.checked)
}

</script>

<template>
  <div class="toggler" :style="{ ...buttonSizeStyles }">
    <label>
      <input type="checkbox" :checked="checked" @input="updateHandler">
      <span></span>
    </label>
    <strong>
      <slot />
    </strong>
  </div>
</template>

<style scoped lang="scss">
.toggler {
  --button-width: 500px;
  --button-height: 295px;
  /* 里面圆形直径 */
  --toggle-diameter: 255px;
  /* 按钮与里面圆形之间的距离 */
  --button-toggle-offset: calc((var(--button-height) - var(--toggle-diameter)) / 2);
  /* 里面圆形阴影的大小 */
  --toggle-shadow-offset: 10px;
  /* 里面圆形在长按状态下的宽度 */
  --toggle-wider: 333px;
  --color-grey: #E9E9EA;
  --color-dark-grey: #39393D;
  --color-green: #30D158;

  display: inline-flex;
  flex-direction: row;

  strong {
    line-height: var(--button-height);
    font-size: var(--toggle-diameter);
    margin-left: calc(var(--toggle-diameter) / 4);
  }
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

span {
  display: inline-block;
  width: var(--button-width);
  height: var(--button-height);
  background-color: var(--color-grey);
  border-radius: calc(var(--button-height) / 2);
  position: relative;
  transition: .3s all ease-in-out;
}

span::after {
  content: '';
  display: inline-block;
  width: var(--toggle-diameter);
  height: var(--toggle-diameter);
  background-color: #fff;
  border-radius: calc(var(--toggle-diameter) / 2);
  position: absolute;
  top: var(--button-toggle-offset);
  left: 0;
  transform: translateX(var(--button-toggle-offset));
  box-shadow: var(--toggle-shadow-offset) 0 calc(var(--toggle-shadow-offset) * 4) rgba(0, 0, 0, .10);
  transition: .3s all ease-in-out;
}

input[type="checkbox"]:checked + span {
  background-color: var(--color-green);
}

input[type="checkbox"]:checked + span::after {
  transform: translateX(calc(var(--button-width) - var(--toggle-diameter) - var(--button-toggle-offset)));
  box-shadow: calc(var(--toggle-shadow-offset) * -1) 0 calc(var(--toggle-shadow-offset) * 4) rgba(0, 0, 0, .10);
}

input[type="checkbox"] {
  display: none;
}

input[type="checkbox"]:active + span::after {
  width: var(--toggle-wider);
}

input[type="checkbox"]:checked:active + span::after {
  transform: translateX(calc(var(--button-width) - var(--toggle-wider) - var(--button-toggle-offset)));
}

@media(prefers-color-scheme: dark) {
  body {
    background-color: #1C1C1E;
  }
  span {
    background-color: var(--color-dark-grey);
  }
}
</style>
