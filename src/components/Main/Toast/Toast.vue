<template>
  <div :class="$style.container" :data-type="toast.type" @click="onClick">
    <icon :class="$style.icon" :name="iconName" mdi :size="32" />
    <div :class="$style.text">{{ toast.text }}</div>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  PropType,
  computed,
  onMounted,
  onUnmounted
} from '@vue/composition-api'
import { Toast } from '@/store/ui/toast'
import Icon from '@/components/UI/Icon.vue'
import store from '@/store'

const iconNameMap: Record<Toast['type'], string> = {
  success: 'info',
  error: 'alert',
  info: 'info'
}

const useAutoHide = (props: { toast: Toast }) => {
  let timer: number | undefined

  const remove = () => {
    store.commit.ui.toast.deleteToast(props.toast.id)
  }

  onMounted(() => {
    timer = window.setTimeout(() => {
      remove()
    }, props.toast.timeout)
  })
  onUnmounted(() => {
    window.clearTimeout(timer)
  })

  return { remove }
}

export default defineComponent({
  name: 'Toast',
  components: {
    Icon
  },
  props: {
    toast: {
      type: Object as PropType<Toast>,
      required: true
    }
  },
  setup(props) {
    const { remove } = useAutoHide(props)

    const onClick = () => {
      if (props.toast.onClick) {
        props.toast.onClick()
      } else {
        remove()
      }
    }

    const iconName = computed(() => iconNameMap[props.toast.type])
    return { onClick, iconName }
  }
})
</script>

<style lang="scss" module>
.container {
  @include drop-shadow-default;
  display: flex;
  align-items: center;
  width: 320px;
  max-width: calc(100vw - 40px);
  margin: 12px 0;
  padding: 8px;
  border-radius: 4px;
  pointer-events: auto;
  color: $theme-background-primary;
  &[data-type='success'] {
    @include background-accent-primary;
  }
  &[data-type='error'] {
    background: $theme-accent-error;
  }
  &[data-type='info'] {
    background: $theme-ui-secondary;
  }
}
.icon {
  @include color-common-text-white-primary;
  margin: 4px;
  flex-shrink: 0;
  vertical-align: middle;
}
.text {
  margin: 4px;
  word-break: break-all;
}
</style>
