# ADR-001: Adoptar un monolito modular para el Sistema de Vacantes y Prospectos

## Estado

Aceptada

## Contexto

El sistema registra vacantes, prospectos, postulaciones y documentos para el área de Recursos Humanos de una empresa. Finanzas/Contaduría y Corporativo lo consultan, pero cada área debe ver solo la información que le corresponde (RF-009, RF-010, CU-06). Todo cambio queda registrado en una bitácora (RNF-06).

Condiciones reales del proyecto que influyen en la decisión:

- **Un solo cliente.** El sistema es software instalado en la infraestructura de una sola empresa.
- **Carga baja y predecible.** Los usuarios son el personal de RH y unas pocas personas de otras áreas. No hay picos de tráfico que justifiquen escalar partes del sistema por separado.
- **Reglas de negocio muy relacionadas.** Prospectos, vacantes, postulaciones y bitácora comparten datos y deben mantenerse consistentes. Por ejemplo, cambiar el estado de una postulación debe quedar en la bitácora en la misma operación.

## Decisión

Se construirá como un **monolito modular**: una sola aplicación de negocio que se despliega como una unidad, con módulos internos separados (Usuarios y Roles, Visibilidad por rol, Prospectos, Vacantes, Postulaciones, Documentos, Bitácora, Notificaciones y Persistencia).

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
