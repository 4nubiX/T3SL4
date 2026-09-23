# 01 — PRD (Product Requirements Document)

## 1. Problema

Los integrantes del equipo quieren aprender en serio Linux, Bash, Git y Rust.
Usar Parrot o Kali no enseña cómo están construidos. Hace falta un proyecto
real, con alcance controlado, que obligue a entender cada pieza.

## 2. Usuarios

| Usuario | Nivel | Qué necesita |
|---|---|---|
| Santiago | Bash 8-9/10, Git 7/10, usuario de Parrot | Construir la distro, profundizar en empaquetado, ISO y Rust |
| Compa | Desde cero | Ruta guiada: terminal → Git → Bash → contribuir |

## 3. Objetivo

Construir por fases una distro basada en Debian 13 para ciberseguridad,
de consola primero y con una capa gráfica mínima, que corra en VMs arm64
y amd64, y que el equipo entienda completa.

## 4. Alcance por fase y Definition of Done

### Fase 0 — Cimientos documentales
- **Incluye:** documentación base, decisiones registradas y onboarding.
- **Terminado cuando:** existen los documentos de la Fase 0, las decisiones
  pendientes están cerradas y Santiago aprobó (Gate 1).

### Fase 1 — Laboratorio
- **Incluye:** Debian 13 netinst sin escritorio instalado en VMware
  (arm64 en la Mac y amd64 en Windows), snapshot `base-limpia` y las misiones 1 a 5 del onboarding.
- **Terminado cuando:** las dos VMs arrancan en TTY, tienen snapshot, el compa
  hizo su primer commit y se validaron los supuestos técnicos del TRD (§8).

### Fase 2 — `install.sh` v0.1
- **Incluye:** un script que convierte un Debian 13 mínimo en T3SL4.
  Se hace en bloques:
  - 2a: paquetes base y detección de arquitectura
  - 2b: stack gráfico y `startx`
  - 2c: dotfiles curados y portables
  - 2d: herramientas de seguridad
  - 2e: hardening básico
- **Terminado cuando:** en una VM recién restaurada al snapshot `base-limpia`,
  el script corre sin errores en arm64 y en amd64, al reiniciar se llega a TTY
  y `startx` levanta bspwm con polybar. Una segunda ejecución no rompe nada (idempotencia).

### Fase 3 — Paquetes `.deb`
- **Incluye:** `t3sl4-core`, `t3sl4-dotfiles` y `t3sl4-tools` como metapaquetes y paquetes de configuración.
- **Terminado cuando:** instalar los `.deb` produce el mismo resultado que `install.sh`.

### Fase 4 — ISO propia
- **Incluye:** ISO live instalable con `live-build`, primero amd64 y luego arm64. Aquí nace `11-runbook.md`.
- **Terminado cuando:** el ISO arranca en VMware en ambas arquitecturas y se puede instalar.

### Fase 5 — Herramientas en Rust
- **Incluye:** la primera herramienta, `t3sl4 target` (reemplaza `victim_to_hack.sh` y `settarget`).
- **Terminado cuando:** está empaquetada, documentada y se usa desde polybar.

## 5. Fuera de alcance (por ahora)

- Escribir un kernel o modificar el kernel de Debian.
- Soporte para Raspberry Pi (hasta tener hardware).
- Instalación nativa en hardware real (hasta la Fase 4 o después).
- Wayland (bspwm es solo X11; se reevalúa después de la Fase 4).
- Igualar el catálogo de herramientas de Kali o Parrot.
- Instalador gráfico propio.
- Repositorio APT público propio.

## 6. Casos de uso

1. Arrancar la VM, iniciar sesión en TTY y trabajar solo en terminal (nmap, ssh, scripts).
2. Ejecutar `startx`, abrir Wireshark o el navegador en bspwm y volver a TTY al salir.
3. Fijar un objetivo de pentest y verlo en polybar.
4. Restaurar un snapshot y reinstalar T3SL4 desde cero en minutos.

## 7. Criterios de éxito

Ver [`00-vision.md`](00-vision.md#cómo-sabremos-que-funcionó).

## 8. Restricciones

- Hardware disponible: MacBook Pro M5 Pro (arm64), PC Windows 11 con Core Ultra 7 (amd64)
  y Asus Vivobook con i7 (amd64).
- Tiempo limitado: los dos estudian o trabajan.
- Uno de los integrantes parte de cero.

## 9. Riesgos

| ID | Riesgo | Tipo | Prob. | Impacto | Mitigación | Estado | Dueño |
|---|---|---|---|---|---|---|---|
| R-001 | El alcance crece sin control ("querer ser Kali") | Entrega | Alta | Alto | Fases con gates; lo que no es de la fase actual va al backlog | Abierto | Santiago |
| R-002 | El compa se atora con la curva y abandona | Entrega | Media | Alto | Onboarding con misiones chicas y logros frecuentes | Abierto | Santiago |
| R-003 | La IA termina escribiendo el código y el equipo no aprende | Entrega | Media | Alto | ADR-007: autonomía nivel 1 en código | Mitigado | Santiago |
| R-004 | picom con animaciones va lento en las VMs | Técnico | Alta | Medio | Modo "ligero" sin animaciones (ver UI/UX) | Abierto | Equipo |
| R-005 | Algo funciona en arm64 y falla en amd64, o al revés | Técnico | Media | Medio | Ningún bloque se cierra sin pasar la matriz de pruebas | Abierto | Equipo |
| R-006 | Se cuelan credenciales o archivos privados desde los dotfiles | Seguridad | Baja | Medio | Curar archivo por archivo, `.gitignore` y revisión antes de cada commit | Abierto | Santiago |
| R-007 | Un script rompe la VM | Operativo | Alta | Bajo | Snapshot antes de cada prueba | Mitigado | Equipo |
| R-008 | Los supuestos sobre VMware Fusion arm64 resultan falsos | Técnico | Media | Medio | Validarlos en la Fase 1; UTM como plan B | Abierto | Santiago |
