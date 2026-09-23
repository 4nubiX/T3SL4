# 10 — Registro de decisiones (ADR)

Estados posibles: Propuesta / Aprobada / Implementada / Revertida / En revisión.

| ID | Decisión | Estado |
|---|---|---|
| ADR-001 | Base Debian 13 Trixie | Aprobada |
| ADR-002 | Soportar arm64 y amd64 desde el inicio | Aprobada |
| ADR-003 | VMware en todas las máquinas; UTM como secundario | Aprobada |
| ADR-004 | TTY + `startx` + bspwm (X11), sin display manager | Aprobada |
| ADR-005 | Bash para el sistema, Rust para herramientas propias | Aprobada |
| ADR-006 | Copiar un subconjunto curado de los dotfiles | Propuesta |
| ADR-007 | La IA no escribe el código del proyecto | Aprobada |
| ADR-008 | Licencia | Propuesta (pendiente) |
| ADR-009 | Flujo Git del equipo | Propuesta (pendiente) |

---

## ADR-001 — Base Debian 13 Trixie

- **Fecha:** 2026-09-23
- **Contexto:** hace falta una base para la distro. El equipo ya usa Parrot y Kali, ambas derivadas de Debian.
- **Opciones:** Debian 12 Bookworm, Debian 13 Trixie, Arch, construir desde cero.
- **Decisión:** Debian 13 Trixie.
- **Motivo:** es la estable actual (publicada en agosto de 2025) y tendrá más años de soporte que la 12, que ya es oldstable. Trae versiones más nuevas de picom, kitty, etc. Es la misma familia que Parrot y Kali, así que el conocimiento del equipo se transfiere, y soporta oficialmente arm64 y amd64.
- **Consecuencias:** hay que seguir las convenciones de Debian (APT, dpkg, live-build). Arch queda descartado a pesar de que el compa tuvo una VM con él.
- **Estado:** Aprobada.

## ADR-002 — Soportar arm64 y amd64 desde el inicio

- **Fecha:** 2026-09-23
- **Contexto:** Santiago trabaja principalmente en una Mac Apple Silicon (arm64); el compa y la PC de Santiago son Intel (amd64).
- **Opciones:** solo amd64 al principio / ambas desde el inicio.
- **Decisión:** ambas desde la Fase 1.
- **Motivo:** el hardware del equipo lo obliga. En las fases 1 y 2 el costo es bajo, porque Debian publica los mismos paquetes en ambas; detectar diferencias temprano es parte del aprendizaje.
- **Consecuencias:** cada bloque se prueba dos veces. En la Fase 4 habrá dos builds de ISO. Raspberry Pi (también arm64) quedará más cerca cuando llegue el hardware.
- **Estado:** Aprobada.

## ADR-003 — VMware en todas las máquinas

- **Fecha:** 2026-09-23
- **Contexto:** Santiago tiene UTM y VMware Fusion disponibles en la Mac; el compa usa VMware en Windows.
- **Opciones:** UTM en la Mac + VMware en Windows / VMware en todos lados.
- **Decisión:** VMware Fusion Pro en la Mac y VMware Workstation Pro en Windows (ambos gratuitos para uso personal). UTM queda como herramienta secundaria para emular amd64 en la Mac y probar compatibilidad con QEMU.
- **Motivo:** las mismas guest tools (`open-vm-tools`) en todas las máquinas simplifican el script y eliminan una rama condicional. En general, Fusion tiene mejor aceleración gráfica que UTM.
- **Consecuencias:** descargar desde el portal de Broadcom (requiere cuenta). Los supuestos S-1 a S-3 del TRD se validan en la Fase 1; si fallan, el plan B es UTM.
- **Estado:** Aprobada.

## ADR-004 — TTY + `startx` + bspwm, sin display manager

- **Fecha:** 2026-09-23
- **Contexto:** se busca un sistema de consola que pueda abrir apps gráficas, con iconos y animaciones, pero sin interfaz "de sistema".
- **Opciones:** escritorio completo (XFCE, KDE) / display manager + WM / TTY + `startx` + WM tiling / Wayland (Sway, Hyprland).
- **Decisión:** arranque en TTY; `startx` levanta bspwm con sxhkd, polybar, picom, kitty y rofi.
- **Motivo:** enseña cómo funciona el stack gráfico de Linux (Xorg, WM, compositor, procesos). Santiago ya domina esta configuración en Parrot. En VMs, X11 es más estable que Wayland.
- **Consecuencias:** bspwm solo funciona en X11, que es tecnología en retirada; migrar a Wayland implicaría cambiar de WM. Se reevalúa después de la Fase 4.
- **Estado:** Aprobada.

## ADR-005 — Bash para el sistema, Rust para herramientas propias

- **Fecha:** 2026-09-23
- **Contexto:** hace falta elegir lenguajes. Santiago descartó Go.
- **Opciones:** Bash + Python, Bash + Go, Bash + Rust, C.
- **Decisión:** Bash para instalación, configuración y build; Rust para herramientas propias (a partir de la Fase 5).
- **Motivo:** Bash es el idioma nativo de la administración en Linux. Rust ofrece seguridad de memoria, compila a un binario único y su uso en herramientas de seguridad va en aumento.
- **Consecuencias:** Rust tiene una curva empinada. El compa no toca Rust hasta dominar terminal, Git y Bash (ver onboarding). Python queda permitido para prototipos rápidos, pero no para herramientas oficiales.
- **Estado:** Aprobada.

## ADR-006 — Copiar un subconjunto curado de los dotfiles

- **Fecha:** 2026-09-23
- **Contexto:** la interfaz de T3SL4 se basa en los dotfiles personales de Santiago (`4nubiX/dotfiles`), que incluyen configuración de cinco escritorios, rutas fijas y algunos archivos sensibles.
- **Opciones:** submódulo Git del repo personal / copiar todo / copiar un subconjunto curado a `config/`.
- **Decisión (propuesta):** copiar a `config/` solo lo necesario, corregido para ser portable. El repo personal no se modifica.
- **Motivo:** la distro necesita configuración portable y sin datos personales, y un submódulo arrastraría todo el ruido y los archivos sensibles.
- **Consecuencias:** las dos copias pueden divergir, y eso se acepta: T3SL4 tiene su propia identidad. El detalle de qué entra y qué no está en [`03-ui-ux.md`](03-ui-ux.md#5-curación-de-los-dotfiles).
- **Estado:** Propuesta. Se implementa en el bloque 2c.

## ADR-007 — La IA no escribe el código del proyecto

- **Fecha:** 2026-09-23
- **Contexto:** el objetivo principal del proyecto es que el equipo aprenda a programar y a desarrollar.
- **Opciones:** la IA implementa / la IA guía y el equipo implementa.
- **Decisión:** en código, la IA trabaja con autonomía **nivel 1** (explica, propone, revisa, hace preguntas). En documentación, con **nivel 2** (redacta en bloques pequeños, con aprobación).
- **Motivo:** si la IA escribe el código, el proyecto avanza pero el equipo no aprende (R-003).
- **Consecuencias:** se avanza más lento, pero con aprendizaje real. Se permiten ejemplos pequeños para explicar un concepto, pero no el entregable completo.
- **Estado:** Aprobada.

## ADR-008 — Licencia

- **Fecha:** 2026-09-23
- **Contexto:** el código propio (scripts, configuración, herramientas en Rust) necesita una licencia antes de hacer público el repo.
- **Opciones:** GPL-3.0 / MIT / sin licencia (repo privado).
- **Propuesta:** GPL-3.0, que es lo habitual en distros y en el ecosistema Debian, y obliga a que los derivados sigan siendo libres.
- **Pendiente:** decisión de Santiago y definir si el repo será público o privado.
- **Estado:** Propuesta.

## ADR-009 — Flujo Git del equipo

- **Fecha:** 2026-09-23
- **Contexto:** dos personas con niveles muy distintos trabajan en el mismo repo.
- **Propuesta:** `main` protegida; todo cambio va en una rama (`feature/`, `fix/`, `docs/`, `spike/`) y entra por PR revisado por Santiago. Commits en el formato `tipo: descripción` (metodología §32.4).
- **Motivo:** la revisión de PRs también es parte del aprendizaje y protege `main`.
- **Pendiente:** aprobación de Santiago, el usuario de GitHub del compa y la configuración de la protección de rama.
- **Estado:** Propuesta.
