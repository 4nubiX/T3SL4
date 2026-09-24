# Fase 1 — Laboratorio

Objetivo: tener una VM de Debian 13 **sin escritorio** en cada arquitectura,
con snapshot `base-limpia`, y validar los supuestos S-1 a S-5 del
[TRD](02-trd.md#8-supuestos-a-validar-en-la-fase-1).

| Quién | Máquina | Guía |
|---|---|---|
| Santiago | MacBook Pro M5 Pro → **arm64** | Esta página |
| @P01ar7 | Asus Vivobook → **amd64** | Misión M1 de [`onboarding.md`](onboarding.md) |

> Los portales de descarga cambian con frecuencia. Si un paso no coincide con
> lo que ves en pantalla, anota la diferencia en el handoff y actualiza esta guía.

## 1. Instalar VMware Fusion Pro (Mac)

1. Entra al portal de soporte de Broadcom (`support.broadcom.com`) y crea una cuenta gratuita si no tienes.
2. Ve a **My Downloads → VMware Fusion → VMware Fusion Pro for Personal Use** y elige la versión más reciente.
3. Acepta los términos y descarga el `.dmg`.
4. Instala arrastrando la app a `Applications` y ábrela.
5. macOS pedirá permisos (**System Settings → Privacy & Security**). Concédelos y reinicia Fusion si lo pide.
6. Si pide licencia, elige la opción **uso personal**.

> Homebrew **ya no** sirve: el cask `vmware-fusion` fue retirado
> (verificado el 2026-09-24). La única vía es el portal de Broadcom.

## 2. Descargar Debian 13 arm64

En `debian.org/distrib/netinst` descarga el **netinst** para **arm64**
(`debian-13.x.x-arm64-netinst.iso`). Verifica el checksum con el `SHA256SUMS` publicado junto al ISO:

```bash
shasum -a 256 debian-13.*-arm64-netinst.iso
```

## 3. Crear la VM en Fusion

1. **File → New → Install from disc or image** y arrastra el ISO.
2. Sistema operativo: la opción más reciente de **Debian 64-bit Arm**. Si Debian 13 no aparece,
   usa Debian 12 o "Other Linux 6.x kernel 64-bit Arm"; solo afecta los valores por defecto.
3. **Customize Settings** antes de arrancar:
   - Processors & Memory: 4 vCPU, 8 GB de RAM
   - Hard Disk: 50 GB
   - Display: activa **Accelerate 3D Graphics** (para el supuesto S-3)
   - Network: NAT (Share with my Mac)

## 4. Instalar Debian

Igual que la misión M1 del onboarding. Lo importante es la pantalla **Software selection**:

- ❌ Ningún escritorio ("Debian desktop environment", GNOME, Xfce, etc.)
- ✅ "SSH server" y "standard system utilities"

Al terminar debe arrancar en un **login de texto**.

## 5. Snapshot

**Virtual Machine → Snapshots → Take Snapshot** con el nombre `base-limpia`.
A partir de aquí, **toda** prueba empieza restaurando este snapshot.

## 6. Validar los supuestos (S-1 a S-5)

En la VM, ya con sesión iniciada:

```bash
dpkg --print-architecture          # esperado: arm64
systemd-detect-virt                # esperado: vmware
sudo apt update
apt policy open-vm-tools picom bspwm sxhkd polybar kitty rofi feh
```

Anota en el handoff, por cada paquete, si existe y qué versión trae
(para picom: ¿es la 12 o mayor?). S-3 (aceleración 3D útil para picom) se valida
hasta el bloque 2b, cuando exista la capa gráfica.

| Supuesto | Resultado | Notas |
|---|---|---|
| S-1 Debian 13 arm64 corre en Fusion | | |
| S-2 `open-vm-tools` disponible en arm64 | | |
| S-3 Aceleración 3D útil para picom | Pendiente (bloque 2b) | |
| S-4 picom ≥ 12 | | |
| S-5 Stack gráfico disponible en ambas arquitecturas | | Comparar con la VM amd64 de @P01ar7 |

## 7. Cierre de la Fase 1

La Fase 1 termina (y se evalúa el Gate 2) cuando se cumple el checklist de la
[Fase 1 en el plan de pruebas](05-test-plan.md#fase-1--laboratorio) y @P01ar7 fusionó su primer PR (misión M6).
