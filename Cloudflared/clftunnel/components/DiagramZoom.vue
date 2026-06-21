<script setup lang="ts">
import { nextTick, onMounted, onUnmounted, ref } from 'vue'

withDefaults(defineProps<{
  label?: string
}>(), {
  label: 'Ampliar diagrama',
})

const isOpen = ref(false)
const closeButton = ref<HTMLButtonElement | null>(null)

async function openZoom() {
  isOpen.value = true
  await nextTick()
  closeButton.value?.focus()
}

function closeZoom() {
  isOpen.value = false
}

function handleKeydown(event: KeyboardEvent) {
  if (event.key === 'Escape' && isOpen.value)
    closeZoom()
}

onMounted(() => window.addEventListener('keydown', handleKeydown))
onUnmounted(() => window.removeEventListener('keydown', handleKeydown))
</script>

<template>
  <div class="diagram-zoom">
    <slot />

    <button
      type="button"
      class="diagram-zoom__trigger"
      :title="label"
      :aria-label="label"
      @click.stop="openZoom"
    >
      <svg viewBox="0 0 24 24" aria-hidden="true">
        <circle cx="10.8" cy="10.8" r="5.8" />
        <path d="m15.2 15.2 4 4" />
        <path d="M10.8 8.2v5.2M8.2 10.8h5.2" />
      </svg>
      <span>Enfocar</span>
    </button>

    <Teleport to="body">
      <Transition name="diagram-focus">
        <div
          v-if="isOpen"
          class="diagram-zoom__backdrop"
          role="dialog"
          aria-modal="true"
          :aria-label="label"
          @click.self="closeZoom"
        >
          <div class="diagram-zoom__modal">
            <button
              ref="closeButton"
              type="button"
              class="diagram-zoom__close"
              aria-label="Cerrar diagrama ampliado"
              title="Cerrar"
              @click="closeZoom"
            >
              <svg viewBox="0 0 24 24" aria-hidden="true">
                <path d="m6 6 12 12M18 6 6 18" />
              </svg>
            </button>

            <div class="diagram-zoom__content">
              <slot />
            </div>
          </div>
        </div>
      </Transition>
    </Teleport>
  </div>
</template>

<style scoped>
.diagram-zoom {
  position: relative;
  width: 100%;
}

.diagram-zoom__trigger,
.diagram-zoom__close {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgb(196 85 0 / 28%);
  background: rgb(255 250 243 / 94%);
  color: #a84600;
  box-shadow: 0 8px 20px rgb(27 44 69 / 14%);
  cursor: pointer;
  transition: transform 180ms ease, background 180ms ease, box-shadow 180ms ease;
}

.diagram-zoom__trigger {
  position: absolute;
  z-index: 4;
  top: 0.55rem;
  right: 0.55rem;
  gap: 0.25rem;
  min-height: 1.65rem;
  border-radius: 999px;
  padding: 0.28rem 0.48rem;
  font-size: 0.62rem;
  font-weight: 800;
  letter-spacing: 0.02em;
}

.diagram-zoom__trigger:hover,
.diagram-zoom__trigger:focus-visible,
.diagram-zoom__close:hover,
.diagram-zoom__close:focus-visible {
  background: #fff0df;
  box-shadow: 0 10px 24px rgb(122 58 12 / 20%);
  outline: 2px solid rgb(255 122 26 / 38%);
  outline-offset: 2px;
  transform: translateY(-1px);
}

.diagram-zoom__trigger svg {
  width: 0.85rem;
  height: 0.85rem;
}

.diagram-zoom__trigger svg,
.diagram-zoom__close svg {
  fill: none;
  stroke: currentColor;
  stroke-width: 1.8;
  stroke-linecap: round;
  stroke-linejoin: round;
}

.diagram-zoom__backdrop {
  position: fixed;
  z-index: 10000;
  inset: 0;
  display: grid;
  place-items: center;
  background: rgb(10 20 34 / 78%);
  padding: 3vh 3vw;
  backdrop-filter: blur(10px);
}

.diagram-zoom__modal {
  position: relative;
  display: grid;
  place-items: center;
  width: min(94vw, 1200px);
  height: min(90vh, 820px);
  border: 1px solid rgb(255 255 255 / 28%);
  border-radius: 28px;
  background: linear-gradient(145deg, #fffaf3, #edf3fa);
  padding: clamp(1.25rem, 3vw, 2.5rem);
  box-shadow: 0 30px 90px rgb(0 0 0 / 42%);
}

.diagram-zoom__close {
  position: absolute;
  z-index: 2;
  top: 0.8rem;
  right: 0.8rem;
  width: 2.25rem;
  height: 2.25rem;
  border-radius: 999px;
}

.diagram-zoom__close svg {
  width: 1.15rem;
  height: 1.15rem;
}

.diagram-zoom__content {
  display: grid;
  place-items: center;
  width: 100%;
  height: 100%;
  overflow: auto;
}

.diagram-zoom__content :deep(.architecture-figure-card) {
  width: min(100%, 1050px);
  max-height: 100%;
  padding: 1rem;
}

.diagram-zoom__content :deep(.architecture-figure) {
  width: 100%;
  max-height: 68vh;
  object-fit: contain;
}

.diagram-zoom__content :deep(.architecture-figure-caption) {
  font-size: clamp(0.85rem, 1.2vw, 1.05rem);
}

.diagram-zoom__content :deep(.diagram-card) {
  min-width: min(80vw, 42rem);
  max-width: 100%;
  padding: clamp(1.4rem, 3vw, 2.4rem);
  font-size: clamp(1.15rem, 2.1vw, 1.9rem);
  line-height: 1.5;
}

.diagram-zoom__content :deep(.architecture-component-map) {
  width: min(100%, 1100px);
  padding: clamp(1rem, 2vw, 1.6rem);
  gap: 0.8rem 1rem;
}

.diagram-zoom__content :deep(.architecture-component-node) {
  min-height: 7rem;
  padding: 1rem;
}

.diagram-zoom__content :deep(.architecture-component-node strong) {
  font-size: clamp(1rem, 1.5vw, 1.3rem);
}

.diagram-zoom__content :deep(.architecture-component-node small) {
  font-size: clamp(0.78rem, 1.15vw, 1rem);
}

.diagram-zoom__content :deep(.architecture-component-arrow) {
  font-size: 2rem;
}

.diagram-focus-enter-active,
.diagram-focus-leave-active {
  transition: opacity 180ms ease;
}

.diagram-focus-enter-active .diagram-zoom__modal,
.diagram-focus-leave-active .diagram-zoom__modal {
  transition: transform 180ms ease;
}

.diagram-focus-enter-from,
.diagram-focus-leave-to {
  opacity: 0;
}

.diagram-focus-enter-from .diagram-zoom__modal,
.diagram-focus-leave-to .diagram-zoom__modal {
  transform: scale(0.96);
}
</style>
