

# Sistema Inteligente de Gestión de Leads — Proyecto Final

## 1. Descripción
Automatización de recepción y seguimiento de consultas comerciales de una empresa vinculada al sector de obras y construcción. Se utiliza n8n como orquestador, Notion como base de datos, OpenAI para clasificación y redacción asistida, y Gmail para aprobación humana y comunicación.

## 2. Objetivo
Registrar consultas, identificar clientes existentes, clasificar la prioridad de cada solicitud, generar una respuesta preliminar con IA y solicitar revisión humana antes del envío. Mantener la trazabilidad mediante estados en Notion.

## 3. Tecnologías
- **n8n Cloud:** Webhook, validaciones, bifurcaciones y coordinación del flujo.
- **Notion:** base de Clientes y base de Leads; conserva relaciones, prioridad, respuesta, fecha y estado.
- **OpenAI (gpt-4o-mini):** clasifica `ALTA`, `MEDIA`, `BAJA` y redacta una respuesta comercial.
- **Gmail:** correo de solicitud de aprobación y correo final de prueba.

## 4. Flujo principal
1. Webhook `POST /nuevo-lead` recibe nombre, email, empresa, tipo de cliente y consulta.
2. IF verifica que el correo contenga `@` (validación básica, no exhaustiva).
3. Notion busca al cliente por email; se utiliza el existente o se crea uno nuevo.
4. Se crea el lead y se relaciona con el cliente.
5. OpenAI produce una clasificación y una propuesta de respuesta en un objeto JSON.
6. Notion almacena prioridad, respuesta y estado **Pendiente de aprobación**.
7. Gmail envía una solicitud de revisión a la persona responsable y espera la respuesta.
8. IF distingue aprobación y rechazo:
   - **Aprobado:** Notion registra `Aprobado`, Gmail envía la respuesta al destinatario de prueba y Notion registra `Enviado` si el envío termina correctamente.
   - **Rechazado:** Notion registra `Rechazado`; no se envía la respuesta final.

## 5. Variables dinámicas
Se obtienen del cuerpo del Webhook los datos del cliente y su consulta. La respuesta de OpenAI se utiliza para completar las propiedades de Notion. Los nodos posteriores reutilizan el ID creado para el lead, evitando actualizar otro registro.

## 6. Control humano (HITL)
La aprobación vía Gmail es una condición previa al correo de respuesta. No se debe enviar la respuesta al cliente cuando se selecciona rechazo. Para la demostración, el nodo de envío final utiliza una casilla de prueba fija; su sustitución por el email del cliente está pendiente antes de cualquier puesta en producción.

## 7. Gestión de errores y límites observados
- El flujo incluye una bifurcación inicial para rechazar correos sin `@`, pero en el JSON exportado esa salida no tiene tratamiento posterior conectado.
- Existe un circuito auxiliar de espera, relectura de Notion, condicionales y conteo de intentos; **no está conectado al flujo principal**, por lo que no debe presentarse como manejo de errores plenamente operativo.
- El contador de intentos se calcula con un campo del ítem, no hay confirmación de persistencia entre ejecuciones.
- La validación de email es superficial. No debe afirmarse que garantiza un correo válido.
- El flujo exportado aparece **inactivo**. Para uso real es necesario habilitarlo y confirmar credenciales y URL de producción.
- El export original contiene `pinData` con datos de prueba; se recomienda excluir datos personales de evidencias públicas y del repositorio.

## 8. Evidencias y pruebas
Consultar `PRUEBAS_Y_EVIDENCIAS.md`. No declarar resultados de pruebas no realizadas. Evidencias observadas hasta el momento: generación de respuesta y actualización en Notion, respuesta de aprobación `approved: true` y envío de correo de prueba. Aún falta documentar pruebas completas de ambas ramas y manejo de errores.

## 9. Seguridad
No compartir claves API, tokens, credenciales, cabeceras del webhook ni datos reales de clientes. Utilizar ejemplos ficticios y anonimizar capturas. Evitar publicar un workflow exportado con datos de ejecución fijados (`pinData`).

## 10. Archivos de la entrega
- `Arquitectura_Sistema_Inteligente_Leads.pdf`: diagrama del sistema.
- `Sistema_Inteligente_Leads_Revision_Final.json`: export para revisión/importación, sujeto a validación en n8n.
- `PRUEBAS_Y_EVIDENCIAS.md`: matriz de pruebas y registro de capturas.
- `evidencias/`: capturas seleccionadas, sin información sensible.
- `video_demo`: enlace al video de hasta 3 minutos (incorporar enlace una vez grabado).

## 11. Enlaces para completar
- Repositorio GitHub: **[agregar enlace]**
- Base Notion, acceso de solo lectura: **[https://young-carnation-60e.notion.site/3f74757243ab801d8e7fd689ffe3c8b0?v=3f74757243ab80c4923a000c30c2c5a6]**
- Video demostrativo: **[agregar enlace]**

## 12. Conclusión
El prototipo evidencia la integración de cuatro servicios, el enriquecimiento comercial con IA y un punto de revisión humana. Quedan pendientes la validación integral de las ramas y el endurecimiento del manejo de errores antes de su uso productivo.
