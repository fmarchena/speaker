<template>
  <button
    v-motion
    :initial="{ opacity: 0, scale: 0.8 }"
    :enter="{ opacity: 1, scale: 1 }"
    :hovered="{ scale: 1.12 }"
    type="button"
    @click="$slidev.nav.next"
    class="
      absolute bottom-4 right-4
      h-9 w-9
      cursor-pointer
      rounded-full
      border-none
      bg-orange-500
      text-lg
      text-white
      shadow-md
      transition-all
      duration-200
      hover:bg-orange-600
    "
    title="Siguiente diapositiva"
    aria-label="Siguiente diapositiva"
  >
    →
  </button>
</template>
