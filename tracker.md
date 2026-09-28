# Tracker — T3SL4

| Campo | Valor |
|---|---|
| Proyecto | T3SL4, distro Linux para ciberseguridad basada en Debian 13 |
| Cuenta/contexto | Personal |
| Clasificación | Tipo B (proyecto personal, fin de aprendizaje) |
| Nivel documental | Mediano (metodología v1.3, §7.2) |
| Autonomía de la IA | Nivel 2 en documentación; nivel 1 en código (ADR-007) |
| Estado | Spec → Laboratorio |
| Fase actual | Fase 0 cerrada; inicia la Fase 1 (laboratorio) |
| Gate actual | Gate 1 (spec lista): **GO**. Siguiente: Gate 2 (arquitectura aprobada, al cerrar la Fase 1) |
| Última acción | 2026-09-24: el repo ya es público (ADR-008 cumplido); se corrigió la guía de Fusion (el cask de Homebrew ya no existe) |
| Próximo paso | Santiago: ruleset de `main`, Fusion + VM arm64 ([guía](docs/fase-1-laboratorio.md)). @P01ar7: misión M1 del onboarding |
| Bloque actual | Fase 1: laboratorio |
| Riesgos | Ver matriz en [`docs/01-prd.md`](docs/01-prd.md#9-riesgos) |
| Archivos tocados | tracker.md, CHANGELOG.md, docs/fase-1-laboratorio.md |
| Documentos faltantes | `11-runbook.md` (entra en la Fase 4) |
| Último handoff | [`docs/handoffs/2026-09-24-cierre-gate-1.md`](docs/handoffs/2026-09-24-cierre-gate-1.md) |
| Fecha de actualización | 2026-09-24 |

## Acciones manuales pendientes (Santiago, en GitHub)

| Acción | Dónde | Por qué |
|---|---|---|
| Crear ruleset en `main`: PR obligatorio + 1 aprobación | Settings → Rules → Rulesets | ADR-009: @P01ar7 tiene *write* y hoy podría hacer push directo a `main` |

## Pendiente sin fecha

| Tema | Bloquea |
|---|---|
| Ritmo de trabajo (horas por semana, cada cuánto se sincronizan) | Planear fechas |

## Checklist de la Fase 0 (Gate 1)

- [x] Visión
- [x] PRD con alcance, fuera de alcance y DoD por fase
- [x] TRD
- [x] UI/UX
- [x] Plan de pruebas
- [x] ADRs de las decisiones tomadas
- [x] Onboarding del compa
- [x] Decisiones pendientes cerradas (ADR-008, ADR-009, visibilidad)
- [x] Revisión de Santiago (2026-09-24)
