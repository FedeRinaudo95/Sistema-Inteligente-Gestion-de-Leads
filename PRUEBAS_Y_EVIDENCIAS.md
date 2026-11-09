# Registro de pruebas y evidencias

> Completar **Resultado real**, **Fecha**, **Evidencia** y **Observaciones** después de cada prueba. No marcar como exitosa una prueba no ejecutada.

| Caso | Entrada / acción | Resultado esperado | Resultado real | Evidencia |
|---|---|---|---|---|
| P01 — Cliente nuevo | Email de ejemplo nuevo y consulta comercial | Crear cliente y lead en Notion | Pendiente de documentar | `evidencias/P01.png` |
| P02 — Cliente existente | Reutilizar email registrado | Crear lead sin duplicar cliente | Pendiente de documentar | `evidencias/P02.png` |
| P03 — Aprobación | Aprobar desde el correo de revisión | Estado Aprobado → envío → Enviado | Pendiente de validar de extremo a extremo | `evidencias/P03.png` |
| P04 — Rechazo | Rechazar desde el correo de revisión | Estado Rechazado y ningún correo final | Pendiente de validar | `evidencias/P04.png` |
| P05 — Datos incompletos | Email sin `@` | Desviar a rama inválida, sin crear lead | Pendiente de validar; rama de error sin destino | `evidencias/P05.png` |
| P06 — Falla externa (opcional) | Simular falla controlada en copia de prueba | Registrar el error y evitar envíos incorrectos | No implementado integralmente en flujo principal | `evidencias/P06.png` |

## Evidencia existente observada en capturas del desarrollo
- OpenAI devolvió prioridad `MEDIA` y una propuesta de respuesta para una consulta ficticia.
- El registro de Notion mostró el estado `Pendiente de aprobación` y la respuesta guardada.
- Gmail devolvió `approved: true` en una ejecución anterior.
- Una prueba del nodo `Send a message` devolvió datos del mensaje enviado.

Estos resultados **no prueban** automáticamente el cierre completo de ambas ramas.

## Capturas recomendadas
1. Vista general del flujo (sin URLs sensibles).
2. Entrada Webhook anonimizada y validación.
3. Salida JSON de OpenAI.
4. Registro del lead en Notion con prioridad y respuesta.
5. Correo de aprobación / respuesta de aprobación, ocultando direcciones privadas.
6. Rama TRUE → estado Enviado.
7. Rama FALSE → estado Rechazado.
8. Ejecución fallida o validación de correo incompleto, con descripción honesta del comportamiento.

## Guion para video (3 minutos)
- **0:00–0:25** Objetivo y las cuatro herramientas.
- **0:25–1:05** Vista del flujo: Webhook, validación y Notion.
- **1:05–1:45** OpenAI genera prioridad y respuesta; mostrar Notion.
- **1:45–2:25** Gmail solicita aprobación; explicar True/False.
- **2:25–3:00** Resultado documentado, limitaciones y conclusión. No mostrar credenciales ni datos personales reales.
