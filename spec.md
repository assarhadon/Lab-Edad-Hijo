# Especificación: Calculadora de Edad de tu ser más amado

## Objetivo
App web mínima que calcula y muestra en tiempo real la edad de un niño/a desde su fecha y hora de nacimiento, con actualización continua de segundos y minutos.

---

## Entradas

| Campo | Tipo | Obligatorio | Restricciones |
|-------|------|-------------|---------------|
| Fecha de nacimiento | Date picker | Sí | Rango: 01/01/1900 hasta "hoy" (fecha actual). Fechas futuras bloqueadas. |
| Hora de nacimiento | Time picker (input type="time") | Sí | Formato 24 horas. Se interpreta siempre como hora de Bogotá, Colombia. |

---

## Salidas

**Formato del resultado:**
```
La edad de tu ser más amado es X años, X meses, X días, X minutos, X segundos
```

**Ejemplo:**
```
La edad de tu ser más amado es 5 años, 0 meses, 3 días, 8 minutos, 33 segundos
```

**Características:**
- Se muestran todos los valores, incluyendo ceros.
- Singular/plural correcto: "1 año" / "2 años", "1 mes" / "2 meses", etc.
- Actualización en tiempo real (reloj vivo mientras el usuario observa).
- Zona horaria de cálculo: Bogotá, Colombia (GMT-5).

---

## Reglas de negocio

1. **Cálculo automático:** El resultado aparece inmediatamente cuando el usuario ha ingresado fecha Y hora válidas. No hay botón "Calcular".

2. **Zona horaria fija:** La hora ingresada siempre se interpreta como hora de Bogotá, Colombia, independientemente de la zona horaria del navegador del usuario.

3. **Reloj en vivo:** Los minutos y segundos se actualizan en tiempo real mientras el usuario observa la pantalla.

4. **Reinicio automático:** Si el usuario cambia la fecha u hora, el reloj se reinicia con los nuevos valores.

5. **Estado inicial:** Al abrir la app, los campos están vacíos y no se muestra resultado.

6. **Limpieza:** Si el usuario borra la fecha u hora, el resultado desaparece y vuelve al estado inicial.

---

## Casos borde

| Caso | Comportamiento |
|------|----------------|
| Fecha futura | Bloqueada en el date picker (no se puede seleccionar). |
| Fecha muy antigua (más de 100 años) | Bloqueada por el rango mínimo: 01/01/1900. |
| Fecha ingresada sin hora | Se muestra mensaje: "Por favor ingresa la hora de nacimiento". |
| Hora ingresada sin fecha | No se muestra resultado. Se espera ambos campos. |
| Cambio de fecha/hora | El reloj se reinicia con los nuevos valores automáticamente. |
| Valores en cero | Se muestran (ej: "0 meses", "0 días"). |

---

## Fuera de alcance

- Responsive design (no requerido).
- Selección de zona horaria (siempre Bogotá).
- Fecha de referencia alternativa (siempre relativo a "ahora").
- Persistencia de datos (no se guarda nada en localStorage/cookies).
- Internacionalización (solo español Colombia).
- Tests automatizados.
- Frameworks, build tools, package.json.
- Cualquier cosa que no sea un único archivo `index.html` con Vanilla JS.

---

## Restricciones técnicas

- **Un único archivo:** `index.html` con HTML, CSS y JavaScript vanilla.
- **Sin dependencias externas:** No frameworks, no librerías, no CDNs.
- **Verificación:** Abrir `index.html` directamente en Chrome.
- **Idioma:** Español (Colombia).
- **Título:** "Calculadora de Edad de tu ser mas amado"
- **Diseño:** Minimalista con colores neutros.