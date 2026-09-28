# Referencias

Material externo que sirve para consultar o comparar. **Nada de esta carpeta
se usa en el build de T3SL4.**

| Archivo | Qué es | Para qué sirve | Origen |
|---|---|---|---|
| [`arch-pkglist.x86_64.txt`](arch-pkglist.x86_64.txt) | Lista de los 455 paquetes (nombre y versión) de un ISO de **Arch Linux** x86_64: `archinstall`, `pacman`, `mkinitcpio-archiso`, etc. | Comparar qué trae una imagen live de otra distro al armar nuestra lista de paquetes (bloques 2a y 2d) y la del ISO con `live-build` (Fase 4) | Subido por @P01ar7 el 2026-09-23 como `pkglist.x86_64.txt` en la raíz; se movió aquí el 2026-09-28 |

## Ojo al comparar

Los nombres de paquete de Arch **no** coinciden siempre con los de Debian
(por ejemplo, `base` en Arch no existe en Debian). Antes de agregar un paquete a T3SL4, verifícalo con
`apt policy <paquete>` en una VM Debian 13.
