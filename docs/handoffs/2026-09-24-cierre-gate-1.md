# Handoff — T3SL4 — 2026-09-24

## Contexto
Continuación de [`2026-09-23-arranque.md`](2026-09-23-arranque.md). La Fase 0 estaba documentada,
con ADR-008 (licencia) y ADR-009 (flujo Git) pendientes.

## Objetivo de la sesión
Cerrar las decisiones pendientes y pasar el Gate 1.

## Estado actual
**Gate 1: GO.** Fase 0 cerrada. Inicia la Fase 1 (laboratorio). Todavía no hay código.

## Qué se hizo
- ADR-008 aprobado: **GPL-3.0** y **repo público**. Se agregó `LICENSE` con el texto canónico de GPL-3.0.
- ADR-009 aprobado: `main` protegida, ramas y PR revisado por Santiago.
- Se verificó en GitHub que **@P01ar7** (el compa) es colaborador con permiso *write*.
- Se creó `docs/fase-1-laboratorio.md` con la instalación de VMware Fusion, la VM arm64 y la validación de supuestos.
- Se agregó la sección de equipo en el README.

## Decisiones tomadas
- ADR-008 y ADR-009 (ver [`../10-decisions-adr.md`](../10-decisions-adr.md)).

## Archivos tocados
- `LICENSE` (nuevo)
- `README.md`, `CHANGELOG.md`, `tracker.md`
- `docs/01-prd.md`, `docs/10-decisions-adr.md`, `docs/onboarding.md`
- `docs/fase-1-laboratorio.md` (nuevo)
- `docs/handoffs/2026-09-24-cierre-gate-1.md` (este archivo)

## Pruebas / validaciones
Solo documentación: se revisaron los enlaces internos y el colaborador en GitHub.

## Riesgos o fragilidades
- **Hasta que exista el ruleset, `main` no está protegida.** @P01ar7 puede hacer push directo.
- Los pasos del portal de Broadcom pueden cambiar; la guía de la Fase 1 lo advierte.
- VMware Fusion **no** lo puede instalar la IA: la sesión corre en un contenedor en la nube, sin acceso a la Mac.

## Pendientes
- Santiago: crear el ruleset en `main` y cambiar el repo a público (acciones manuales en GitHub).
- Santiago: instalar Fusion, crear la VM arm64 y validar los supuestos S-1, S-2, S-4 y S-5.
- @P01ar7: misiones M1 a M6 del onboarding.
- Definir el ritmo de trabajo.

## Próximo paso recomendado
Seguir [`../fase-1-laboratorio.md`](../fase-1-laboratorio.md) y traer los resultados de
`apt policy` para registrarlos y cerrar los supuestos.

## Cuenta / herramienta usada
Cuenta personal, Claude Code (sesión remota). Rama `claude/hola-claude-8tkii7`.

## Texto para tracker
- Estado: Laboratorio
- Última acción: Gate 1 GO; ADR-008 y ADR-009 aprobados; LICENSE y guía de la Fase 1
- Próximo paso: ruleset + repo público + VM arm64; M1 para @P01ar7
- Riesgos: `main` sin proteger hasta crear el ruleset
