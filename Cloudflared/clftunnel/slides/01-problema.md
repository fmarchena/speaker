---
layout: center
---

# 1. El problema inicial

Publicar servicios internos abriendo puertos puede funcionar, pero aumenta la exposición directa hacia Internet.

---
layout: two-cols
---

# Modelo tradicional

```text
Internet
   ↓
IP pública
   ↓
Firewall / NAT
   ↓
Puerto abierto
   ↓
Servidor interno
```

::right::

# Riesgo principal

- El origen queda visible.
- Cualquier persona puede intentar conectarse.
- Los bots escanean puertos abiertos.
- Una mala regla puede dejar servicios sensibles expuestos.

---
layout: center
---

# El problema no es solo abrir un puerto

El problema es crear una ruta directa desde Internet hacia la infraestructura interna.

---
layout: two-cols
---

# Servicios que suelen exponerse

```text
443  → aplicación web
8080 → panel interno
22   → SSH
3389 → RDP
3000 → Grafana
8081 → Keycloak
```

::right::

# Errores frecuentes

- Puertos temporales que nunca se cierran.
- Paneles internos publicados sin MFA.
- Servicios de prueba expuestos como producción.
- Reglas NAT difíciles de auditar.
- Aplicaciones internas accesibles desde cualquier lugar.

---
layout: center
---

# Qué cambia con Cloudflared Tunnel

El servidor interno inicia una conexión saliente hacia Cloudflare.

```text
Servidor on-premise
        ↓ conexión saliente
Cloudflare
        ↓
Usuario autorizado
```

---
layout: two-cols
---

# Antes

```text
Internet
   ↓
IP pública
   ↓
Puerto abierto
   ↓
Servidor interno
```

::right::

# Después

```text
Usuario
   ↓
Cloudflare
   ↓
Tunnel
   ↓
Servicio interno
```

---
layout: center
---

# Idea clave

Cloudflared Tunnel permite publicar servicios on-premise sin abrir puertos entrantes directamente hacia Internet.

> Menos exposición, más control.

---
layout: center
---

# Punto importante

Cloudflared Tunnel no hace segura una aplicación vulnerable.

Reduce la exposición y permite agregar controles como:

- Cloudflare Access
- MFA
- WAF
- Políticas por usuario o grupo
- Registro de accesos

---
layout: center
---

# Cierre del punto 1

No se trata solo de publicar una aplicación.

Se trata de publicarla con menos exposición y más control.
 