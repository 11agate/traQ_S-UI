<template>
  <div :class="$style.container">
    <icon
      :class="$style.icon"
      v-if="iconName"
      :name="iconName"
      :mdi="iconMdi"
    />
    <h1 :class="$style.title">{{ title }}</h1>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'
import Icon from '@/components/UI/Icon.vue'

export default defineComponent({
  name: 'MainViewHeaderTitle',
  components: { Icon },
  props: {
    iconMdi: {
      type: Boolean,
      defualt: false
    },
    iconName: String,
    title: { type: String, required: true }
  },
  setup() {
    return {}
  }
})
</script>

<style lang="scss" module>
.container {
  display: flex;
  align-items: center;
}

.icon {
  margin-right: 8px;
}

.title {
  @include size-h1;
}
</style>
