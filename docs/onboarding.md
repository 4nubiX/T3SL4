# Onboarding — ruta desde cero

Esta guía es para quien empieza sin experiencia en Linux. Son **misiones**
chicas: cada una tiene un objetivo y una forma clara de saber que ya la lograste.
No hay prisa. Lo importante es entender, no terminar rápido.

## Reglas del juego

1. **No copies y pegues sin entender.** Si un comando no lo entiendes, pregunta o lee `man <comando>`.
2. **Rompe cosas sin miedo, pero con snapshot.** Para eso existen.
3. **Pregunta "¿por qué?"** a Santiago, a la IA o a la documentación. Siempre hay un porqué.
4. **Anota lo que aprendas.** Si algo te costó, probablemente le va a costar a alguien más.

## Mapa

```txt
M1 VM Debian ─► M2 Terminal ─► M3 Usuarios y permisos ─► M4 Paquetes (apt)
                                                              │
M8 Primer bloque de T3SL4 ◄─ M7 Bash ◄─ M6 Git + GitHub ◄─ M5 Archivos de config
```

---

## M1 — Tu primera VM de Debian (hecha a mano)

**Objetivo:** instalar Debian 13 **sin escritorio** en VMware Workstation, entendiendo cada pantalla.

1. Instala **VMware Workstation Pro** (gratis para uso personal; se descarga desde el portal de Broadcom).
2. Descarga el ISO **netinst de Debian 13 para amd64** desde debian.org.
3. Crea la VM: 2-4 vCPU, 4-8 GB de RAM, 30-50 GB de disco, red NAT.
4. Instala. En la pantalla **"Software selection"**:
   - ❌ Desmarca "Debian desktop environment" y cualquier escritorio.
   - ✅ Deja marcadas "SSH server" y "standard system utilities".
5. Al terminar, toma un snapshot llamado `base-limpia`.

**Lo lograste cuando:** la VM arranca, ves un login en texto y puedes entrar con tu usuario.

**Concepto para entender:** ¿qué es virtualizar y por qué tu VM es amd64?
(Pista: [`02-trd.md`](02-trd.md#3-arquitecturas-y-máquinas).)

## M2 — Moverte en la terminal

**Comandos:** `pwd`, `ls -la`, `cd`, `mkdir`, `touch`, `cp`, `mv`, `rm`, `cat`, `less`, `nano`, `man`.

**Lo lograste cuando:** puedes crear una carpeta `~/practica`, entrar, crear un archivo con `nano`,
escribir tu nombre, verlo con `cat`, copiarlo, renombrarlo y borrarlo, todo sin ayuda.

**Concepto:** ¿qué es `~`? ¿Qué diferencia hay entre una ruta absoluta y una relativa?

## M3 — Usuarios y permisos

**Comandos:** `whoami`, `id`, `sudo`, `su`, `chmod`, `chown`, `ls -l`.

**Lo lograste cuando:** puedes explicar qué significa `-rwxr-xr--` y por qué no se trabaja como root todo el tiempo.

## M4 — Instalar software

**Comandos:** `apt update`, `apt search`, `apt show`, `apt install`, `apt remove`, `apt policy`.

**Lo lograste cuando:** instalas `htop`, lo usas y lo desinstalas, y puedes explicar la diferencia entre `apt update` y `apt upgrade`.

## M5 — Archivos de configuración

**Conceptos:** `/etc`, archivos ocultos (dotfiles), `~/.bashrc`, `~/.config/`.

**Lo lograste cuando:** creas un alias en `~/.bashrc` (por ejemplo `alias ll='ls -la'`), lo cargas con `source` y funciona.
Aquí entiendes por qué se llaman **dotfiles**.

## M6 — Git y GitHub

**Comandos:** `git config`, `git clone`, `git status`, `git add`, `git commit`, `git log`, `git branch`, `git switch`, `git push`.

1. Ya eres colaborador del repo (`@P01ar7`). Si GitHub te pide aceptar una invitación, acéptala.
2. Configura tu nombre y correo en Git.
3. Clona T3SL4 en tu VM.
4. Crea una rama `docs/mi-primer-cambio`.
5. Agrega tu nombre a la sección "Equipo" del final de esta guía.
6. Haz commit con el mensaje `docs: agregar mi nombre al equipo`, haz push y abre un Pull Request.

**Lo lograste cuando:** Santiago aprueba y fusiona tu primer PR. 🎉

## M7 — Bash

**Conceptos:** shebang, variables, `if`, `for`, funciones, códigos de salida (`$?`), `set -euo pipefail`.

**Lo lograste cuando:** escribes un script que diga qué arquitectura tiene la máquina
(`dpkg --print-architecture`) y actúe distinto si es `arm64` o `amd64`. Además, debe pasar `shellcheck` sin advertencias.

Ese script es, literalmente, la primera pieza de `install.sh`.

## M8 — Tu primer bloque de T3SL4

Con Santiago, tomas un bloque chico de la Fase 2, lo implementas en una rama,
lo pruebas con el ciclo de [`05-test-plan.md`](05-test-plan.md) y abres el PR.

**Lo lograste cuando:** tu código es parte de T3SL4.

---

## Equipo

- Santiago (4nubiX)
