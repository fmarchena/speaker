<script setup lang="ts">
const props = defineProps<{
  size?: string
}>()

const logoSrc = '/images/PurpleSpace_Academy_transparente.png'
</script>

<template>
  <div
    class="space-cat-logo"
    :style="{ '--logo-size': props.size ?? '6.25rem' }"
    aria-hidden="true"
  >
    <div class="space-cat-stars">
      <span class="space-cat-star space-cat-star-a" />
      <span class="space-cat-star space-cat-star-b" />
      <span class="space-cat-star space-cat-star-c" />
    </div>

    <img
      :src="logoSrc"
      alt="PurpleSpace Academy"
      class="space-cat-image"
    />
  </div>
</template>

<style scoped>
.space-cat-logo {
  position: fixed;
  left: 0.65rem;
  bottom: 0.55rem;
  z-index: 35;
  width: var(--logo-size);
  pointer-events: none;
  opacity: 0.8;
  animation: cat-float 4.6s ease-in-out infinite;
}

.space-cat-stars {
  position: absolute;
  inset: -0.25rem;
}

.space-cat-star {
  position: absolute;
  width: 0.28rem;
  height: 0.28rem;
  border-radius: 9999px;
  background: rgba(255, 208, 140, 0.95);
  box-shadow: 0 0 12px rgba(255, 193, 118, 0.8);
  opacity: 0.55;
}

.space-cat-star-a {
  top: 0.4rem;
  right: 0.55rem;
  animation: star-blink 2.8s ease-in-out infinite;
}

.space-cat-star-b {
  top: 1.35rem;
  right: -0.05rem;
  width: 0.2rem;
  height: 0.2rem;
  animation: star-blink 3.1s ease-in-out infinite 0.7s;
}

.space-cat-star-c {
  left: 0.2rem;
  top: 0.95rem;
  width: 0.18rem;
  height: 0.18rem;
  animation: star-blink 2.5s ease-in-out infinite 1.2s;
}

.space-cat-image {
  display: block;
  width: 50%;
  height: auto;
  filter: drop-shadow(0 12px 24px rgba(32, 38, 73, 0.2));
  animation: cat-tilt 5.2s ease-in-out infinite;
  transform-origin: 50% 72%;
}

@keyframes cat-float {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-0.22rem);
  }
}

@keyframes cat-tilt {
  0%,
  100% {
    transform: rotate(-1.2deg) scale(1);
  }

  50% {
    transform: rotate(1.6deg) scale(1.015);
  }
}

@keyframes star-blink {
  0%,
  100% {
    opacity: 0.35;
    transform: scale(0.9);
  }

  50% {
    opacity: 0.95;
    transform: scale(1.25);
  }
}
</style>
