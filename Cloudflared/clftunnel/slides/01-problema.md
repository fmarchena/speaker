---
layout: center
---

<div class="eyebrow">Punto 1</div>

# El problema inicial

<p class="lead-copy">
  Publicar servicios internos abriendo puertos puede funcionar, pero también amplía la superficie visible hacia Internet.
</p>

<div class="callout">
  Cada regla entrante crea una ruta directa hacia sistemas que antes estaban contenidos dentro de la red interna.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols
---

<div class="content-stack">
  <div class="caption">Ruta clásica</div>

  # Modelo tradicional

  <pre class="diagram-card"><code>Internet
   ↓
IP pública
   ↓
Firewall / NAT
   ↓
Puerto abierto
   ↓
Servidor interno</code></pre>
</div>

::right::

<div class="content-stack">
  <div class="caption">Qué se expone</div>

  # Riesgo principal

  <div class="list-card">
    <AnimatedList
      :items="[
        'El origen queda visible para cualquiera que lo escanee.',
        'Los bots prueban puertos y rutas de forma constante.',
        'Una regla mal hecha puede publicar servicios sensibles.',
        'Es fácil dejar accesos temporales abiertos más tiempo del debido.',
      ]"
    />
  </div>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="eyebrow">Cambio de enfoque</div>

# El problema no es solo abrir un puerto

<p class="lead-copy">
  El riesgo real es crear una ruta directa desde Internet hacia infraestructura que fue pensada para operar de forma interna.
</p>

<div class="callout callout--highlight">
  La pregunta correcta deja de ser “qué puerto publico” y pasa a ser “qué superficie expongo”.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols
---

<div class="caption">Lo que suele terminar expuesto</div>

# Servicios que suelen exponerse

<pre class="diagram-card"><code>443  → aplicación web
8080 → panel interno
22   → SSH
3389 → RDP
3000 → Grafana
8081 → Keycloak</code></pre>

::right::

<div class="caption">Errores frecuentes</div>

# Errores frecuentes

<div class="list-card">
  <AnimatedList
    :items="[
      'Puertos temporales que nunca se cierran.',
      'Paneles internos publicados sin MFA.',
      'Servicios de prueba expuestos como si fueran producción.',
      'Reglas NAT difíciles de auditar con el tiempo.',
      'Aplicaciones internas accesibles desde cualquier lugar.',
    ]"
  />
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="caption">Lectura visual del problema</div>

# Señales de mala exposición
 

<div class="meme-grid">
  <div class="meme-card meme-card--compact">
    <img
      src="../public/images/this-is-fine.jpg"
      alt="Meme This Is Fine"
      class="meme-image"
    />
    <div class="meme-caption">
      <p class="meme-title">Panel interno sin MFA</p>
      <p class="meme-note">“Todo bien” aunque el acceso ya quedó demasiado abierto para algo sensible.</p>
    </div>
  </div>

  <div class="meme-card meme-card--compact">
    <img
      src="../public/images/same-picture.jpg"
      alt="Meme They're The Same Picture"
      class="meme-image"
    />
    <div class="meme-caption">
      <p class="meme-title">Staging tratado como producción</p>
      <p class="meme-note">Cuando prueba y producción cambian de nombre, pero en exposición terminan siendo lo mismo.</p>
    </div>
  </div>

  <div class="meme-card meme-card--compact">
    <img
      src="../public/images/charlie-conspiracy.jpg"
      alt="Meme Charlie Conspiracy"
      class="meme-image"
    />
    <div class="meme-caption">
      <p class="meme-title">Reglas NAT imposibles de auditar</p>
      <p class="meme-note">El clásico momento en que nadie entiende ya qué regla abre qué servicio y por qué sigue viva.</p>
    </div>
  </div>

  <div class="meme-card meme-card--compact">
    <img
      src="../public/images/oprah-you-get-a.jpg"
      alt="Meme Oprah You Get A"
      class="meme-image"
    />
    <div class="meme-caption">
      <p class="meme-title">Aplicación interna accesible desde cualquier lugar</p>
      <p class="meme-note">“Tú accedes, tú accedes, todos acceden”, justo lo contrario a segmentar.</p>
    </div>
  </div>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="eyebrow">Lo que cambia</div>

# Qué cambia con Cloudflared Tunnel

<p class="lead-copy">
  El servidor interno inicia una conexión saliente hacia Cloudflare, en lugar de esperar conexiones entrantes desde Internet.
</p>

<pre class="diagram-card"><code>Servidor on-premise
        ↓ conexión saliente
Cloudflare
        ↓
Usuario autorizado</code></pre>

<PreviousSlideButton />
<NextSlideButton />

---
layout: two-cols-header
---

<div class="caption">Antes y después</div>

# Comparación rápida

::left::

<div class="comparison-card">
  <h2>Antes</h2>
  <pre class="diagram-card"><code>Internet
   ↓
IP pública
   ↓
Puerto abierto
   ↓
Servidor interno</code></pre>
</div>

::right::

<div class="comparison-card comparison-card--after">
  <h2>Después</h2>
  <pre class="diagram-card"><code>Usuario
   ↓
Cloudflare
   ↓
Tunnel
   ↓
Servicio interno</code></pre>
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="eyebrow">Idea clave</div>

# Menos exposición, más control

<p class="lead-copy">
  Cloudflared Tunnel permite publicar servicios on-premise sin abrir puertos entrantes directamente hacia Internet.
</p>

<div class="callout callout--highlight">
  Publicar no desaparece como necesidad. Lo que cambia es el punto de exposición y el nivel de control.
</div>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="eyebrow">Controles adicionales</div>

# Punto importante

<p class="lead-copy">
  Cloudflared Tunnel no vuelve segura por sí sola a una aplicación vulnerable, pero sí reduce exposición y facilita poner controles encima.
</p>

<AnimatedPills
  :items="[
    'Cloudflare Access',
    'MFA',
    'WAF',
    'Políticas por usuario o grupo',
    'Registro de accesos',
  ]"
/>

<PreviousSlideButton />
<NextSlideButton />

---
layout: center
---

<div class="eyebrow">Cierre</div>

# Publicar con menos exposición

<p class="lead-copy">
  No se trata solo de publicar una aplicación. Se trata de publicarla con menos superficie visible y con más control operativo.
</p>

<div class="callout">
  Ese cambio de enfoque es lo que hace que Cloudflared Tunnel resulte tan valioso en entornos on-premise.
</div>

<PreviousSlideButton />
<NextSlideButton />
