# Post-Mortem: Incidente en la Pasarela de Pagos PayFlow

**Fecha del incidente:** 28 de octubre de 2026  
**Servicio afectado:** PayFlow - API de Pagos  
**Severidad:** P1 - Crítica  
**Issue relacionado:** #1  
**Estado:** Resuelto

## 1. Resumen del incidente

El 28 de octubre de 2026, la API de pagos PayFlow presentó errores HTTP 504 Gateway Timeout, impidiendo que los usuarios completaran sus transacciones.

El problema se originó por el agotamiento del pool de conexiones de la base de datos. Las conexiones utilizadas no se liberaban correctamente después de procesar las operaciones.

## 2. Impacto del incidente

De acuerdo con los datos simulados del escenario del laboratorio:

- **Tiempo total de interrupción:** 35 minutos.
- **MTTD (tiempo medio de detección):** 2 minutos.
- **MTTR (tiempo medio de recuperación):** 33 minutos.
- **Transacciones afectadas:** 450.
- **Impacto económico estimado:** USD 18,500.

Estos valores corresponden al escenario académico y no a mediciones de un servicio real.

## 3. Línea de tiempo

| Hora (UTC) | Evento |
|---|---|
| 10:00 | Se detecta el incidente en la API de pagos. |
| 10:02 | Se identifican errores de agotamiento de conexiones. |
| 10:10 | Se inicia la investigación técnica. |
| 10:20 | Se identifica que las conexiones no se liberan correctamente. |
| 10:30 | Se implementa el hotfix para liberar conexiones. |
| 10:35 | Se verifica la recuperación del servicio. |

## 4. Análisis de causa raíz: Los 5 porqués

**Problema:** La API de pagos devuelve errores HTTP 504.

1. ¿Por qué ocurre el error HTTP 504?  
   Porque la API no consigue obtener conexiones disponibles de la base de datos.

2. ¿Por qué no hay conexiones disponibles?  
   Porque todas las conexiones del pool están ocupadas.

3. ¿Por qué las conexiones permanecen ocupadas?  
   Porque no se liberan después de procesar las operaciones.

4. ¿Por qué no se liberan correctamente?  
   Porque el código no implementaba un mecanismo adecuado de liberación de recursos.

5. ¿Por qué no se detectó antes?  
   Porque las pruebas y revisiones no contemplaban adecuadamente el agotamiento del pool de conexiones.

**Causa raíz:** Una gestión inadecuada del ciclo de vida de las conexiones a la base de datos, junto con pruebas insuficientes para detectar el agotamiento de recursos.

## 5. Acciones correctivas aplicadas

- Se modificó la lógica de procesamiento para liberar las conexiones mediante un bloque `finally`.
- Se ajustó la capacidad del pool de conexiones.
- Se creó una rama hotfix para corregir el problema.
- Se realizó una revisión mediante Pull Request antes de integrar los cambios a `main`.

## 6. Acciones preventivas

- Implementar pruebas para verificar la liberación de conexiones.
- Monitorear el uso y la disponibilidad del pool de conexiones.
- Configurar alertas cuando se alcance un nivel crítico de conexiones.
- Realizar revisiones de código enfocadas en la gestión de recursos.
- Ejecutar pruebas de carga para detectar posibles problemas antes de producción.

## 7. Lecciones aprendidas

El incidente demuestra la importancia de gestionar correctamente los recursos de la base de datos y contar con mecanismos de monitoreo y pruebas.

También se evidencia la utilidad del flujo de trabajo con Git y GitHub para gestionar incidencias, implementar hotfixes y revisar los cambios antes de integrarlos.

## 8. Conclusión

El incidente de PayFlow fue atendido mediante la identificación de la causa raíz y la implementación de una corrección en el manejo de conexiones.

Las acciones preventivas propuestas permitirán reducir el riesgo de que vuelva a ocurrir un problema similar.