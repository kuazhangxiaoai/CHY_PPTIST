<template>
  <div class="editable-element-video"
    :class="{ 'lock': elementInfo.lock }"
    :style="{
      top: elementInfo.top + 'px',
      left: elementInfo.left + 'px',
      width: elementInfo.width + 'px',
      height: elementInfo.height + 'px',
    }"
  >
    <div
      class="rotate-wrapper"
      :style="{ transform: `rotate(${elementInfo.rotate}deg)` }"
    >
      <div 
        class="element-content" 
        v-contextmenu="contextmenus" 
        @mousedown="$event => handleSelectElement($event, false)"
        @touchstart="$event => handleSelectElement($event, false)"
      >
        <div>hello</div>
        <div 
          :class="['handler-border', item]" 
          v-for="item in ['t', 'b', 'l', 'r']" 
          :key="item"
          @mousedown="$event => handleSelectElement($event)"
          @touchstart="$event => handleSelectElement($event)"
        ></div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { onBeforeMount} from 'vue'
import { storeToRefs } from 'pinia'
import { useMainStore } from '@/store'
import type { PPTMap2dElement } from '@/types/slides'
import type { ContextmenuItem } from '@/components/Contextmenu/types'


const props = defineProps<{
  elementInfo: PPTMap2dElement
  selectElement: (e: MouseEvent | TouchEvent, element: PPTMap2dElement, canMove?: boolean) => void
  contextmenus: () => ContextmenuItem[] | null
}>()

const { canvasScale } = storeToRefs(useMainStore())

const handleSelectElement = (e: MouseEvent | TouchEvent, canMove = true) => {
  if (props.elementInfo.lock) return
  e.stopPropagation()

  props.selectElement(e, props.elementInfo, canMove)
}

onBeforeMount(() => {
  console.log('enter pms')
})

</script>

<style lang="scss" scoped>
.editable-element-video {
  position: absolute;

  &.lock .handler-border {
    cursor: default;
  }
}
.rotate-wrapper {
  width: 100%;
  height: 100%;
}
.element-content {
  width: 100%;
  height: 100%;
  position: relative;
}
.handler-border {
  position: absolute;
  cursor: move;

  &.t {
    width: 100%;
    height: 20px;
    top: 0;
    left: 0;
  }
  &.b {
    width: 100%;
    height: 5px;
    bottom: 0;
    left: 0;
  }
  &.l {
    width: 10px;
    height: 100%;
    left: 0;
    top: 0;
  }
  &.r {
    width: 10px;
    height: 100%;
    right: 0;
    top: 0;
  }
}
</style>
