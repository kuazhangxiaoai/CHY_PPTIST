<template>
    <div class="base-element-video screen-element-video"
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
        <div class="element-content">
          <div></div>
        </div>
      </div>
    </div>
  </template>
  
<script lang="ts" setup>
import { computed, onBeforeMount, inject, ref } from 'vue'
import { storeToRefs } from 'pinia'
import { useSlidesStore } from '@/store'
import type { PPTMap2dElement } from '@/types/slides'
import { injectKeySlideId, injectKeySlideScale } from '@/types/injectKey'
  

  
defineProps<{
    elementInfo: PPTMap2dElement
}>()
  
const { currentSlide } = storeToRefs(useSlidesStore())
  
const scale = inject(injectKeySlideScale) || ref(1)
const slideId = inject(injectKeySlideId) || ref('')
  
const inCurrentSlide = computed(() => currentSlide.value.id === slideId.value)
  
onBeforeMount(() => {
  console.log('enter screen')
})
</script>
  
<style lang="scss" scoped>
  .screen-element-video {
    position: absolute;
  }
  .rotate-wrapper {
    width: 100%;
    height: 100%;
  }
  .element-content {
    width: 100%;
    height: 100%;
  }
</style>
  