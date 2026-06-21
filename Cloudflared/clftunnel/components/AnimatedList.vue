<script setup lang="ts">
withDefaults(defineProps<{
  items: string[]
  class?: string
  baseDelay?: number
  stepDelay?: number
}>(), {
  class: 'feature-list',
  baseDelay: 60,
  stepDelay: 70,
})
</script>

<template>
  <ul :class="class">
    <li
      v-for="(item, index) in items"
      :key="`${item}-${index}`"
      v-motion
      :initial="{ opacity: 0, y: 10 }"
      :enter="{
        opacity: 1,
        y: 0,
        transition: {
          delay: baseDelay + index * stepDelay,
          duration: 420,
        },
      }"
    >
      {{ item }}
    </li>
  </ul>
</template>
