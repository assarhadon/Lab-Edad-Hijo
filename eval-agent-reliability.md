# Eval — Fiabilidad de Agent mode: GLM 5.2 vs Claude (harness Continue)

**Fecha inicio:** 2026-07-09
**Objetivo:** comprobar empíricamente la afirmación *"Agent mode con GLM no es confiable todavía para este pipeline"*, convirtiendo una observación aislada en una **tasa medida** sobre repeticiones, con un brazo de control que aísle si el fallo es del modelo o del harness.

---

## Definición operacional de "confiable"

Tarea = rol **Maker** implementando **Issue #2** (lógica de cálculo). Sin intervención humana, el Agent debe completar la cadena:

> leer `spec.md` → editar `index.html` → **guardar a disco** → `git commit` → `git push`

Cada corrida es **PASA** (cadena completa, archivo persistido, commit hecho) o **FALLA** (se colgó / no persistió / error / corrompió).

**Aprobar vs rescatar (regla de clasificación):** hacer click en "Accept" sobre un diff es la **compuerta de aprobación** de Continue — es HITL esperado y **cuenta hacia PASA**. Lo que marca **FALLA** es tener que *rescatar* el Agent: se cuelga, emite tool-call malformado/vacío, corrompe el archivo, o no llega a `commit` + `push`. La pregunta es: **¿tuviste que aprobar, o tuviste que arreglar?** El **modo de aprobación debe ser idéntico en las 20 corridas** (todas manuales o todas auto-apply — no mezclar).

**Métrica:** tasa PASA = (corridas PASA) / N.

## Umbral (fijado ANTES de correr — no mover después)

> **Confiable = ≥ 9/10 corridas PASA.** Por debajo de 9/10, la afirmación "no confiable todavía" queda **respaldada**.

## Prompt fijo (Opción A — pipeline real, bloqueado)

Pégalo **verbatim** en las 20 corridas (brazos A y B). No variar ni una palabra.

```
/maker implementa el Issue #2
```

El Agent obtiene el Issue #2 por su cuenta (`gh issue view 2`) — ese tool-call es parte de lo que se mide.

---

## Setup

```powershell
# UNA sola vez (ya hecho): captura el baseline = index.html con solo Issue #1
cp index.html index.baseline.html

# ANTES DE CADA intento: restaura el baseline para que todas las corridas partan igual
cp index.baseline.html index.html
```

Protocolo por corrida:
1. `cp index.baseline.html index.html` (restaura punto de partida).
2. Abre un chat **NUEVO** de Continue (contexto limpio).
3. Invoca `/maker` para Issue #2 — **mismo prompt exacto** cada vez.
4. Cronometra. Registra el resultado en la tabla.
5. Si se cuelga: mira `Ver → Salida → Continue` y anota el modo de fallo (ver §Diagnóstico).

> Nota: `index.baseline.html` es un archivo temporal de trabajo. No lo commitees (añádelo a `.gitignore` o bórralo al terminar el eval).

---

## Brazo A — GLM-5, modo Agent

| # | ¿Completó loop? | ¿Guardó a disco? | ¿Commit? | ¿Colgó? ¿dónde? | Tiempo | PASA/FALLA | Notas |
|---|:---:|:---:|:---:|---|---|:---:|---|
| 1 | | | | | | | |
| 2 | | | | | | | |
| 3 | | | | | | | |
| 4 | | | | | | | |
| 5 | | | | | | | |
| 6 | | | | | | | |
| 7 | | | | | | | |
| 8 | | | | | | | |
| 9 | | | | | | | |
| 10 | | | | | | | |

**Tasa GLM-Agent = ____ / 10**

---

## Brazo B — Control: Claude, modo Agent (MISMO harness Continue)

> El control debe correr **dentro de Continue**, misma config, cambiando **solo el modelo** a Claude. El panel derecho (Claude Code) es OTRO harness → no sirve de control limpio. Si aún no tienes Claude en Continue, agrégalo por API primero.

| # | ¿Completó loop? | ¿Guardó a disco? | ¿Commit? | ¿Colgó? ¿dónde? | Tiempo | PASA/FALLA | Notas |
|---|:---:|:---:|:---:|---|---|:---:|---|
| 1 | | | | | | | |
| 2 | | | | | | | |
| 3 | | | | | | | |
| 4 | | | | | | | |
| 5 | | | | | | | |
| 6 | | | | | | | |
| 7 | | | | | | | |
| 8 | | | | | | | |
| 9 | | | | | | | |
| 10 | | | | | | | |

**Tasa Claude-Agent = ____ / 10**

---

## Diagnóstico del modo de fallo (por qué se cuelga)

Cuando se cuelgue, mira `Ver → Salida → Continue` y clasifica. Es la diferencia entre "el modelo no puede" y "falta afinar config":

- [ ] **GLM emite tool-call malformado / no emite `stop`** → limitación del **MODELO** (tool-calling). Difícil de arreglar desde tu lado.
- [ ] **Timeout / streaming cortado** → problema de **CONFIG/API** (tokens, timeout). **Arreglable** → conclusión sería "falta afinar", no "GLM no sirve".
- [ ] **Schema de herramienta que Continue no reconoce** → **CONFIG** de Continue. Arreglable.
- [ ] Otro: __________

---

## Resultado y veredicto

- Tasa GLM-Agent: ____ / 10
- Tasa Claude-Agent (control): ____ / 10
- Modo de fallo dominante de GLM: __________

**Interpretación:**
- Si GLM < 9/10 **y** Claude ≥ 9/10 (mismo harness) → la afirmación se sostiene: **el modelo es la variable**.
- Si ambos < 9/10 → el problema es **Continue/config/API**, no GLM.
- Si GLM ≥ 9/10 → la afirmación NO se sostiene; era ruido de una corrida.

**Veredicto:** __________

---

## Observaciones ligadas (contexto de Fase 0)

- **M-coherencia (aparte de este eval):** GLM/Maker declaró "no hay contradicciones" con la deriva "horas" presente en `spec.md` (líneas 21/26) vs `ticket2-body.md` — falso negativo, reproducido 2/2. Registrar en el eval de coherencia semántica, no aquí.
- Este eval mide **solo fiabilidad del loop de Agent**, no corrección semántica. Son ejes distintos.
