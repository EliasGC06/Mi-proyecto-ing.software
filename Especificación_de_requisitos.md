# Especificación de requisitos

Sistema: Base de Datos de vacantes y prospectos R.H.

Autor: Elias García Cisneros

Versión: 0.1 

Fecha de la última actualización: 30/09/2026

## 1. Propósito y alcance

Enlace Figma: 

Propósito del documento: En este documento se especifica lo que se hace y con que calidad debe tener el sistema que desarrollaremos para recopilar y concentrar la información de las vacantes, prospectos y personal activo de la empresa. Esté nace en base a la visión del producto que hicimos y la entrevista realizada.

Alcance del sistema: Registrar prospectos según su vacante, darles un estatus, verificar sus datos, filtrar la información, adjuntar y consultar sus documentos, ver el historial profesional del prospecto, restringir la información sensible a las otras áreas y mostrar con prioridad las vacantes urgentes.   

Fuera del alcance:

| Fuera | Por qué |
|---|---|
| Comparar prospectos automáticamente. | No hay criterios de evaluación definidos |
| Reportes financieros o de nómina. | Este tema lo manejaría Contaduría/Finanzas especificamente, confirmado en la entrevista (pregunta 13) |
| Notificaciones automáticas por correo o SMS. | Requiere un desarrollo más extenso no contemplado para esta versión. | 

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema | 
|---|---|---|
| Recursos Humanos | Gestiona de 5 a 10 vacantes activas a la vez, de distintas empresas, horarios, zonas y etapas. Buscando archivo por archivo lo que necesita y hablando temas con otras áreas en persona, perdiendo tiempo y esfuerzo. | Registrar, asignar, dar seguimiento y encontrar el historial de un prospecto en un solo lugar. |
| Finanzas/Contaduría | Pregunta que vacantes ya están ocupadas y por quien, en lo cual recopila sus datos o documentos (Numero de cuenta, banco, adeudos importantes como INFONAVIT O FONACOT), también si un empleado renuncia, para cerrar su nomina y verificar que le deben dar de finiquito o no. | Consultar el estatus de las vacantes en tiempo real en un solo lugar. |
| Corporativo | Supervisa, coordina y comunica con R.H sobre las vacantes, prospectos y empleados que tiene la empresa, preguntando en persona sobre su estatus de cada uno; quitando mucho tiempo y esfuerzo. | Supervisar el estatus de todo el personal en una sola plataforma, viendo con claridad y transparencia lo que su cede con cada uno sin tener que preguntar al área de R.H. |

**Conflictos identificados entre usuarios:**
Finanzas/Contaduría y Corporativo quieren visibilidad amplia, pero los datos sensibles (problemas médicos, comentarios internos, temas familiares) solo los puede ver R.H, como se confirmo en la entrevista, la visibilidad por área, las demás áreas que no son R.H ven únicamente estatus, puesto y urgencia. 

## 3. Requisitos funcionales

### 3.1 
| ID | Nombre | Prioridad | Origen |
|---|---|---|---|
| RF-001 | Registrar prospecto | Alta | Supuesto (Visión, alcance 1) |
| RF-002 | Registrar vacante | Alta | Supuesto (Visión, alcance 2) + Confirmado (P5) |
| RF-003 | Mantener estatus del prospecto | Alta | Supuesto (Visión, alcance 3) + Confirmado (P12) |
| RF-004 | Mantener estatus de la vacante | Alta | Supuesto (Visión, alcance 3) |
| RF-005 | Archivar prospecto que no avanza | Alta | Confirmado (P10, P12) |
| RF-006 | Reutilizar el registro de un prospecto que vuelve a aplicar | Alta | Confirmado (P10) + Supuesto (mecanismo) |
| RF-007 | Validar campos obligatorios | Alta | Supuesto (Visión, alcance 4) |
| RF-008 | Validar formato de teléfono y correo | Media | Supuesto (Visión, alcance 4) |
| RF-009 | Impedir que un prospecto esté activo en dos vacantes | Alta | Confirmado (P11) |
| RF-010 | Filtrar vacantes | Media | Supuesto (Visión, alcance 5) |
| RF-011 | Filtrar prospectos | Media | Supuesto (Visión, alcance 5) |
| RF-012 | Adjuntar documentos a un prospecto | Media | Supuesto (Visión, alcance 6) |
| RF-013 | Consultar el historial de un prospecto en una sola pantalla | Alta | Confirmado (P7, P9) |
| RF-014 | Limitar lo que ven Finanzas, Contaduría y Corporativo | Alta | Supuesto (Visión, resolución del conflicto) |
| RF-015 | Reservar los datos sensibles a RH | Alta | Supuesto (Visión, regla 2) |
| RF-016 | Mostrar vacantes urgentes con prioridad | Media (pendiente de validar) | Supuesto (no confirmado en la entrevista) |
| RF-017 | Reabrir una vacante ocupada | Alta | Confirmado (ficha de dominio; P4, P8) |
| RF-018 | Señalar las vacantes reabiertas a Finanzas y Contaduría | Alta | Confirmado (P4, P8) + Supuesto (mecanismo) |
| RF-019 | Autenticar usuarios con un rol | Alta | Derivado de RF-014 y RF-015 |

### 3.2 Fichas 

#### RF-001 - Registrar prospecto
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir a RH registrar un prospecto con nombre, edad, puesto, empresa, horario, localidad, escolaridad, teléfono y correo. |
| Origen | Supuesto (Visión, alcance 1) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario RH autenticado, cuando captura los nueve campos y guarda, entonces el prospecto aparece en el listado con estatus "prospecto". |
| Relacionado con | CU-01 · RF-007, RF-008 |
 
#### RF-002 - Registrar vacante
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir a RH registrar una vacante con puesto, área, horario, localidad, nivel de urgencia (normal o urgente) y motivo de apertura. |
| Origen | Supuesto (Visión, alcance 2). El campo "motivo de apertura" y que RH decide la urgencia según puesto, tareas y forma en que se desocupó salen de P5 (Confirmado). |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario RH, cuando captura los seis campos y guarda, entonces la vacante aparece en el listado con estatus "disponible" y su nivel de urgencia. |
| Relacionado con | CU-02 · RF-004, RF-007 |
 
#### RF-003 - Mantener estatus del prospecto
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe asignar a cada prospecto exactamente un estatus: prospecto, en proceso, contratado o archivado. |
| Origen | Supuesto (Visión, alcance 3); el estatus "archivado" es Confirmado (P12) |
| Prioridad | Alta |
| Criterio de aceptación | Todo prospecto guardado tiene un estatus de los cuatro valores; el sistema no permite guardar un prospecto sin estatus ni con dos a la vez. |
| Relacionado con | CU-01, CU-03, CU-08 |
 
#### RF-004 - Mantener estatus de la vacante
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe asignar a cada vacante exactamente un estatus: disponible u ocupada. |
| Origen | Supuesto (Visión, alcance 3) |
| Prioridad | Alta |
| Criterio de aceptación | Toda vacante guardada tiene uno de los dos estatus; al cambiarlo, el nuevo valor se ve en el listado de inmediato. |
| Relacionado con | CU-02, CU-03, CU-07 |
 
#### RF-005 - Archivar prospecto que no avanza
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe cambiar a estatus "archivado" a un prospecto que no avanza en el proceso, sin eliminar su información. |
| Origen | Confirmado (P10, P12) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto "en proceso" que RH descarta, cuando RH confirma, entonces su estatus es "archivado" y todos sus datos y documentos siguen consultables desde su historial. |
| Relacionado con | CU-08 · RF-003, RF-013 |
 
#### RF-006 - Reutilizar el registro de un prospecto que vuelve a aplicar
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe actualizar el registro existente de un prospecto archivado cuando vuelve a aplicar, en lugar de crear uno nuevo. |
| Origen | Confirmado el comportamiento (P10). Supuesto el mecanismo de detección (por correo o teléfono). |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto archivado con correo X, cuando RH registra a una persona con correo X, entonces el sistema ofrece reactivar el registro existente, conserva su historial y no crea un segundo registro. |
| Relacionado con | CU-01 · RF-005 |
 
#### RF-007 - Validar campos obligatorios
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe impedir guardar un registro de prospecto o vacante que tenga algún campo obligatorio vacío. |
| Origen | Supuesto (Visión, alcance 4) |
| Prioridad | Alta |
| Criterio de aceptación | Cuando RH guarda con al menos un campo obligatorio vacío, el sistema no guarda y marca cada campo faltante. |
| Relacionado con | CU-01, CU-02 |
 
#### RF-008 - Validar formato de teléfono y correo
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe rechazar teléfonos que no tengan 10 dígitos y correos sin formato válido. |
| Origen | Supuesto (Visión, alcance 4) |
| Prioridad | Media |
| Criterio de aceptación | Cuando RH captura "55123" como teléfono o "juan@" como correo, el sistema no guarda e indica el formato esperado. |
| Relacionado con | CU-01 |
 
#### RF-009 - Impedir que un prospecto esté activo en dos vacantes
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe rechazar la asignación de un prospecto que ya esté activo en otra vacante. |
| Origen | Confirmado (P11) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto "en proceso" en la vacante A, cuando RH intenta asignarlo a la vacante B, el sistema rechaza la asignación, indica que está activo en la vacante A y el prospecto sigue ligado solo a A. |
| Relacionado con | CU-03 · RF-003 |
 
#### RF-010 - Filtrar vacantes
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe filtrar el listado de vacantes por puesto, urgencia, localidad y estatus. |
| Origen | Supuesto (Visión, alcance 5) |
| Prioridad | Media |
| Criterio de aceptación | Con uno o más filtros aplicados, el listado muestra solo las vacantes que cumplen todos. |
| Relacionado con | CU-05 |
 
#### RF-011 - Filtrar prospectos
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe filtrar el listado de prospectos por puesto, urgencia de la vacante, localidad y estatus. |
| Origen | Supuesto (Visión, alcance 5) |
| Prioridad | Media |
| Criterio de aceptación | Con uno o más filtros aplicados, el listado muestra solo los prospectos que cumplen todos. |
| Relacionado con | CU-05, CU-03 |
 
#### RF-012 - Adjuntar documentos a un prospecto
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir a RH adjuntar documentos PDF, JPG o PNG de hasta 10 MB a un prospecto. |
| Origen | Supuesto (Visión, alcance 6). El formato y el tamaño son propuesta. |
| Prioridad | Media |
| Criterio de aceptación | Cuando RH adjunta un PDF de 2 MB, este aparece en los documentos del prospecto con nombre y fecha; un archivo de 15 MB es rechazado con mensaje. |
| Relacionado con | CU-01, CU-04 |
 
#### RF-013 - Consultar el historial de un prospecto en una sola pantalla
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar en una sola pantalla los datos, documentos y vacantes anteriores de un prospecto, incluso si aplicó hace meses. |
| Origen | Confirmado (P7, P9: es el mayor dolor actual) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto que aplicó hace seis meses, cuando RH lo busca por nombre y abre su registro, ve en una sola pantalla sus datos, sus documentos y las vacantes a las que aplicó, con fechas. |
| Relacionado con | CU-04 · RF-005, RF-006 |
 
#### RF-014 - Limitar lo que ven Finanzas, Contaduría y Corporativo
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar a los usuarios de Finanzas, Contaduría y Corporativo únicamente estatus, puesto y urgencia. |
| Origen | Supuesto (Visión, resolución del conflicto; regla 2 de la ficha no verificada en entrevista) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario de Finanzas/Contaduría o Corporativo, cuando abre una vacante o un prospecto, ve solo estatus, puesto y urgencia; no ve documentos ni datos sensibles. |
| Relacionado con | CU-06 · RF-015, RF-019 |
 
#### RF-015 - Reservar los datos sensibles a RH
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir ver y editar los campos "problemas médicos" y "comentarios internos" solo a usuarios RH. |
| Origen | Supuesto (Visión, regla 2; no verificado en entrevista) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario sin rol RH, cuando intenta abrir o editar esos campos, el sistema no los muestra y rechaza la edición. |
| Relacionado con | CU-01, CU-04 · RF-014, RF-019 |
 
#### RF-016 - Mostrar vacantes urgentes con prioridad
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar las vacantes urgentes antes que las demás y con indicador visual, para los tres roles. |
| Origen | Supuesto. La entrevista no lo confirmó (P4: las áreas interactúan "solo cuando es necesario"). |
| Prioridad | Media (pendiente de validar) |
| Criterio de aceptación | En el listado de vacantes de cualquier rol, las vacantes urgentes aparecen en las primeras posiciones con la etiqueta "Urgente". |
| Relacionado con | CU-05, CU-06 |
 
#### RF-017 - Reabrir una vacante ocupada
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir a RH cambiar una vacante ocupada a disponible, registrando el motivo. |
| Origen | Confirmado (ficha de dominio, excepción; P4, P8). No estaba en la Visión. |
| Prioridad | Alta |
| Criterio de aceptación | Dada una vacante ocupada, cuando RH la reabre e indica el motivo, su estatus pasa a "disponible", conserva su nivel de urgencia y queda registrado usuario, fecha y motivo. |
| Relacionado con | CU-07 · RF-004, RF-018 |
 
#### RF-018 - Señalar las vacantes reabiertas a Finanzas y Contaduría
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar a Finanzas y Contaduría, al iniciar sesión, las vacantes reabiertas en las últimas 24 horas. |
| Origen | La necesidad de avisar con urgencia a Finanzas es Confirmada (P4, P8). El mecanismo es Supuesto, porque las notificaciones automáticas están fuera del alcance. |
| Prioridad | Alta |
| Criterio de aceptación | Dada una vacante reabierta hoy, cuando un usuario de Finanzas/Contaduría abre su pantalla de inicio, la ve en "Reabiertas recientes" con fecha y hora de reapertura. |
| Relacionado con | CU-07 · RF-017 |
 
#### RF-019 - Autenticar usuarios con un rol
 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir el acceso solo a usuarios con credenciales válidas y un único rol: RH, Finanzas/Contaduría o Corporativo. |
| Origen | Derivado de RF-014 y RF-015 |
| Prioridad | Alta |
| Criterio de aceptación | Cuando alguien intenta entrar con credenciales inválidas, no ve ningún dato del sistema. |
| Relacionado con | Transversal a todos los CU · RF-014, RF-015 |


## 4. Requisitos no funcionales

### 4.1 Resumen

|ID|Atributo de calidad|Descripción Métrica|Origen|Prioridad|Por qué importa|Afecta a|
|---|---|---|---|---|---|---|
|RNF-01|Confidencialidad|El 100% de los intentos de un usuario sin rol RH por ver o editar datos sensibles (problemas médicos, comentarios internos) debe ser rechazado. Se verifica con pruebas de los tres roles.|Visión (atributo de confidencialidad)|Alta|Son datos de salud y comentarios internos; una filtración daña a la persona y genera riesgo legal, por eso no admite excepciones.|RF-14, RF-15, RF-19 · CU-01, CU-04, CU-06|
|RNF-02|Confidencialidad|La sesión debe cerrarse tras 15 minutos de inactividad y las contraseñas deben tener al menos 10 caracteres.|Supuesto (propuesta a validar)|Media|En una oficina se comparten equipos y se deja la pantalla desatendida; 15 minutos equilibra protección y continuidad del trabajo.|RF-19 · Todos los CU|
|RNF-03|Integridad|Un cambio de estatus de una vacante o prospecto debe ser visible para los tres roles en 5 segundos o menos.|Entrevista (P8 y ficha de dominio: respuestas desactualizadas a Contaduría); Visión (integridad)|Alta|Hoy Contaduría recibe respuestas desactualizadas porque la información no se sincroniza entre áreas; con 5 segundos el problema desaparece en la práctica.|RF-04, RF-17, RF-18 · CU-03, CU-06, CU-07|
|RNF-04|Integridad|Cero registros de prospecto eliminados físicamente: toda baja se hace por archivado y se conservan sus datos y documentos.|Confirmado (P10, P12)|Alta|La información de un prospecto nunca se borra, porque puede volver a aplicar y RH necesita su historial.|RF-05, RF-06 · CU-01, CU-08|
|RNF-05|Usabilidad|Una persona de RH que no haya usado el sistema debe registrar un prospecto completo en 3 minutos o menos, sin capacitación. Se prueba con 3 personas.|Visión (usabilidad)|Alta|Si el sistema es más lento que Excel o WhatsApp, RH regresa al método anterior y el sistema fracasa.|RF-01, RF-07, RF-08 · CU-01|
|RNF-06|Usabilidad|Encontrar el historial de un prospecto debe tomar 30 segundos o menos y 3 clics como máximo.|Confirmado (P7, P9)|Alta|Buscar archivo por archivo es el mayor dolor reportado por RH y lo que más tiempo le quita.|RF-13 · CU-04|
|RNF-07|Rendimiento|Búsquedas y filtros deben responder en 2 segundos o menos con hasta 5,000 prospectos registrados.|Supuesto (volumen a validar)|Media|Con 5 a 10 vacantes activas el histórico crece con los años; 2 segundos se percibe como respuesta inmediata.|RF-10, RF-11 · CU-05|
|RNF-08|Disponibilidad|El sistema debe estar disponible el 99% del horario laboral (lunes a viernes, de 8:00 a 18:00).|Supuesto (horario a validar)|Baja|RH lo usa en horario de oficina; fuera de él, una caída no bloquea el proceso de contratación.|Todos los RF · Todos los CU|
|RNF-09|Trazabilidad|El 100% de los cambios de estatus y de datos sensibles debe registrar usuario, fecha y hora.|Visión (Corporativo teme falta de transparencia)|Media|Permite saber quién cambió qué y cuándo, y evita incongruencias entre áreas.|RF-03, RF-04, RF-15, RF-17 · CU-03, CU-07, CU-08|


## 5. Casos de uso

Se trabajan en la semana 7, después de la entrevista. Cada caso de uso se relaciona con los requisitos funcionales que realiza.

|Casos de uso| Descripción |
|---|---|
| CU-01 | Registrar prospecto |
| CU-02 | Registrar vacante |
| CU-03 | Asignar prospecto a vacante |
| CU-04 | Consultar historial de un prospecto |
| CU-05 | Filtar vacantes y prospectso |
| CU-06 | Consultar estatus de vacantes |
| CU-07 | Reabrir vacante ocupada |
| CU-08 | Archivar prospectos |


## 6. Trazabilidad

Esta tabla es la que hace posible el análisis de impacto de la semana 15. Mantenla actualizada conforme cambien los requisitos.

| Requisito | Origen | Caso de uso | Elemento del prototipo |
|---|---|---|---|
| RF-01 | Visión | CU-01 | — |
| RF-02 | Visión, P5 | CU-02 | — |
| RF-03 | Visión, P12 | CU-01, CU-03, CU-08 | P4, P5 |
| RF-04 | Visión | CU-02, CU-03, CU-07 | P1, P2, P5 |
| RF-05 | P10, P12 | CU-08 | — |
| RF-06 | P10 | CU-01 | — |
| RF-07, RF-08 | Visión | CU-01, CU-02 | — |
| RF-09 | P11 | CU-03 | P6 (flujo alterno) |
| RF-10 | Visión | CU-05 | P1 |
| RF-11 | Visión | CU-05, CU-03 | P3 |
| RF-12 | Visión | CU-01, CU-04 | — |
| RF-13 | P7, P9 | CU-04 | — |
| RF-14 | Visión | CU-06 | P7 |
| RF-15 | Visión | CU-01, CU-04 | P7 |
| RF-16 | Visión | CU-05, CU-06 | P1 |
| RF-17 | Ficha, P4, P8 | CU-07 | — |
| RF-18 | P4, P8 | CU-07 | — |
| RF-19 | Derivado | Transversal | — |


## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
|---|---|---|---|
| 0.1 | 30/09/2026 | Primera versión, construida desde la Visión y la entrevista |
| 0.1 | 30/09/2026 | Se agregan RF-05, RF-06, RF-13, RF-17 y RF-18, surgidos de la entrevista (P7, P9, P10, P12 y excepción de la ficha) |
| 0.1 | 30/09/2026 | La Visión permitía a RH "eliminar" datos sensibles; se sustituye por archivado (RNF-04) según P10 y P12 |
| 0.1 | 30/09/2026 | RF-16 baja a prioridad Media y queda como supuesto: la entrevista no lo confirmó |

## Revisión de la dupla 
| Campo| Detalle |
|---|---|
| Revisora| |
| Fecha de revisión |  |
| Observaciones recibidas |  |
| Cambios realizados |  |

    [ ] Todos los requisitos tienen identificador único y ninguno está repetido
    [ ] Cada requisito expresa una sola idea
    [ ] Cada requisito funcional tiene criterio de aceptación comprobable
    [ ] Cada requisito no funcional tiene una métrica, no solo un adjetivo
    [ ] El campo Origen distingue lo confirmado por el cliente de lo que sigo suponiendo
    [ ] Hay al menos un requisito no funcional por cada atributo de calidad que impone mi tipo de sistema
    [ ] Ningún requisito impone una solución técnica
    [ ] Todos los requisitos caben dentro del alcance declarado
    [ ] La tabla de trazabilidad está completa
    [ ] Mi dupla revisó el documento y su revisión está registrada
    [ ] Borré los ejemplos y las instrucciones en cursiva
