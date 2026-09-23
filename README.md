# T3SL4

Distro Linux propia para ciberseguridad, basada en **Debian 13 (Trixie)**.
Se maneja desde la consola, con una capa gráfica mínima (bspwm) para abrir
herramientas como el navegador o Wireshark, sin escritorio tradicional.

> T3SL4 = TESLA escrito con números.

## Qué es y qué no es

| Es | No es |
|---|---|
| Un proyecto para **aprender** Linux, Bash, Git y Rust construyendo algo real | Un competidor de Kali o Parrot |
| Debian mínimo + configuración, herramientas y personalidad propias | Un kernel o un sistema operativo escrito desde cero |
| Terminal primero; gráficos solo cuando una herramienta los necesita | Un escritorio con menús, paneles de sistema e iconos de escritorio |

## Reglas del proyecto

1. **Regla madre:** la IA propone, Santiago decide, el repo documenta.
   Si algo importante solo está en un chat, todavía no existe.
2. **Regla de aprendizaje:** el código lo escriben las personas del equipo.
   La IA explica, propone, revisa y documenta, pero no entrega código terminado
   para copiar y pegar (ver [ADR-007](docs/10-decisions-adr.md)).
3. **Bloques pequeños:** cada cambio se prueba en las dos arquitecturas
   (arm64 y amd64) antes de darlo por terminado.
4. **Snapshot antes de probar:** ningún script se ejecuta en una VM sin snapshot previo.

## Estado actual

**Fase 0: cimientos documentales.** Todavía no hay código.
El estado vivo está en [`tracker.md`](tracker.md).

```txt
Fase 0  Cimientos documentales        ◀ aquí
Fase 1  Laboratorio (VMs + onboarding)
Fase 2  install.sh v0.1 (Debian mínimo → T3SL4)
Fase 3  Paquetes .deb propios
Fase 4  ISO con live-build
Fase 5  Herramientas propias en Rust
```

## Decisiones base

| Tema | Decisión |
|---|---|
| Base | Debian 13 Trixie |
| Arquitecturas | arm64 (Mac Apple Silicon) + amd64 (PCs Intel) |
| Hipervisor | VMware en todos lados (Fusion en Mac, Workstation en Windows); UTM como secundario |
| Interfaz | TTY → `startx` → bspwm + sxhkd + polybar + picom + kitty + rofi |
| Lenguajes | Bash para el sistema, Rust para herramientas propias |

Detalle y motivos en [`docs/10-decisions-adr.md`](docs/10-decisions-adr.md).

## Documentación

| Documento | Para qué |
|---|---|
| [`docs/00-vision.md`](docs/00-vision.md) | Por qué existe T3SL4 |
| [`docs/01-prd.md`](docs/01-prd.md) | Qué se construye, alcance por fase y riesgos |
| [`docs/02-trd.md`](docs/02-trd.md) | Cómo se construye técnicamente |
| [`docs/03-ui-ux.md`](docs/03-ui-ux.md) | Cómo se ve y se usa |
| [`docs/05-test-plan.md`](docs/05-test-plan.md) | Cómo se prueba |
| [`docs/10-decisions-adr.md`](docs/10-decisions-adr.md) | Registro de decisiones |
| [`docs/onboarding.md`](docs/onboarding.md) | Ruta de aprendizaje para quien empieza desde cero |
| [`docs/handoffs/`](docs/handoffs/) | Cierres de sesión |
| [`tracker.md`](tracker.md) | Estado actual y próximo paso |
| [`CHANGELOG.md`](CHANGELOG.md) | Historial de cambios |

## Cómo empezar

- **Si nunca has usado Linux en serio:** empieza por [`docs/onboarding.md`](docs/onboarding.md).
- **Si ya sabes:** lee la visión, el PRD y el tracker, en ese orden.

## Licencia

Pendiente de decidir (ver ADR-008).
