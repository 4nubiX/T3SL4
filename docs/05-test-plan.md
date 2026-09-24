# 05 — Plan de pruebas

## 1. Regla principal

**Ningún bloque se da por terminado sin pasar en las dos arquitecturas.**

| | arm64 (Mac / Fusion) | amd64 (Windows / Workstation) |
|---|---|---|
| Responsable típico | Santiago | Compa (o Santiago en su PC) |
| Snapshot base | `base-limpia` | `base-limpia` |

## 2. Ciclo de prueba

```txt
1. Restaurar snapshot `base-limpia`
2. Clonar el repo / copiar la rama a probar
3. Ejecutar el bloque
4. Revisar resultados (checklist del bloque)
5. Ejecutar el bloque OTRA VEZ → no debe romper nada (idempotencia)
6. Guardar evidencia (salida, log, captura)
7. Anotar el resultado en el PR o en el handoff
```

**Nunca** se prueba sobre una VM sin snapshot previo.

## 3. Validaciones estáticas (antes de ejecutar)

| Validación | Comando | Criterio |
|---|---|---|
| Sintaxis de Bash | `bash -n script.sh` | Sin errores |
| Lint de Bash | `shellcheck script.sh` | Sin advertencias |
| Credenciales | Revisión manual del diff (y más adelante `gitleaks`) | Nada sensible |

## 4. Pruebas por fase

### Fase 1 — Laboratorio
- [ ] La VM arm64 arranca en TTY y tiene red (`ping debian.org`).
- [ ] La VM amd64 arranca en TTY y tiene red.
- [ ] Existe el snapshot `base-limpia` en ambas.
- [ ] Se validaron los supuestos S-1 a S-5 del TRD.

### Fase 2 — `install.sh`
| Caso | Esperado |
|---|---|
| Feliz: VM limpia, con red | Termina sin errores; al reiniciar llega a TTY; `startx` levanta bspwm + polybar |
| Segunda ejecución | Sin errores y sin configuración duplicada |
| Sin red | Falla pronto con un mensaje claro, sin dejar el sistema a medias |
| Arquitectura no soportada | Error claro que dice la arquitectura detectada |
| Sin sudo o sin root | Error claro antes de hacer cambios |
| Fuera de VMware | No instala `open-vm-tools`; avisa |
| picom en VM lenta | El modo ligero deja el sistema usable |

### Fases 3 a 5
El detalle se define al iniciar cada fase.

## 5. Evidencia

- La salida del script o el log se adjunta al PR.
- Las capturas de pantalla se guardan en el PR (no en el repo) salvo que documenten algo permanente.
- La evidencia no debe contener IPs públicas, usuarios reales ni credenciales.

## 6. Criterio de "listo" de un bloque

- [ ] Pasa las validaciones estáticas.
- [ ] Pasa en arm64 y amd64.
- [ ] Es idempotente.
- [ ] Hay evidencia en el PR.
- [ ] La documentación afectada está actualizada.
- [ ] El tracker está actualizado.
