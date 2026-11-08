---
name: Reporte de Incidencia
about: Utilice esta plantilla para registrar interrupciones en producción.
title: '[INCIDENTE] <Título corto del problema>'
labels: 'status: triage'
assignees: ''
---

## 1. Información General

* **Servicio / Módulo Afectado:** API de Pasarela de Pagos
* **Entorno:** Producción
* **Hora de Detección:** 2026-10-28 10:00 UTC
* **Severidad Estimada:** P1 - Crítico

## 2. Descripción del Síntoma

Error HTTP 504 Gateway Timeout masivo al intentar procesar transacciones.

## 3. Evidencias Técnicas y Logs

```text
[2026-10-28T10:02:15Z] [ERROR] [DBConnectionPool] TimeoutException: Unable to acquire connection from pool after 30000ms. Active connections: 50/50.
