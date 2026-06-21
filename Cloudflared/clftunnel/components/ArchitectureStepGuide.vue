<script setup lang="ts">
import { computed, nextTick, onMounted, onUnmounted, ref } from 'vue'

const steps = [
  {
    name: 'Usuario',
    role: 'Inicia la solicitud',
    detail: 'Abre el hostname público de la aplicación. No necesita conocer la IP privada ni la ubicación real del servidor.',
    output: 'Entrega una solicitud al dominio público.',
  },
  {
    name: 'Cloudflare DNS',
    role: 'Dirige hacia Cloudflare',
    detail: 'Resuelve el hostname publicado y hace que la solicitud llegue primero a la red global de Cloudflare.',
    output: 'Lleva la solicitud al borde de Cloudflare.',
  },
  {
    name: 'Access / WAF',
    role: 'Aplica controles',
    detail: 'Access puede validar identidad y políticas; WAF inspecciona la solicitud y bloquea patrones no permitidos.',
    output: 'Solo deja continuar tráfico autorizado y válido.',
  },
  {
    name: 'Cloudflare Tunnel',
    role: 'Transporta la solicitud',
    detail: 'Usa las conexiones persistentes existentes para llevar el tráfico desde Cloudflare hasta el conector de tu red.',
    output: 'Entrega el tráfico autorizado a cloudflared.',
  },
  {
    name: 'cloudflared',
    role: 'Conecta y entrega',
    detail: 'El proceso local mantiene las conexiones salientes, identifica el destino configurado y actúa como proxy hacia él.',
    output: 'Envía la solicitud al servicio privado correcto.',
  },
  {
    name: 'Servicio interno',
    role: 'Procesa y responde',
    detail: 'La aplicación recibe la solicitud dentro de la red privada y devuelve su respuesta por el mismo recorrido.',
    output: 'Responde sin publicar directamente su IP privada.',
  },
]

const isOpen = ref(false)
const current = ref(0)
const closeButton = ref<HTMLButtonElement | null>(null)
const step = computed(() => steps[current.value])

async function openGuide() {
  current.value = 0
  isOpen.value = true
  await nextTick()
  closeButton.value?.focus()
}

function closeGuide() {
  isOpen.value = false
}

function previous() {
  if (current.value > 0)
    current.value--
}

function next() {
  if (current.value < steps.length - 1)
    current.value++
}

function handleKeydown(event: KeyboardEvent) {
  if (!isOpen.value)
    return

  if (event.key === 'Escape')
    closeGuide()
  else if (event.key === 'ArrowLeft')
    previous()
  else if (event.key === 'ArrowRight')
    next()
}

onMounted(() => window.addEventListener('keydown', handleKeydown))
onUnmounted(() => window.removeEventListener('keydown', handleKeydown))
</script>

<template>
  <button type="button" class="step-guide__open" @click="openGuide">
    <svg viewBox="0 0 24 24" aria-hidden="true">
      <path d="M5 6h14M5 12h14M5 18h14" />
      <circle cx="7" cy="6" r="1.5" />
      <circle cx="12" cy="12" r="1.5" />
      <circle cx="17" cy="18" r="1.5" />
    </svg>
    Ver componente por componente
  </button>

  <Teleport to="body">
    <Transition name="step-guide">
      <div
        v-if="isOpen"
        class="step-guide__backdrop"
        role="dialog"
        aria-modal="true"
        aria-label="Recorrido por los componentes de la arquitectura"
        @click.self="closeGuide"
      >
        <section class="step-guide__panel">
          <button
            ref="closeButton"
            type="button"
            class="step-guide__close"
            aria-label="Cerrar recorrido"
            @click="closeGuide"
          >
            ×
          </button>

          <div class="step-guide__header">
            <span>Recorrido guiado</span>
            <strong>{{ current + 1 }} / {{ steps.length }}</strong>
          </div>

          <nav class="step-guide__steps" aria-label="Componentes">
            <button
              v-for="(item, index) in steps"
              :key="item.name"
              type="button"
              :class="{ 'is-active': index === current, 'is-complete': index < current }"
              :aria-current="index === current ? 'step' : undefined"
              @click="current = index"
            >
              <span>{{ index + 1 }}</span>
              {{ item.name }}
            </button>
          </nav>

          <Transition name="step-content" mode="out-in">
            <article :key="step.name" class="step-guide__content">
              <div class="step-guide__number">{{ current + 1 }}</div>
              <div>
                <p class="step-guide__role">{{ step.role }}</p>
                <h2>{{ step.name }}</h2>
                <p class="step-guide__detail">{{ step.detail }}</p>
                <div class="step-guide__output">
                  <span>Resultado de este paso</span>
                  <strong>{{ step.output }}</strong>
                </div>
              </div>
            </article>
          </Transition>

          <footer class="step-guide__footer">
            <button type="button" :disabled="current === 0" @click="previous">← Anterior</button>
            <span>También puedes usar ← y →</span>
            <button v-if="current < steps.length - 1" type="button" @click="next">Siguiente →</button>
            <button v-else type="button" class="step-guide__finish" @click="closeGuide">Finalizar</button>
          </footer>
        </section>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.step-guide__open {
  display: inline-flex;
  align-items: center;
  gap: 0.38rem;
  margin-top: 0.48rem;
  border: 1px solid rgb(196 85 0 / 30%);
  border-radius: 999px;
  background: #c45500;
  padding: 0.42rem 0.72rem;
  color: white;
  font-size: 0.68rem;
  font-weight: 800;
  box-shadow: 0 10px 24px rgb(122 58 12 / 18%);
  cursor: pointer;
}

.step-guide__open:hover,
.step-guide__open:focus-visible {
  background: #a84600;
  outline: 2px solid rgb(255 122 26 / 40%);
  outline-offset: 2px;
}

.step-guide__open svg {
  width: 1rem;
  height: 1rem;
  fill: none;
  stroke: currentColor;
  stroke-width: 1.7;
  stroke-linecap: round;
}

.step-guide__backdrop {
  position: fixed;
  z-index: 10010;
  inset: 0;
  display: grid;
  place-items: center;
  background: rgb(10 20 34 / 82%);
  padding: 3vh 3vw;
  backdrop-filter: blur(10px);
}

.step-guide__panel {
  position: relative;
  display: grid;
  grid-template-rows: auto auto 1fr auto;
  width: min(94vw, 1120px);
  height: min(90vh, 760px);
  overflow: hidden;
  border: 1px solid rgb(255 255 255 / 28%);
  border-radius: 28px;
  background: linear-gradient(145deg, #fffaf3, #edf3fa);
  padding: clamp(1.2rem, 3vw, 2.2rem);
  color: #16253b;
  box-shadow: 0 30px 90px rgb(0 0 0 / 42%);
}

.step-guide__close {
  position: absolute;
  z-index: 2;
  top: 0.8rem;
  right: 0.8rem;
  display: grid;
  place-items: center;
  width: 2.2rem;
  height: 2.2rem;
  border: 1px solid rgb(196 85 0 / 25%);
  border-radius: 999px;
  background: #fff7ed;
  color: #a84600;
  font-size: 1.35rem;
  line-height: 1;
  cursor: pointer;
}

.step-guide__header {
  display: flex;
  justify-content: space-between;
  padding-right: 2.8rem;
  color: #c45500;
  font-size: 0.78rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.step-guide__steps {
  display: grid;
  grid-template-columns: repeat(6, minmax(0, 1fr));
  gap: 0.4rem;
  margin-top: 1rem;
}

.step-guide__steps button {
  display: grid;
  justify-items: center;
  gap: 0.3rem;
  border: 0;
  background: transparent;
  padding: 0.35rem;
  color: #5b687b;
  font-size: 0.68rem;
  font-weight: 700;
  cursor: pointer;
}

.step-guide__steps button span {
  display: grid;
  place-items: center;
  width: 1.8rem;
  height: 1.8rem;
  border: 2px solid #cbd5e1;
  border-radius: 999px;
  background: white;
}

.step-guide__steps button.is-active {
  color: #a84600;
}

.step-guide__steps button.is-active span,
.step-guide__steps button.is-complete span {
  border-color: #d85f00;
  background: #d85f00;
  color: white;
}

.step-guide__content {
  display: grid;
  grid-template-columns: auto minmax(0, 1fr);
  align-items: center;
  gap: clamp(1.2rem, 4vw, 3rem);
  max-width: 54rem;
  margin: auto;
}

.step-guide__number {
  display: grid;
  place-items: center;
  width: clamp(5.5rem, 12vw, 8rem);
  height: clamp(5.5rem, 12vw, 8rem);
  border-radius: 26px;
  background: linear-gradient(145deg, #ff7a1a, #c45500);
  color: white;
  font-size: clamp(2.8rem, 6vw, 4.8rem);
  font-weight: 800;
  box-shadow: 0 20px 42px rgb(122 58 12 / 24%);
}

.step-guide__role {
  margin: 0;
  color: #c45500;
  font-size: 0.78rem;
  font-weight: 800;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.step-guide__content h2 {
  margin: 0.25rem 0 0;
  color: #16253b;
  font-family: "Iowan Old Style", Georgia, serif;
  font-size: clamp(1.8rem, 4vw, 3.2rem);
}

.step-guide__detail {
  max-width: 42rem;
  margin: 0.75rem 0 0;
  color: #425168;
  font-size: clamp(0.9rem, 1.5vw, 1.15rem);
  line-height: 1.55;
}

.step-guide__output {
  display: grid;
  gap: 0.22rem;
  margin-top: 1rem;
  border-left: 5px solid #ff7a1a;
  border-radius: 0 16px 16px 0;
  background: rgb(255 232 208 / 75%);
  padding: 0.75rem 0.9rem;
}

.step-guide__output span {
  color: #8a3a00;
  font-size: 0.68rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.step-guide__footer {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  align-items: center;
  gap: 1rem;
  border-top: 1px solid rgb(22 37 59 / 10%);
  padding-top: 1rem;
}

.step-guide__footer button {
  width: fit-content;
  border: 1px solid rgb(196 85 0 / 25%);
  border-radius: 999px;
  background: #fff7ed;
  padding: 0.55rem 0.85rem;
  color: #a84600;
  font-weight: 800;
  cursor: pointer;
}

.step-guide__footer button:last-child {
  justify-self: end;
}

.step-guide__footer button:disabled {
  opacity: 0.35;
  cursor: default;
}

.step-guide__footer span {
  color: #718096;
  font-size: 0.68rem;
}

.step-guide__footer .step-guide__finish {
  background: #c45500;
  color: white;
}

.step-guide-enter-active,
.step-guide-leave-active {
  transition: opacity 180ms ease;
}

.step-guide-enter-from,
.step-guide-leave-to {
  opacity: 0;
}

.step-content-enter-active,
.step-content-leave-active {
  transition: opacity 120ms ease, transform 120ms ease;
}

.step-content-enter-from {
  opacity: 0;
  transform: translateX(12px);
}

.step-content-leave-to {
  opacity: 0;
  transform: translateX(-12px);
}
</style>
