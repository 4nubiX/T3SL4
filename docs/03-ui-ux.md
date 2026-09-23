# 03 — UI/UX

## 1. Filosofía

**Terminal primero.** El sistema no tiene interfaz gráfica "de sistema":
ni gestor de inicio gráfico, ni panel de control, ni menú de inicio, ni
iconos en el escritorio. Lo gráfico existe solo para las herramientas que
lo necesitan, pero con personalidad: iconos, colores y animaciones.

## 2. Flujo de arranque

```txt
Encender VM
  │
  ▼
GRUB (tema T3SL4, fase posterior)
  │
  ▼
Login en TTY (texto) ──────────────► Trabajo 100% en terminal
  │                                   (nmap, ssh, scripts, nvim)
  │ usuario escribe: startx
  ▼
Xorg + bspwm
  ├── polybar   (workspaces, red, VPN, objetivo, hora)
  ├── picom     (transparencias / animaciones)
  ├── feh       (fondo)
  └── sxhkd     (atajos de teclado)
  │
  │ super + shift + q
  ▼
De vuelta en TTY
```

### Flujos de error

| Situación | Comportamiento esperado |
|---|---|
| `startx` falla (driver, config rota) | Regresa a TTY con el error visible y el log en `~/.local/share/xorg/` |
| picom va lento o falla | bspwm funciona sin compositor; se activa el modo ligero |
| polybar falla | bspwm sigue usable; los atajos no dependen de polybar |

## 3. Modos visuales

| Modo | picom | Cuándo |
|---|---|---|
| **Completo** | Animaciones, transparencias, sombras | VM con buena aceleración 3D o hardware nativo |
| **Ligero** | Sin animaciones (o sin picom) | VM sin aceleración o que va lenta |

Cómo se elige el modo se define en el bloque 2b (variable de configuración o detección).

## 4. Atajos base

Heredados del `sxhkdrc` de Santiago y sujetos a revisión en el bloque 2c:

| Atajo | Acción |
|---|---|
| `super + Return` | Abrir kitty |
| `super + d` | Lanzador (rofi) |
| `super + w` | Cerrar ventana |
| `super + shift + w` | Matar ventana |
| `super + m` | Alternar tiled / monocle |
| `super + {t, s, f}` | Ventana tiled / flotante / pantalla completa |
| `super + flechas` | Mover el foco |
| `super + shift + flechas` | Intercambiar ventanas |
| `super + Escape` | Recargar sxhkd |
| `super + shift + r` | Reiniciar bspwm |
| `super + shift + q` | Salir de bspwm (vuelve a TTY) |

## 5. Curación de los dotfiles

Fuente: el repo personal `4nubiX/dotfiles` (configuración de Parrot).
Se **copia un subconjunto curado** a `config/` en este repo (ADR-006); el repo personal no se modifica.

| Entra | No entra (motivo) |
|---|---|
| `bspwm/` (sin `bspwmrc.save`) | KDE, XFCE, LXDE, MATE, Openbox, Caja (otros escritorios) |
| `sxhkd/` | `pulse/` (incluye una cookie de autenticación) |
| `polybar/` y sus scripts | `dconf/user` (binario) |
| `picom/` | `kwalletrc`, `keepassxc/` (configuración personal y sensible) |
| `kitty/` | `autostart/` de MATE |
| `nvim/` | `opensnitch/`, `bleachbit/` (se evalúan aparte) |
| `rofi` (temas en `polybar/scripts/themes`) | |

Correcciones obligatorias al importar:

- Quitar rutas fijas (`/home/anubis/...`) → `$HOME`.
- Quitar `/opt/kitty/bin/kitty` → `kitty` (paquete de Debian).
- Quitar `vmware-user-suid-wrapper` fijo del `bspwmrc` → lo maneja el script según el hipervisor.
- Revisar las licencias de las fuentes incluidas (Helvetica **no** es libre y no se redistribuye).

## 6. Estados visuales en polybar

| Módulo | Con dato | Sin dato |
|---|---|---|
| Objetivo | `󰯐 10.10.10.5 - máquina` | `󰓾 No target` |
| VPN | IP de la interfaz VPN | Oculto o "sin VPN" |
| Ethernet | IP local | "desconectado" |
