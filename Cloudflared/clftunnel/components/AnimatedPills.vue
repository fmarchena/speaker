<script setup lang="ts">
withDefaults(defineProps<{
  items: string[]
  class?: string
  itemClass?: string
  baseDelay?: number
  stepDelay?: number
}>(), {
  class: 'controls-grid',
  itemClass: 'control-pill',
  baseDelay: 60,
  stepDelay: 170,
})
</script>

<template>
  <div :class="class">
    <span
      v-for="(item, index) in items"
      :key="`${item}-${index}`"
      v-motion
      :initial="{ opacity: 0, y: 10 }"
      :enter="{
        opacity: 1,
        y: 0,
        transition: {
          delay: baseDelay + index * stepDelay,
          duration: 820,
        },
      }"
      :class="itemClass"
    >
      {{ item }}
    </span>
  </div>
</template>
