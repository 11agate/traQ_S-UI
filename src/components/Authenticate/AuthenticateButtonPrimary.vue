<template>
  <button :class="$style.container" :disabled="disabled" @click="emit('click')">
    {{ label }}
  </button>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  name: 'AuthenticateButtonPrimary',
  props: {
    label: {
      type: String,
      default: ''
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  setup(props, { emit }) {
    return { emit }
  }
})
</script>

<style lang="scss" module>
.container {
  @include background-accent-primary;
  padding: 12px 64px;
  border: {
    style: solid;
    width: 2px;
    radius: 4px;
    color: $theme-accent-primary;
  }
  color: white;
  font-weight: bold;
  cursor: pointer;
  &[disabled] {
    cursor: not-allowed;
  }
}
</style>
