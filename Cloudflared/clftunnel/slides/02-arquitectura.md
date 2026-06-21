---
layout: two-cols-header
---

<div class="eyebrow">Punto 2</div>

# Arquitectura básica

::left::

<div class="content-stack">
  <div class="caption">Qué cambia</div>

  <p class="lead-copy">
    Cloudflared Tunnel conecta un servicio interno con Cloudflare usando una conexión saliente iniciada desde la red on-premise.
  </p>

  <div class="callout callout--highlight">
    El origen permanece interno. Cloudflare se convierte en la cara pública del acceso y del control.
  </div>

  <div class="controls-grid">
    <span class="control-pill">Origen privado</span>
    <span class="control-pill">Salida controlada</span>
    <span class="control-pill">Identidad delante</span>
  </div>
</div>

::right::

<div class="architecture-figure-card">
  <img
    src="../public/images/architecture-overview.svg"
    alt="Diagrama general de usuario, Cloudflare y origen on-premise conectado por cloudflared"
    class="architecture-figure"
  />
  <p class="architecture-figure-caption">
    Vista rápida: el usuario entra por Cloudflare y el origen se conecta hacia afuera con <code>cloudflared</code>.
  </p>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
---

<div class="caption">Recorrido completo</div>

# Flujo de extremo a extremo

::left::

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

::right::

<div class="list-card">
  <AnimatedList
    :items="[
      'El usuario solo ve el dominio público.',
      'Cloudflare recibe primero la solicitud.',
      'Las políticas se aplican antes de tocar el origen.',
      'El tráfico autorizado baja por el túnel hasta el servicio interno.',
    ]"
  />
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols
---

<div class="content-stack">
  <div class="caption">De cara a Internet</div>

  # Lado público

  <div class="list-card">
    <AnimatedList
      :items="[
        'El usuario accede al dominio publicado.',
        'Cloudflare recibe la solicitud primero.',
        'Puede aplicar DNS, WAF, Access y otras políticas.',
        'Solo el tráfico válido continúa al túnel.',
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
        '`cloudflared` mantiene la conexión saliente viva.',
        'Recibe solicitudes desde Cloudflare.',
        'Las dirige al servicio interno correcto.',
        'La IP privada del origen no queda publicada.',
      ]"
    />
  </div>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
---

<div class="caption">Relación hostname → origen</div>

# Cómo enruta el túnel

::left::

<div class="content-stack">
  <div class="callout">
    El túnel no “expone una red completa”. Expone rutas concretas y cada hostname puede terminar en un destino distinto.
  </div>

  <div class="list-card">
    <AnimatedList
      :items="[
        'Un dominio público apunta a una intención de acceso.',
        'El túnel decide a qué servicio interno mandar esa solicitud.',
        'Eso permite publicar varias apps sin abrir varios puertos.',
      ]"
    />
  </div>
</div>

::right::

<div class="architecture-figure-card">
  <img
    src="../public/images/tunnel-routing-example.svg"
    alt="Ejemplo de hostnames públicos mapeados por Cloudflare Tunnel a varios servicios internos"
    class="architecture-figure"
  />
  <p class="architecture-figure-caption">
    Ejemplo: distintos hostnames terminan en distintos orígenes internos.
  </p>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols
---

<div class="content-stack">
  <div class="caption">Agente on-premise</div>

  # Qué hace `cloudflared`

  <div class="list-card">
    <AnimatedList
      :items="[
        'Mantiene la conexión con Cloudflare.',
        'Recibe tráfico del túnel.',
        'Lo entrega al servicio interno configurado.',
        'Reporta estado, errores y salud operativa.',
      ]"
    />
  </div>
</div>

::right::

<div class="content-stack">
  <div class="caption">Qué puede publicar</div>

  # Tipos de servicios

  <pre class="diagram-card"><code>Aplicación web
API
Grafana
Metabase
SSH
RDP
Servidor interno</code></pre>

  <div class="callout">
    El patrón es el mismo: hostname público adelante, servicio privado atrás.
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
  No es Cloudflare quien abre una conexión entrante hacia la red interna. El origen sale primero y mantiene esa relación viva.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
---

<div class="caption">Caso realista</div>

# Ejemplo práctico: Grafana interno

::left::

<div class="content-stack">
  <pre class="diagram-card"><code>Usuario
   ↓
grafana.midominio.com
   ↓
Cloudflare Access
   ↓
Cloudflare Tunnel
   ↓
cloudflared
   ↓
10.10.10.30:3000</code></pre>

  <div class="callout">
    El usuario entra por un dominio controlado. El servidor interno sigue siendo privado y solo recibe tráfico que ya pasó los controles.
  </div>
</div>

::right::

<div class="architecture-figure-card">
  <img
    src="../public/images/access-grafana-example.svg"
    alt="Ejemplo visual de acceso a Grafana interno pasando primero por Cloudflare Access y luego por el túnel"
    class="architecture-figure"
  />
  <p class="architecture-figure-caption">
    Access valida primero; Tunnel entrega después.
  </p>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
---

<div class="caption">Distinción importante</div>

# Tunnel vs Access

::left::

<div class="comparison-card">
  <h2>Tunnel</h2>
  <p class="lead-copy">
    Se encarga de transportar el tráfico entre Cloudflare y el origen interno.
  </p>
  <div class="list-card">
    <AnimatedList
      :items="[
        'Conecta el servicio.',
        'Evita abrir puertos entrantes.',
        'Resuelve cómo llegar al origen interno.',
      ]"
    />
  </div>
</div>

::right::

<div class="comparison-card comparison-card--after">
  <h2>Access</h2>
  <p class="lead-copy">
    Se encarga de decidir quién puede pasar antes de tocar el servicio.
  </p>
  <div class="list-card">
    <AnimatedList
      :items="[
        'Valida identidad.',
        'Puede exigir MFA.',
        'Filtra por grupos, correos o políticas.',
      ]"
    />
  </div>
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
  El valor aparece cuando ambos trabajan juntos: Tunnel para publicar sin exponer el origen y Access para decidir quién entra.
</div>

<PreviousSlideButton />
<NextSlideButton />
