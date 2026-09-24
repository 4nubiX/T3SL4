# 00 — Visión

## Por qué existe

Parrot y Kali ya existen y funcionan. T3SL4 no nace para reemplazarlos,
sino para **entender por dentro lo que hacen**: cómo arranca un sistema,
cómo se instala y configura software, cómo se arma un entorno gráfico
mínimo y cómo se empaqueta y distribuye todo eso.

La mejor forma de aprenderlo es construir una distro propia, pieza por pieza.

## Qué es T3SL4

Una distro basada en Debian 13 orientada a ciberseguridad, que:

- Arranca en **consola (TTY)**, sin gestor gráfico de inicio.
- Levanta una capa gráfica mínima con `startx` solo cuando hace falta
  (navegador, Wireshark, Burp, etc.).
- Tiene personalidad propia: iconos, colores y animaciones, pero **sin
  "sistema en UI"**: nada de panel de control, menú de inicio ni escritorio con iconos.
- Corre en máquinas virtuales arm64 y amd64 y, más adelante, en Raspberry Pi
  y hardware nativo.
- Incluye herramientas propias escritas en Rust.

## Para quién

1. **Santiago:** nivel avanzado en Bash y Git, usuario diario de Parrot. Lidera y revisa.
2. **Su compa:** parte de cero. Aprende Linux, terminal, Git y Bash en el camino.
3. **Después, quizá:** cualquier persona que quiera una distro de seguridad
   minimalista y entendible.

## Principios

- **Entender antes que acumular.** Una herramienta entra solo si sabemos por qué está.
- **Terminal primero.** Lo gráfico es un complemento, no el centro.
- **Portable.** Nada de rutas fijas como `/home/usuario` ni supuestos de una sola máquina.
- **Seguro por defecto.** Hardening básico desde la primera versión instalable.
- **Pequeño y probado.** Cada bloque se prueba en arm64 y amd64 antes de seguir.

## Cómo sabremos que funcionó

- Los dos integrantes pueden explicar qué hace cada parte de T3SL4.
- Existe un ISO booteable propio para amd64 y arm64.
- Existe al menos una herramienta propia en Rust que se usa de verdad.
- El compa hizo commits y PRs reales al proyecto.
