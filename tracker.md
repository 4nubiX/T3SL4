# Tracker — T3SL4

| Campo | Valor |
|---|---|
| Proyecto | T3SL4, distro Linux para ciberseguridad basada en Debian 13 |
| Cuenta/contexto | Personal |
| Clasificación | Tipo B (proyecto personal, fin de aprendizaje) |
| Nivel documental | Mediano (metodología v1.3, §7.2) |
| Autonomía de la IA | Nivel 2 en documentación; nivel 1 en código (ADR-007) |
| Estado | Spec |
| Fase actual | Fase 0: cimientos documentales |
| Gate actual | Gate 0 (idea válida): **GO**. Siguiente: Gate 1 (spec lista) |
| Última acción | 2026-09-23: se creó la documentación base de la Fase 0 |
| Próximo paso | Santiago revisa la Fase 0 y cierra las decisiones pendientes (ADR-008 y ADR-009) |
| Bloque actual | Fase 0, bloque 1: documentación base |
| Riesgos | Ver matriz en [`docs/01-prd.md`](docs/01-prd.md#9-riesgos) |
| Archivos tocados | README.md, CHANGELOG.md, tracker.md, docs/* |
| Documentos faltantes | `11-runbook.md` (entra en la Fase 4) |
| Último handoff | [`docs/handoffs/2026-09-23-arranque.md`](docs/handoffs/2026-09-23-arranque.md) |
| Fecha de actualización | 2026-09-23 |

## Decisiones pendientes

| ID | Tema | Opciones | Bloquea |
|---|---|---|---|
| ADR-008 | Licencia | GPL-3.0 (propuesta) / MIT | Hacer público el repo |
| ADR-009 | Flujo Git del equipo | `main` protegida + `feature/*` + PR (propuesta) | Que el compa haga su primer PR |
| — | Visibilidad del repo | Público / privado | ADR-008 |
| — | Usuario de GitHub del compa | — | Darle acceso de colaborador |
| — | Ritmo de trabajo | Horas por semana y cada cuánto se sincronizan | Planear fechas |

## Checklist de la Fase 0 (Gate 1)

- [x] Visión
- [x] PRD con alcance, fuera de alcance y DoD por fase
- [x] TRD
- [x] UI/UX
- [x] Plan de pruebas
- [x] ADRs de las decisiones tomadas
- [x] Onboarding del compa
- [ ] Decisiones pendientes cerradas (ADR-008, ADR-009, visibilidad)
- [ ] Revisión de Santiago
