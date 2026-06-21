---
layout: center
---

<div class="eyebrow">Punto 2</div>

# Arquitectura básica

<p class="lead-copy">
  Cloudflared Tunnel conecta un servicio interno con Cloudflare mediante una conexión saliente iniciada desde la red on-premise.
</p>

<div class="callout callout--highlight">
  La arquitectura cambia el punto de exposición: el origen permanece interno y Cloudflare se vuelve la cara pública del acceso.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="caption">Ruta completa</div>

# Flujo general

<pre class="diagram-card"><code>Usuario
   ↓
Cloudflare DNS
   ↓
Cloudflare Access / WAF
   ↓
Cloudflare Tunnel
   ↓
cloudflared
   ↓
Aplicación on-premise</code></pre>

<div class="controls-grid">
  <span class="control-pill">DNS resuelve</span>
  <span class="control-pill">Access decide</span>
  <span class="control-pill">Tunnel transporta</span>
  <span class="control-pill">cloudflared entrega</span>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols
---

<div class="content-stack">
  <div class="caption">Desde Internet</div>

  # Lado público

  <div class="list-card">
    <AnimatedList
      :items="[
        'El usuario accede al dominio publicado.',
        'Cloudflare recibe primero la solicitud.',
        'Se aplican controles de seguridad y políticas.',
        'La petición autorizada viaja hacia el túnel.',
      ]"
    />
  </div>
</div>

::right::

<div class="content-stack">
  <div class="caption">Dentro de la red</div>

  # Lado interno

  <div class="list-card">
    <AnimatedList
      :items="[
        '`cloudflared` mantiene la conexión saliente activa.',
        'Recibe la solicitud desde Cloudflare.',
        'La dirige al servicio interno correcto.',
        'El origen no queda expuesto directamente.',
      ]"
    />
  </div>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="caption">Inventario mínimo</div>

# Componentes principales

<pre class="diagram-card"><code>Dominio público
Cloudflare DNS
Cloudflare Access
Cloudflare Tunnel
Agente cloudflared
Servicio interno</code></pre>

<div class="callout">
  No todos cumplen el mismo rol: unos publican la ruta, otros transportan el tráfico y otros controlan quién puede entrar.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols
---

<div class="content-stack">
  <div class="caption">Resolución pública</div>

  # Cloudflare DNS

  <p class="lead-copy">
    Relaciona el dominio público con el punto que Cloudflare usa para llevar la solicitud al túnel.
  </p>

  <pre class="diagram-card"><code>app.midominio.com
grafana.midominio.com
api.midominio.com</code></pre>
</div>

::right::

<div class="content-stack">
  <div class="caption">Ruta al origen</div>

  # Cloudflare Tunnel

  <p class="lead-copy">
    Define qué hostname debe terminar en qué servicio interno.
  </p>

  <pre class="diagram-card"><code>app.midominio.com
        ↓
http://192.168.1.50:8080</code></pre>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols
---

<div class="content-stack">
  <div class="caption">Agente on-premise</div>

  # cloudflared

  <div class="list-card">
    <AnimatedList
      :items="[
        'Mantiene la conexión con Cloudflare.',
        'Recibe solicitudes del túnel.',
        'Envía tráfico al servicio interno.',
        'Reporta estado y errores operativos.',
      ]"
    />
  </div>
</div>

::right::

<div class="content-stack">
  <div class="caption">Destino final</div>

  # Servicio interno

  <pre class="diagram-card"><code>Aplicación web
API
Grafana
Metabase
SSH
RDP
Servidor interno</code></pre>

  <div class="callout">
    El túnel no está limitado a una sola app web: puede servir distintos tipos de servicios según el caso de uso.
  </div>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="eyebrow">Idea clave</div>

# La conexión se inicia desde adentro

<pre class="diagram-card"><code>On-premise
   ↓ conexión saliente
Cloudflare</code></pre>

<div class="callout callout--highlight">
  No es Cloudflare quien abre una conexión entrante directa hacia la red interna. El origen es quien sale primero.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="caption">Caso simple</div>

# Ejemplo práctico

<pre class="diagram-card"><code>Usuario
   ↓
https://grafana.midominio.com
   ↓
Cloudflare Access
   ↓
Cloudflare Tunnel
   ↓
cloudflared
   ↓
http://10.10.10.30:3000</code></pre>

<div class="callout">
  Para el usuario hay un dominio público y controlado. Para la red interna sigue existiendo un servicio privado sin exponer su IP directamente.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
---

<div class="caption">Control de acceso</div>

# Sin Access vs Con Access

::left::

<div class="comparison-card">
  <h2>Sin Access</h2>
  <p class="lead-copy">
    El túnel publica el servicio, pero el control de identidad no existe por sí solo.
  </p>
  <div class="list-card">
    <AnimatedList
      :items="[
        'El servicio queda accesible por hostname.',
        'Cualquiera podría intentar entrar.',
        'La protección depende de controles externos o del propio servicio.',
      ]"
    />
  </div>
</div>

::right::

<div class="comparison-card comparison-card--after">
  <h2>Con Access</h2>
  <div class="list-card">
    <AnimatedList
      :items="[
        'Se valida identidad antes de llegar al origen.',
        'Se puede exigir MFA.',
        'Se filtra por grupos, correos o políticas.',
        'Solo entra quien está autorizado.',
      ]"
    />
  </div>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="eyebrow">Distinción clave</div>

# Idea importante

<div class="architecture-emphasis-stack">
  <div class="architecture-emphasis-card">
    <span class="architecture-emphasis-label">Transporte</span>
    <strong>Cloudflared Tunnel transporta el tráfico.</strong>
  </div>

  <div class="architecture-emphasis-card architecture-emphasis-card--accent">
    <span class="architecture-emphasis-label">Control</span>
    <strong>Cloudflare Access controla quién puede entrar.</strong>
  </div>

  <p class="architecture-emphasis-note">No son lo mismo.</p>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="eyebrow">Cierre</div>

# Cierre del punto 2

<pre class="diagram-card"><code>Tunnel  → conecta el servicio
Access  → controla el acceso</code></pre>

<div class="callout callout--highlight">
  El valor real aparece cuando ambos trabajan juntos: Tunnel para publicar sin abrir puertos y Access para decidir quién entra.
</div>

<PreviousSlideButton />
<NextSlideButton />
