# 02 — TRD (Especificación técnica)

## 1. Arquitectura por capas

```txt
┌─────────────────────────────────────────────────────────┐
│  Herramientas propias (Rust)        t3sl4 target, ...   │  Fase 5
├─────────────────────────────────────────────────────────┤
│  Capa gráfica mínima (X11)                              │
│  startx → bspwm + sxhkd + polybar + picom + kitty + rofi│  Fase 2b-2c
├─────────────────────────────────────────────────────────┤
│  Herramientas de seguridad          nmap, wireshark, ...│  Fase 2d
├─────────────────────────────────────────────────────────┤
│  Configuración T3SL4                dotfiles, hardening │  Fase 2c-2e
├─────────────────────────────────────────────────────────┤
│  Debian 13 Trixie mínimo (netinst, sin escritorio)      │  Fase 1
├─────────────────────────────────────────────────────────┤
│  VMware Fusion (arm64)   │   VMware Workstation (amd64) │
└─────────────────────────────────────────────────────────┘
```

## 2. Stack

| Componente | Elección | Por qué |
|---|---|---|
| Base | Debian 13 Trixie | Estable actual, la misma base que Parrot y Kali, soporta arm64 y amd64 (ADR-001) |
| Servidor gráfico | Xorg (X11) | bspwm solo funciona en X11 (ADR-004) |
| Gestor de ventanas | bspwm + sxhkd | Tiling, se maneja con teclado, el equipo ya lo conoce |
| Barra | polybar | Iconos (Nerd Fonts) y módulos propios (objetivo, VPN) |
| Compositor | picom | Transparencias y animaciones |
| Terminal | kitty | Aceleración por GPU, configurable |
| Lanzador | rofi | Lanzar apps sin menú de sistema |
| Fondo | feh | Ligero |
| Editor | neovim | Ya configurado en los dotfiles |
| Scripts del sistema | Bash | Lenguaje nativo de administración en Linux (ADR-005) |
| Herramientas propias | Rust | Seguridad de memoria, binario único, uso creciente en seguridad (ADR-005) |
| Build de ISO | live-build | Herramienta oficial de Debian para imágenes live (Fase 4) |

## 3. Arquitecturas y máquinas

| Máquina | CPU | Hipervisor | Arquitectura de la VM | Guest tools |
|---|---|---|---|---|
| MacBook Pro (Santiago) | Apple M5 Pro | VMware Fusion Pro | arm64 | `open-vm-tools` |
| PC Windows (Santiago) | Intel Core Ultra 7 | VMware Workstation Pro | amd64 | `open-vm-tools` |
| Asus Vivobook (compa) | Intel i7 | VMware Workstation Pro | amd64 | `open-vm-tools` |

**Concepto clave:** un hipervisor **virtualiza**; no cambia la arquitectura.
La VM usa la del CPU físico. Correr amd64 en la Mac requiere **emulación**
(UTM/QEMU), que funciona pero es más de 10 veces más lenta. Solo sirve para pruebas puntuales.

## 4. Requisitos mínimos de la VM

| Recurso | Mínimo | Recomendado |
|---|---|---|
| vCPU | 2 | 4 |
| RAM | 4 GB | 8 GB |
| Disco | 30 GB | 50 GB |
| Gráficos | — | Aceleración 3D activada (para picom) |
| Red | NAT | NAT (y host-only para laboratorios) |

## 5. Diseño de `install.sh` (Fase 2)

Principios que el script debe cumplir. El código lo escribe el equipo:

1. **Modo estricto:** `set -euo pipefail`.
2. **Idempotente:** correrlo dos veces no debe romper nada ni duplicar configuración.
3. **Detección de arquitectura** con `dpkg --print-architecture` (`arm64` o `amd64`),
   con ramas explícitas y un error claro si es otra.
4. **Detección del hipervisor** con `systemd-detect-virt`, para instalar las guest tools correctas.
5. **Nada de rutas fijas:** usar `$HOME` y la variable del usuario objetivo, nunca `/home/<nombre>`.
6. **Nada de `curl | bash`:** todo sale de los repos de Debian o del propio repo.
7. **Log** de cada paso en un archivo, además de la salida en pantalla.
8. **Modular:** un archivo por bloque (2a…2e), orquestado desde `install.sh`.
9. **Validación estática:** debe pasar `shellcheck` sin advertencias.

## 6. Estructura del repo propuesta

Se crea conforme avanzan las fases; no se generan carpetas vacías.

```txt
T3SL4/
├── install.sh              # Fase 2: orquestador
├── scripts/                # Fase 2: un script por bloque
├── config/                 # Fase 2c: dotfiles curados (bspwm, sxhkd, polybar, ...)
├── packages/               # Fase 3: fuentes de los .deb
├── live-build/             # Fase 4: configuración del ISO
├── tools/                  # Fase 5: herramientas en Rust
└── docs/
```

## 7. Dependencias

Todas vienen de los repos oficiales de Debian 13. Cualquier dependencia fuera
de Debian necesita una evaluación documentada (metodología §35).

## 8. Supuestos a validar en la Fase 1

| # | Supuesto | Cómo se valida |
|---|---|---|
| S-1 | VMware Fusion en Apple Silicon corre Debian 13 arm64 sin problemas | Instalar netinst arm64 |
| S-2 | `open-vm-tools` está disponible en Debian 13 para arm64 | `apt policy open-vm-tools` en la VM arm64 |
| S-3 | Fusion ofrece aceleración 3D útil para picom en invitados arm64 | Probar picom con y sin animaciones |
| S-4 | El picom de Debian 13 trae animaciones nativas (versión 12 o mayor) | `apt policy picom` |
| S-5 | bspwm, polybar, kitty y rofi existen en Debian 13 para ambas arquitecturas | `apt policy <paquete>` en las dos VMs |

Si un supuesto falla, se registra y se decide (plan B: UTM en la Mac).

## 9. Límites conocidos

- X11 es tecnología en retirada; migrar a Wayland implicaría cambiar bspwm (fuera de alcance).
- Las animaciones dependen de la aceleración gráfica del hipervisor.
- Por ahora no hay soporte para hardware físico.
