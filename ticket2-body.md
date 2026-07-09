## Qué construir

Implementar la lógica de cálculo de edad en tiempo real basándose en la fecha y hora de nacimiento ingresadas, comparándolas con la fecha/hora actual en zona Bogotá (GMT-5). Incluir un reloj en vivo (ticker) que actualice los valores cada segundo. La lógica debe ser determinista y funcional: funciones puras separadas del DOM que reciban parámetros y devuelvan objetos con los valores calculados.

## Criterios de aceptación

- [ ] Función pura `calcularEdad(fechaNacimiento, horaNacimiento)` que retorne objeto `{ años, meses, días, horas, minutos, segundos }`
- [ ] Función pura `formatearEdad(edadObj)` que genere el string con singular/plural correcto ("1 año" / "2 años", etc.)
- [ ] Función `obtenerFechaHoraBogota()` que retorne la fecha/hora actual en GMT-5
- [ ] Ticker implementado con `setInterval` que actualice cada segundo
- [ ] Formato de salida: "La edad de tu ser más amado es X años, X meses, X días, X horas, X minutos, X segundos"
- [ ] Singular/plural correcto para todos los valores (año/años, mes/meses, día/días, hora/horas, minuto/minutos, segundo/segundos)
- [ ] Valores en cero se muestran (ej: "0 meses", "0 días")
- [ ] Si fecha u hora están vacíos, no mostrar resultado (estado limpio)
- [ ] Zona horaria GMT-5 (Bogotá) aplicada correctamente en el cálculo
- [ ] Código separado: lógica de cálculo independiente del manejo del DOM

## Casos borde a manejar

- [ ] Campos vacíos: no mostrar resultado
- [ ] Solo fecha sin hora: mostrar mensaje "Por favor ingresa la hora de nacimiento"
- [ ] Solo hora sin fecha: no mostrar resultado

## Bloqueado por

Issue #1 ✅ (Completado - estructura base implementada)