---
layout: two-cols-header
layoutClass: architecture-slide architecture-slide--definition
---

<div class="eyebrow">Primero, los nombres</div>

# Cloudflare y `cloudflared` no son lo mismo

::left::

<div class="content-stack">
  <div class="caption">La empresa y la plataforma</div>

  <div class="cloudflared-definition cloudflare-company-definition">
    <span class="cloudflared-definition__name">Cloudflare</span>
    <p>
      Es la empresa que opera una red global y ofrece servicios como DNS, seguridad, rendimiento y Zero Trust.
    </p>
  </div>

  <div class="controls-grid">
    <span class="control-pill">Red global</span>
    <span class="control-pill">DNS y seguridad</span>
    <span class="control-pill">Access y Tunnel</span>
  </div>
</div>

::right::

<div class="content-stack">
  <div class="caption">El programa dentro de tu red</div>

  <div class="cloudflared-definition">
    <span class="cloudflared-definition__name">cloudflared</span>
    <p>
      Es el proceso ligero que instalas en tu infraestructura. Crea y mantiene las conexiones de Cloudflare Tunnel y entrega solicitudes a servicios locales.
    </p>
  </div>

  <div class="controls-grid">
    <span class="control-pill">Conector local</span>
    <span class="control-pill">Conexión saliente</span>
    <span class="control-pill">Proxy al origen</span>
  </div>
</div>

<div class="callout architecture-definition-note">
  En corto: Cloudflare está afuera y presta la plataforma; <code>cloudflared</code> corre adentro y conecta tu servicio con ella.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
layoutClass: architecture-slide architecture-slide--visual
---

<div class="caption">Mapa general</div>

# Arquitectura básica

::left::

<div class="content-stack">
  <div class="caption">Qué cambia</div>

  <p class="lead-copy">
    El usuario entra por Cloudflare mientras <code>cloudflared</code> conecta el origen desde la red privada. Ambos recorridos se encuentran en el túnel.
  </p>

  <div class="callout callout--highlight architecture-highlight">
    El origen permanece interno. Cloudflare se convierte en la cara pública del acceso y del control.
  </div>

  <div class="controls-grid">
    <span class="control-pill">Origen privado</span>
    <span class="control-pill">Salida controlada</span>
    <span class="control-pill">Identidad delante</span>
  </div>
</div>

::right::

<DiagramZoom label="Ampliar arquitectura básica">
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
</DiagramZoom>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
layoutClass: architecture-slide architecture-slide--components
---

<div class="caption">Lectura guiada</div>

# La arquitectura, componente por componente

<DiagramZoom label="Ampliar componentes de la arquitectura">
  <div class="architecture-component-map">
    <div class="architecture-component-node">
      <span class="architecture-component-node__step">1</span>
      <strong>Usuario</strong>
      <small>Inicia la solicitud al dominio público.</small>
    </div>
    <span class="architecture-component-arrow" aria-hidden="true">→</span>
    <div class="architecture-component-node">
      <span class="architecture-component-node__step">2</span>
      <strong>Cloudflare DNS</strong>
      <small>Resuelve el hostname hacia Cloudflare.</small>
    </div>
    <span class="architecture-component-arrow" aria-hidden="true">→</span>
    <div class="architecture-component-node architecture-component-node--security">
      <span class="architecture-component-node__step">3</span>
      <strong>Access / WAF</strong>
      <small>Valida identidad, reglas y seguridad.</small>
    </div>
    <span class="architecture-component-arrow architecture-component-arrow--down" aria-hidden="true">↓</span>
    <div class="architecture-component-node architecture-component-node--origin">
      <span class="architecture-component-node__step">6</span>
      <strong>Servicio interno</strong>
      <small>Procesa la solicitud sin exponerse.</small>
    </div>
    <span class="architecture-component-arrow architecture-component-arrow--reverse architecture-component-arrow--reverse-right" aria-hidden="true">←</span>
    <div class="architecture-component-node architecture-component-node--agent">
      <span class="architecture-component-node__step">5</span>
      <strong>cloudflared</strong>
      <small>Recibe del túnel y entrega localmente.</small>
    </div>
    <span class="architecture-component-arrow architecture-component-arrow--reverse architecture-component-arrow--reverse-left" aria-hidden="true">←</span>
    <div class="architecture-component-node">
      <span class="architecture-component-node__step">4</span>
      <strong>Cloudflare Tunnel</strong>
      <small>Transporta el tráfico autorizado.</small>
    </div>
  </div>
</DiagramZoom>

<div class="architecture-component-summary">
  DNS dirige, Access y WAF validan, Tunnel transporta, <code>cloudflared</code> entrega y el servicio responde.
</div>

<ArchitectureStepGuide />

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
layoutClass: architecture-slide architecture-slide--visual
---

<div class="caption">Relación hostname → origen</div>

# Cómo enruta el túnel

::left::

<div class="content-stack">
  <div class="callout">
    Para publicar aplicaciones no necesitas exponer toda la red. Cada hostname se asocia con un servicio concreto.
  </div>

  <div class="list-card">
    <AnimatedList
      :items="[
        'DNS lleva el hostname público a Cloudflare.',
        'La configuración del túnel relaciona ese hostname con un origen.',
        'cloudflared entrega la solicitud al servicio configurado.',
      ]"
    />
  </div>

  <div class="controls-grid">
    <span class="control-pill">Web y API</span>
    <span class="control-pill">SSH y RDP</span>
    <span class="control-pill">Herramientas internas</span>
  </div>
</div>

::right::

<DiagramZoom label="Ampliar diagrama de enrutamiento">
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
</DiagramZoom>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
layoutClass: architecture-slide architecture-slide--visual
---

<div class="caption">Caso realista</div>

# Ejemplo práctico: Grafana interno

::left::

<div class="content-stack">
  <div class="caption">Mapeo concreto</div>

  <div class="architecture-route-example">
    <div>
      <span>Hostname público</span>
      <strong>grafana.midominio.com</strong>
    </div>
    <span class="architecture-route-example__arrow" aria-hidden="true">→</span>
    <div>
      <span>Origen privado</span>
      <strong>10.10.10.30:3000</strong>
    </div>
  </div>

  <div class="callout">
    Access valida al usuario antes de que Tunnel transporte la solicitud. Grafana recibe tráfico sin publicar su IP privada.
  </div>
</div>

::right::

<DiagramZoom label="Ampliar arquitectura de acceso a Grafana">
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
</DiagramZoom>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
layoutClass: architecture-slide
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
layoutClass: architecture-slide architecture-slide--center
---

<div class="eyebrow">Cierre</div>

# Cierre del punto 2

<DiagramZoom label="Ampliar relación entre Tunnel y Access">
<pre class="diagram-card"><code>Tunnel  → conecta el servicio
Access  → controla el acceso</code></pre>
</DiagramZoom>

<div class="callout callout--highlight architecture-highlight">
  El valor aparece cuando ambos trabajan juntos: Tunnel para publicar sin exponer el origen y Access para decidir quién entra.
</div>

<PreviousSlideButton />
<NextSlideButton />
