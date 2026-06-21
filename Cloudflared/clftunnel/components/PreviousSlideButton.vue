<script setup lang="ts">
import { ref } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $nav } = useSlideContext()
const isBursting = ref(false)

const particles = [
  { id: 1, x: '-16px', y: '-20px', delay: '0ms', size: '4px' },
  { id: 2, x: '-28px', y: '-8px', delay: '35ms', size: '3px' },
  { id: 3, x: '-24px', y: '14px', delay: '55ms', size: '4px' },
  { id: 4, x: '-8px', y: '24px', delay: '20ms', size: '3px' },
  { id: 5, x: '12px', y: '18px', delay: '45ms', size: '4px' },
  { id: 6, x: '18px', y: '-10px', delay: '10ms', size: '3px' },
]

function handleClick() {
  if (isBursting.value)
    return

  isBursting.value = true

  window.setTimeout(() => {
    $nav.value.prev()
  }, 180)

  window.setTimeout(() => {
    isBursting.value = false
  }, 520)
}
</script>

<template>
  <button
    v-motion
    :initial="{ opacity: 0, scale: 0.9, y: 6 }"
    :enter="{ opacity: 1, scale: 1 }"
    :hovered="{ scale: 1.03, rotate: -1 }"
    type="button"
    :class="{ 'is-bursting': isBursting }"
    class="
      nav-cat group fixed left-5 top-5
      flex h-7 w-7 items-center justify-center
      cursor-pointer
      overflow-visible
      rounded-full
      border-none
      bg-stone-100/75
      text-stone-600
      shadow-sm
      backdrop-blur-sm
      transition-all
      duration-300
      hover:bg-orange-100/85
      hover:text-orange-500
      hover:shadow-md
    "
    title="Diapositiva anterior"
    aria-label="Diapositiva anterior"
    @click="handleClick"
  >
    <span class="particle-layer" aria-hidden="true">
      <span
        v-for="particle in particles"
        :key="particle.id"
        class="particle"
        :style="{
          '--tx': particle.x,
          '--ty': particle.y,
          '--delay': particle.delay,
          '--size': particle.size,
        }"
      />
    </span>

    <svg
      viewBox="0 0 24 24"
      class="h-4 w-4 -scale-x-100 transition-transform duration-300 group-hover:-translate-x-0.5 group-hover:-rotate-1"
      fill="none"
      stroke="currentColor"
      stroke-width="1.6"
      stroke-linecap="round"
      stroke-linejoin="round"
      aria-hidden="true"
    >
      <path d="M8 10 6.2 5.8 3.8 9.3" />
      <path d="M16 10 17.8 5.8 20.2 9.3" />
      <path d="M6.2 10.2c0-2.3 2.6-4.2 5.8-4.2s5.8 1.9 5.8 4.2v3.2c0 2.5-2.3 4.6-5.2 4.6h-1.2c-2.9 0-5.2-2.1-5.2-4.6z" />
      <path d="M10 13.2h.01" />
      <path d="M14 13.2h.01" />
      <path d="M11.2 15.6c.5.4 1.1.4 1.6 0" />
      <path d="M8.6 15.1 6.8 15" />
      <path d="M15.4 15.1 17.2 15" />
    </svg>
  </button>
</template>

<style scoped>
.nav-cat {
  position: fixed;
  z-index: 40;
}

.particle-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.particle {
  position: absolute;
  left: 50%;
  top: 50%;
  width: var(--size);
  height: var(--size);
  border-radius: 9999px;
  background: rgb(251 146 60 / 0.88);
  opacity: 0;
  transform: translate(-50%, -50%) scale(0.3);
  box-shadow: 0 0 0 1px rgb(255 237 213 / 0.7);
}

.is-bursting .particle {
  animation: burst 420ms cubic-bezier(0.22, 1, 0.36, 1) forwards;
  animation-delay: var(--delay);
}

@keyframes burst {
  0% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.3);
  }

  18% {
    opacity: 1;
  }

  100% {
    opacity: 0;
    transform: translate(calc(-50% + var(--tx)), calc(-50% + var(--ty))) scale(1);
  }
}
</style>
