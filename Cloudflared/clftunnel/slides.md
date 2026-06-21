---
theme: default
title: Cloudflared Tunnel
info: |
  Cloudflared Tunnel: una forma práctica de exponer servicios on-premise
layout: cover
class: hero-slide
clickAnimation: fade
drawings:
  persist: false
transition: slide-left
mdc: true
defaults:
  class: cloud-slide
  transition: slide-left
---

<div class="hero-stage">
  <div
    v-motion
    class="hero-shell"
    :initial="{ opacity: 0, x: -36 }"
    :enter="{ opacity: 1, x: 0, transition: { duration: 720 } }"
  >
    <div
      v-motion
      class="eyebrow"
      :initial="{ opacity: 0, y: 14 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 420, delay: 80 } }"
    >
      Cloudflare Zero Trust
    </div>
    <p
      v-motion
      class="hero-kicker"
      :initial="{ opacity: 0, y: 16 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 420, delay: 150 } }"
    >
      On-premise sin puertos abiertos
    </p>
    <h1
      v-motion
      :initial="{ opacity: 0, y: 22, scale: 0.98 }"
      :enter="{ opacity: 1, y: 0, scale: 1, transition: { duration: 560, delay: 230 } }"
    >
      Cloudflared <span>Tunnel</span>
    </h1>
    <p
      v-motion
      class="hero-subtitle"
      :initial="{ opacity: 0, y: 18 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 480, delay: 320 } }"
    >
      Exponer servicios internos con acceso saliente, identidad y menos superficie pública.
    </p>
    <div
      v-motion
      class="hero-chip-row"
      :initial="{ opacity: 0, y: 18 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 480, delay: 400 } }"
    >
      <span class="hero-chip hero-chip--a">Menos exposición</span>
      <span class="hero-chip hero-chip--b">Acceso saliente</span>
      <span class="hero-chip hero-chip--c">Zero Trust</span>
    </div>
    <div
      v-motion
      class="hero-summary"
      :initial="{ opacity: 0, y: 20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 520, delay: 500 } }"
    >
      <div class="hero-summary-label">Idea clave</div>
      <p>El origen sale a Cloudflare. Internet no entra directo a tu red.</p>
    </div>
    <p
      v-motion
      class="hero-meta"
      :initial="{ opacity: 0, y: 14, scale: 0.98 }"
      :enter="{ opacity: 1, y: 0, scale: 1, transition: { duration: 420, delay: 610 } }"
    >
      Francisco Marchena
    </p>
  </div>
  <div
    v-motion
    class="hero-visual"
    aria-hidden="true"
    :initial="{ opacity: 0, x: 40, scale: 0.98 }"
    :enter="{ opacity: 1, x: 0, scale: 1, transition: { duration: 780, delay: 120 } }"
  >
    <div class="hero-visual-glow hero-visual-glow-a" />
    <div class="hero-visual-glow hero-visual-glow-b" />
    <div class="hero-panel">
      <div
        v-motion
        class="hero-panel-header"
        :initial="{ opacity: 0, y: 16 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 420, delay: 220 } }"
      >
        <span class="hero-panel-title">Flujo propuesto</span>
        <span class="hero-panel-badge">Sin puertos inbound</span>
      </div>
      <div class="hero-flow">
        <div
          v-motion
          class="hero-node hero-node--internet"
          :initial="{ opacity: 0, y: 18 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 420, delay: 320 } }"
        >
          <div class="hero-node-label">Usuario</div>
          <strong>Internet</strong>
          <span>Acceso</span>
        </div>
        <div
          v-motion
          class="hero-link"
          :initial="{ opacity: 0, scaleY: 0.6 }"
          :enter="{ opacity: 1, scaleY: 1, transition: { duration: 320, delay: 390 } }"
        >
          <span class="hero-link-line" />
          <span class="hero-link-text">identidad</span>
        </div>
        <div
          v-motion
          class="hero-node hero-node--cloud"
          :initial="{ opacity: 0, y: 18 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 420, delay: 450 } }"
        >
          <div class="hero-node-label">Edge</div>
          <strong>Cloudflare</strong>
          <span>Control público</span>
        </div>
        <div
          v-motion
          class="hero-link"
          :initial="{ opacity: 0, scaleY: 0.6 }"
          :enter="{ opacity: 1, scaleY: 1, transition: { duration: 320, delay: 520 } }"
        >
          <span class="hero-link-line" />
          <span class="hero-link-text">tunnel</span>
        </div>
        <div
          v-motion
          class="hero-node hero-node--origin"
          :initial="{ opacity: 0, y: 18 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 420, delay: 590 } }"
        >
          <div class="hero-node-label">Origen</div>
          <strong>Servicio on-premise</strong>
          <span>Sigue privado</span>
        </div>
      </div>
      <div class="hero-stat-row">
        <div
          v-motion
          class="hero-stat"
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 360, delay: 680 } }"
        >
          <span class="hero-stat-value">0</span>
          <span class="hero-stat-label">puertos</span>
        </div>
        <div
          v-motion
          class="hero-stat"
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 360, delay: 740 } }"
        >
          <span class="hero-stat-value">1</span>
          <span class="hero-stat-label">salida</span>
        </div>
        <div
          v-motion
          class="hero-stat"
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 360, delay: 800 } }"
        >
          <span class="hero-stat-value">+</span>
          <span class="hero-stat-label">control</span>
        </div>
      </div>
    </div>
  </div>
</div>

<NextSlideButton />

---
src: ./slides/01-problema.md
---

---
src: ./slides/02-arquitectura.md
---
