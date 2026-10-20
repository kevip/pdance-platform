# 0013 — Hosting en OVH VPS-1 con Docker Compose

- **Estado:** Aceptado
- **Fecha:** 2026-10-19
- **Decisores:** Kevin Castañeda

## Contexto

Hoy todo corre en un Hostinger KVM que cuesta $24/mes: SPA, backoffice, API, MySQL, backups por cron y nginx. El objetivo es bajar costos. El tráfico es bajo, así que la latencia desde Lima no es crítica. El stack nuevo tiene 3–4 servicios pequeños (admin, API, MySQL y Valkey opcional), con un consumo estimado de ~0.9–1.3 GB de RAM.

## Opciones consideradas

Se evaluaron 11 opciones (VPS, PaaS y capas gratuitas) por costo, operación, rendimiento medido, reputación, backups, previsibilidad y latencia. Ver el benchmark de hosting en Confluence. Las principales:

| Opción | USD/mes | Puntaje |
| --- | --- | --- |
| OVH VPS-1 (2 vCPU, 4 GB) | ~5 | 3.85 |
| Sevalla (PaaS) | 15–20 | 3.50 |
| Netcup VPS 500 | ~7–9 | 3.25 |
| Hostinger actual | 24 | 2.65 |

## Decisión

Elegimos **un solo OVH VPS-1 con Docker Compose** para admin, API, MySQL y Valkey (cuando haga falta), con **Cloudflare** delante para DNS, SSL y caché. La web pública estática puede ir en Cloudflare Pages.

En VPSBenchmarks rinde como el Hostinger KVM 2 actual (62 contra 60) a cerca de un cuarto del precio. Un PaaS cobra por servicio y saldría más caro con 3–4 servicios.

## Consecuencias

**Positivas**

- ~$190–230 de ahorro al año.
- Agregar servicios no cambia el costo mientras alcance la RAM.

**Negativas / costos**

- La operación es nuestra: actualizaciones del SO, firewall, backups y monitoreo.
- El servidor es un único punto de falla.

**Pendiente para implementarla**

- [ ] Confirmar precio (mensual o anual) y región en el configurador de OVH.
- [ ] `docker-compose.yml` y Dockerfiles con `output: "standalone"`.
- [ ] Backups diarios de MySQL fuera del VPS, con una restauración probada.
- [ ] `unattended-upgrades`, firewall (solo 80/443/SSH) y monitor de uptime.

## Más información

- [VPSBenchmarks — OVHcloud](https://www.vpsbenchmarks.com/hosters/ovhcloud)
- [OVHcloud VPS](https://www.ovhcloud.com/en/vps/)
