# ADR-001: Adoptar un monolito modular para el Sistema de Vacantes y Prospectos

## Estado

Aceptada

## Contexto

El sistema registra vacantes, prospectos, postulaciones y documentos para el área de Recursos Humanos de una empresa. Finanzas/Contaduría y Corporativo lo consultan, pero cada área debe ver solo la información que le corresponde (RF-009, RF-010, CU-06). Todo cambio queda registrado en una bitácora (RNF-06).

Condiciones reales del proyecto que influyen en la decisión:

- **Un solo cliente.** El sistema es software instalado en la infraestructura de la empresa, no un servicio para varias organizaciones, así que no hay multi-tenancy ni necesidad de escalar por cliente.
- **Carga baja y predecible.** Los usuarios son el personal de RH y unas pocas personas de otras áreas. No hay picos de tráfico que justifiquen escalar partes del sistema por separado.
- **Equipo pequeño.** El sistema lo construye una sola persona como proyecto escolar, con tiempo limitado y sin equipo de operaciones.
- **Reglas de negocio muy relacionadas.** Prospectos, vacantes, postulaciones y bitácora comparten datos y deben mantenerse consistentes. Por ejemplo, cambiar el estado de una postulación debe quedar en la bitácora en la misma operación.
- **Operación sencilla.** La empresa debe poder instalarlo y mantenerlo sin infraestructura especializada.

Alternativas consideradas:

1. **Monolito clásico:** una sola aplicación sin fronteras internas definidas.
2. **Monolito modular:** una sola aplicación desplegable, dividida internamente en módulos con responsabilidades y dependencias explícitas.
3. **Microservicios:** un servicio desplegable por cada módulo (prospectos, vacantes, documentos, bitácora, notificaciones).

## Decisión

Se construirá el sistema como un **monolito modular**: una sola aplicación de negocio que se despliega como una unidad, con módulos internos separados (Usuarios y Roles, Visibilidad por rol, Prospectos, Vacantes, Postulaciones, Documentos, Bitácora, Notificaciones y Persistencia).

Reglas para mantener la modularidad:

- Cada módulo expone una interfaz y no accede directamente a las tablas de otro módulo.
- Las notificaciones y la bitácora reaccionan a eventos publicados por los demás módulos (patrón Observer), no son llamadas desde ellos.
- El envío de correo se aísla detrás de un adaptador (patrón Adapter), de modo que cambiar de proveedor solo afecta a ese módulo.
- Los contenedores desplegados son cuatro: aplicación web, aplicación de negocio, base de datos y almacén de documentos (ver `diagramas.md`).

## Consecuencias

**Positivas**

- Un solo despliegue y una sola base de datos: instalarlo y respaldarlo es sencillo para el cliente.
- Las operaciones que tocan varios módulos (por ejemplo, cambiar una postulación y registrarlo en la bitácora) pueden ejecutarse en una sola transacción, lo que protege la consistencia de la información.
- La estructura en módulos permite trabajar y probar cada parte por separado, y deja abierta la posibilidad de extraer un módulo como servicio más adelante si hiciera falta.
- Menor costo de desarrollo y de operación que con microservicios, adecuado para un equipo de una persona.

**Negativas / riesgos**

- Todo el sistema se despliega y escala junto: si un módulo (por ejemplo, Documentos) creciera mucho, no se podría escalar aparte sin cambios.
- Una falla grave en un módulo puede afectar a toda la aplicación.
- La separación entre módulos depende de disciplina del equipo; si no se respetan las interfaces, el sistema puede degradarse hacia un monolito clásico.
- Quedamos atados a un solo stack tecnológico para toda la aplicación de negocio.

**Por qué no las otras opciones**

- *Monolito clásico:* sería igual de fácil de desplegar, pero sin fronteras entre módulos sería más difícil de mantener, probar y explicar a quien lea la documentación.
- *Microservicios:* resuelven problemas de escala y de equipos grandes que este sistema no tiene, y a cambio añaden complejidad de red, despliegue y consistencia de datos que un cliente único con un equipo de una persona no puede sostener.

**Cuándo revisar esta decisión:** si el sistema pasa a ofrecerse como servicio para varias empresas (multi-tenancy), o si la carga de un módulo específico crece lo suficiente para necesitar escalarlo de forma independiente.
