# Copa Mundial Repsol — Notas del proyecto

Aplicación web estática (HTML/CSS/JS puro) para mostrar fixtures y resultados de un torneo multideporte interno de Repsol. Sin backend — toda la comunicación entre páginas usa `localStorage`.

---

## Archivos

| Archivo | Rol |
|---|---|
| `index.html` | Pantalla de proyección — muestra imágenes con resultados superpuestos |
| `panel_fase1.html` | Panel del operador — formulario para ingresar resultados Fase 1 |
| `panel.html` | Panel del operador — formulario para clasificados Fecha 3 (fixture) |
| `calibrar.html` | Herramienta de calibración visual de posición de cápsulas |
| `resultados1/` | Imágenes JPG del fixture completo (las 3 fases) por deporte |

---

## Deportes y partidos — Fase 1

**Fútbol Masculino** (`resultados1/futbol_masculino.jpg`)
- 3 campos simultáneos, 2 partidos por campo
- C1: 07:40 Marruecos vs Brasil / 08:30 Marruecos vs Portugal
- C2: 07:40 Francia vs Inglaterra / 08:30 Francia vs España
- C3: 07:40 Argentina vs Ecuador / 08:30 Argentina vs Países Bajos

**Vóley Mixto** (`resultados1/voley.jpg`)
- Grupo A: EE.UU, Suiza, Corea del Sur, Uruguay, México
- Grupo B: Perú, Senegal, Japón, Croacia, Irán
- Fecha 1 y 2 están lado a lado en la misma imagen
- C1: 07:40 / 08:20 / 09:00 / 09:40 / 10:20
- C2: 07:40 / 08:20 / 09:00 / 09:40 / 10:20

**Pádel** (`resultados1/padel.jpg`)
- Grupo A: Arabia S., Paraguay, Suecia, Turquía, Austria
- Grupo B: Canadá, Panamá, Noruega, Australia, Egipto
- Layout igual a Vóley (Fecha 1 izquierda, Fecha 2 derecha)
- C1: 07:40 / 08:10 / 08:40 / 09:10 / 09:40
- C2: 07:40 / 08:10 / 08:40 / 09:10 / 09:40

**Fútbol Femenino** (`resultados1/futbol_femenino.jpg`)
- 1 grupo: Alemania, Bélgica, Colombia
- No tiene Fecha 1 — empieza en Fecha 2 (= Fase 1 de este deporte)
- C2: 07:00 Alemania vs Bélgica / 07:50 Alemania vs Colombia / 08:40 Bélgica vs Colombia

---

## Arquitectura técnica

**Comunicación entre páginas:** `localStorage`
- `fase1_data` — resultados ingresados en panel_fase1.html
- `calibration_coords` — posiciones calibradas de cápsulas (calibrar.html → index.html)

**Actualización en tiempo real:** `window.addEventListener('storage', ...)` detecta cambios y re-renderiza el canvas automáticamente.

**Sistema de canvas:**
- Imágenes cargadas con dimensiones naturales
- Cápsulas posicionadas con coordenadas en **porcentaje** (xPct, yPct) — independientes de resolución
- Función clave: `drawScorePill(ctx, text, xPct, yPct, W, H)`

**Estilo de cápsulas (Fase 1):**
- Color: `#09bdd2` (cyan)
- Ancho: `W * 0.058 + 6`
- Alto: `H * 0.024`
- Fondo: `#000A14`

---

## Posiciones calibradas (FASE1_COORDS en index.html)

```
fm (Fútbol Masculino):
  C1: x=36.0  y=38.8 / 42.3
  C2: x=60.5  y=38.5 / 42.5
  C3: x=85.5  y=38.5 / 42.5

vl (Vóley Mixto):
  C1: x=26.0  y=43.0 / 46.5 / 50.0 / 53.5 / 57.0
  C2: x=47.3  y=43.0 / 46.5 / 50.0 / 53.5 / 57.0

pd (Pádel):
  C1: x=27.0  y=44.5 / 47.5 / 50.5 / 53.5 / 56.5
  C2: x=46.5  y=44.5 / 47.5 / 50.5 / 54.0 / 57.0

ff (Fútbol Femenino):
  C2: x=71.0  y=44.5 / 48.0 / 52.0
```

---

## Flujo de uso en el evento

1. Operador abre `panel_fase1.html` en su dispositivo
2. Pantalla de proyección abre `index.html`
3. Operador escribe resultado (ej: `2-1`) → aparece en pantalla en tiempo real
4. Para recalibrar posiciones: abrir `calibrar.html`, ajustar, y usar "Fijar posiciones en index.html"

## Para GitHub

Las posiciones están hardcodeadas en `FASE1_COORDS` dentro de `index.html` — funcionan sin localStorage en cualquier dispositivo. Si se recalibra, copiar el bloque generado por `calibrar.html` y reemplazar `FASE1_COORDS` en `index.html`.
