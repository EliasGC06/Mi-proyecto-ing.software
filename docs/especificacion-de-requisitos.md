# Especificación de requisitos

Sistema: Base de Datos de vacantes y prospectos R.H.

Autor: Elias García Cisneros

Versión: 0.1 

Fecha de la última actualización: 30/09/2026

## 1. Propósito y alcance

### Enlace Figma: 
https://scope-hull-09512178.figma.site/

Propósito del documento: En este documento se especifica lo que se hace y con que calidad debe tener el sistema que desarrollaremos para recopilar y concentrar la información de las vacantes, prospectos y personal activo de la empresa. Este nace en base a la visión del producto que hicimos y la entrevista realizada.

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
| Corporativo | Supervisa, coordina y comunica con R.H sobre las vacantes, prospectos y empleados que tiene la empresa, preguntando en persona sobre su estatus de cada uno; quitando mucho tiempo y esfuerzo. | Supervisar el estatus de todo el personal en una sola plataforma, viendo con claridad y transparencia lo que sucede con cada uno sin tener que preguntar al área de R.H. |

### Conflictos identificados entre usuarios:

Finanzas/Contaduría y Corporativo quieren visibilidad amplia, pero los datos sensibles (problemas médicos, comentarios internos, temas familiares) solo los puede ver R.H, como se confirmo en la entrevista, la visibilidad por área, las demás áreas que no son R.H ven únicamente estatus, puesto y urgencia. 

## 3. Requisitos funcionales

### 3.1 Resumen de RF
| ID | Nombre | Prioridad | Origen |
|---|---|---|---|
| RF-001 | Registrar prospecto y vacante | Alta | Visión (alcance 1 y 2) y P.5 |
| RF-002 | Mantener estatus del prospecto, si no termina su proceso, y la vacante | Alta | Visión (alcance 3) y P.12 |
| RF-003 | Archivar prospecto que no termina su proceso, para usar su registro en un futuro, si vuelve a aplicar | Alta | P.10 y P.12 |
| RF-004 | Validar campos obligatorios, formato de teléfono y correo | Alta | Visión (alcance 4) |
| RF-005 | Impedir que un prospecto esté activo en dos vacantes | Alta | P.11 |
| RF-006 | Filtrar vacantes y prospectos |Alta| Visión (alcance 5) |
| RF-007 | Adjuntar documentos a un prospecto | Media | Visión (alcance 6) |
| RF-008 | Consultar el historial profesional de un prospecto en una solo lugar| Alta | P.7 y P.9|
| RF-009 | Limitar lo que ven Finanzas/Contaduría y Corporativo | Alta | Visión (resolución del conflicto) | 
| RF-010 | Reservar los datos sensibles a RH | Alta | Visión (regla 2) |
| RF-011 | Mostrar las vacantes urgentes con prioridad | Alta | P.8 |
| RF-012 | Señalar las vacantes reabiertas a Finanzas/Contaduría | Alta | P.4, P.8 |

### 3.2 Fichas 
 
## RF-001 · Registrar prospecto y vacante 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir a RH registrar prospectos con nombre, edad, puesto, empresa, horario, localidad, escolaridad, teléfono y correo, y vacantes con puesto, área, horario, localidad, nivel de urgencia y motivo de apertura. |
| Origen | Supuesto (Visión, alcance 1 y 2). El campo "motivo de apertura" está Confirmado (P.5). |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario RH autenticado, cuando captura todos los campos de un prospecto y guarda, entonces aparece en el listado con estatus "prospecto"; cuando captura todos los campos de una vacante y guarda, entonces aparece en el listado con estatus "disponible" y su nivel de urgencia. |
| Relacionado con | CU-01, CU-02, RF-002, RF-004 |
 
## RF-002 · Mantener estatus del prospecto y de la vacante 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe asignar a cada prospecto exactamente un estatus (prospecto, en proceso, contratado o archivado) y a cada vacante exactamente uno (disponible u ocupada), y permitir a RH reabrir una vacante ocupada. |
| Origen | Supuesto (Visión, alcance 3). El estatus "archivado" está Confirmado (P.12) y la reapertura de vacantes viene de la excepción de la ficha de dominio (P.4). |
| Prioridad | Alta |
| Criterio de aceptación | Todo prospecto y toda vacante guardados tienen un solo estatus válido; cuando RH reabre una vacante ocupada e indica el motivo, su estatus pasa a "disponible", conserva su urgencia y se registra usuario, fecha y motivo. |
| Relacionado con | CU-02, CU-03, CU-07, CU-08, RF-003, RF-012, RNF-03 |

## RF-003 · Archivar prospecto que no termina su proceso y reutilizar su registro 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe cambiar a "archivado" a un prospecto que no termina su proceso, sin eliminar su información, y reutilizar ese registro si vuelve a aplicar. |
| Origen | Confirmado (P.10, P.12) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto "en proceso" que RH descarta, cuando RH confirma, entonces su estatus es "archivado" y sus datos y documentos siguen consultables; cuando RH registra a una persona que ya tiene un registro archivado, el sistema le muestra ese registro y le permite reactivarlo en lugar de crear uno nuevo. |
| Relacionado con | CU-08, CU-01, RF-002, RF-008, RNF-03|
 
## RF-004 · Validar campos obligatorios, formato de teléfono y correo 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe impedir guardar un registro con campos obligatorios vacíos, con un teléfono que no tenga 10 dígitos o con un correo sin formato válido. |
| Origen | Supuesto (Visión, alcance 4) |
| Prioridad | Alta |
| Criterio de aceptación | Cuando RH guarda con un campo obligatorio vacío, con teléfono "55123" o con correo "juan@", el sistema no guarda, marca cada campo con error e indica el formato esperado. |
| Relacionado con | CU-01, CU-02, RF-001, RNF-05 |
 
## RF-005 · Impedir que un prospecto esté activo en dos vacantes 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe rechazar la asignación de un prospecto que ya esté activo en otra vacante. |
| Origen | Confirmado (P.11) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto "en proceso" en la vacante A, cuando RH intenta asignarlo a la vacante B, el sistema rechaza la asignación, indica que está activo en la vacante A y el prospecto sigue ligado solo a A. |
| Relacionado con | CU-03, RF-002, RNF-02 |
 
## RF-006 · Filtrar vacantes y prospectos 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe filtrar los listados de vacantes y de prospectos por puesto, urgencia, localidad y estatus. |
| Origen | Supuesto (Visión, alcance 5) |
| Prioridad | Alta |
| Criterio de aceptación | Con uno o más filtros aplicados en cualquiera de los dos listados, solo aparecen los registros que cumplen todos los filtros. |
| Relacionado con | CU-05, CU-03 |
 
## RF-007 · Adjuntar documentos a un prospecto 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir a RH adjuntar documentos PDF, JPG o PNG de hasta 10 MB a un prospecto. |
| Origen | Supuesto (Visión, alcance 6). El formato y el tamaño son propuesta. |
| Prioridad | Media |
| Criterio de aceptación | Cuando RH adjunta un PDF de 2 MB, aparece en los documentos del prospecto con nombre y fecha; un archivo de 15 MB es rechazado con un mensaje. |
| Relacionado con | CU-01, CU-04, RF-001, RF-002, RF-008, RNF-05 |
 
## RF-008 · Consultar el historial profesional de un prospecto en un solo lugar 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar en una sola pantalla los datos, documentos y vacantes anteriores de un prospecto, incluso si aplicó hace meses. |
| Origen | Confirmado (P.7, P.9) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un prospecto que aplicó hace seis meses, cuando RH lo busca por nombre y abre su registro, ve en una sola pantalla sus datos, sus documentos y las vacantes a las que aplicó, con fechas. |
| Relacionado con | CU-04, RF-003, RF-007, RNF-05 |
 
## RF-009 · Limitar lo que ven Finanzas/Contaduría y Corporativo
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar a los usuarios de Finanzas/Contaduría y Corporativo únicamente estatus, puesto y urgencia. |
| Origen | Supuesto (Visión, resolución del conflicto) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario de Finanzas/Contaduría o Corporativo, cuando abre una vacante o un prospecto, ve solo estatus, puesto y urgencia; no ve documentos ni datos sensibles. |
| Relacionado con | CU-06, RF-010, RNF-01, RNF-06 |
 
## RF-010 · Reservar los datos sensibles a RH 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe permitir ver y editar los campos "problemas médicos" y "comentarios internos" solo a usuarios RH. |
| Origen | Supuesto (Visión, regla 2) |
| Prioridad | Alta |
| Criterio de aceptación | Dado un usuario sin rol RH, cuando intenta abrir o editar esos campos, el sistema no los muestra y rechaza la edición. |
| Relacionado con | CU-01, CU-04, RF-001, RNF-01, RNF-06 |
 
## RF-011 · Mostrar las vacantes urgentes con prioridad 
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar las vacantes urgentes antes que las demás y con una etiqueta "Urgente" para todas las áreas. |
| Origen | P.8 |
| Prioridad | Alta |
| Criterio de aceptación | En el listado de vacantes de cualquier rol, las vacantes urgentes aparecen en las primeras posiciones con la etiqueta "Urgente". |
| Relacionado con | CU-05, CU-06, RF-006 |
 
## RF-012 · Señalar las vacantes reabiertas a Finanzas/Contaduría
| Campo | Contenido |
|---|---|
| Descripción | El sistema debe mostrar a Finanzas/Contaduría, al iniciar sesión, las vacantes reabiertas en las últimas 24 horas. |
| Origen | Confirmado (P.4, P.8) la necesidad de avisar a Finanzas.|
| Prioridad | Alta |
| Criterio de aceptación | Dado una vacante reabierta hoy, cuando un usuario de Finanzas/Contaduría abre su pantalla de inicio, la ve en "Reabiertas recientes" con fecha y hora de reapertura. |
| Relacionado con | CU-07, RF-002, RNF-02 |

## 4. Requisitos no funcionales

### 4.1 Resumen

|ID|Atributo de calidad|Descripción Métrica|Origen|Prioridad|Por qué importa|Afecta a|
|---|---|---|---|---|---|---|
|RNF-01|Confidencialidad|El 100% de los intentos de un usuario sin rol RH por ver o editar datos sensibles (problemas médicos, comentarios internos) debe ser rechazado. Se verifica con pruebas de los tres roles.| Visión (atributo de confidencialidad)| Alta | Son datos de salud y comentarios internos; una filtración daña a la persona y genera riesgo legal, por eso no admite excepciones.|RF-009, RF-010, CU-01, CU-04, CU-06 |
|RNF-02|Integridad|Un cambio de estatus de una vacante o prospecto debe ser visible para los tres roles en 5 segundos o menos.| Entrevista (P.8 y ficha de dominio: respuestas desactualizadas a Contaduría); Visión (integridad)|Alta| Hoy Contaduría recibe respuestas desactualizadas porque la información no se sincroniza entre áreas; con 5 segundos el problema desaparece en la práctica.| RF-002, RF-012, CU-03, CU-06, CU-07 |
|RNF-03|Integridad|Cero registros de prospecto eliminados físicamente: toda baja se hace por archivado y se conservan sus datos y documentos. | Confirmado (P.10, P.12)| Alta | La información de un prospecto nunca se borra, porque puede volver a aplicar y RH necesita su historial.| RF-003, CU-01, CU-08 |
|RNF-04|Usabilidad|Una persona de RH que no haya usado el sistema debe registrar un prospecto completo en 3 minutos o menos, sin capacitación. Se prueba con 3 personas.|Visión (usabilidad)|Alta|Si el sistema es más lento que Excel o WhatsApp, RH regresa al método anterior y el sistema fracasa.|RF-001, RF-004, CU-01 |
|RNF-05|Usabilidad|Encontrar el historial de un prospecto debe tomar 30 segundos o menos y 3 clics como máximo. | Confirmado (P.7, P.9)| Alta | Buscar archivo por archivo es el mayor dolor reportado por RH y lo que más tiempo le quita.|RF-008, CU-04 |
|RNF-06|Trazabilidad|El 100% de los cambios de estatus y de datos sensibles debe registrar usuario, fecha y hora. | Visión (Corporativo teme falta de transparencia)| Media | Permite saber quién cambió qué y cuándo, y evita incongruencias entre áreas.|RF-002, RF-010, CU-03, CU-07, CU-08 |

## 5. Casos de uso

|Casos de uso| Descripción |
|---|---|
| CU-01 | Registrar prospecto |
| CU-02 | Registrar vacante |
| CU-03 | Asignar prospecto a vacante |
| CU-04 | Consultar historial de un prospecto |
| CU-05 | Filtrar vacantes y prospectos |
| CU-06 | Consultar estatus de vacantes |
| CU-07 | Reabrir vacante ocupada |
| CU-08 | Archivar prospectos |

### CU-01 · Registrar prospecto
 
| Campo | Contenido |
|---|---|
| Actor | Recursos Humanos (RH) |
| Objetivo | Dar de alta a un prospecto con sus datos y documentos para poder darle seguimiento. |
| Precondición | RH está autenticado. |
| Escenario principal | 1. RH selecciona "Registrar prospecto". <br> 2. El sistema muestra el formulario con nombre, edad, puesto, empresa, horario, localidad, escolaridad, teléfono, correo, problemas médicos y comentarios internos (estos dos últimos solo visibles para RH). <br> 3. RH captura los datos. <br> 4. RH adjunta los documentos del prospecto. <br> 5. RH guarda. <br> 6. El sistema valida los campos obligatorios y el formato de teléfono y correo. <br> 7. El sistema guarda el prospecto con estatus "prospecto" y lo muestra en el listado. |
| Flujos alternos | **A1 · Campo obligatorio vacío o formato inválido (paso 6).** El sistema no guarda, marca cada campo con error e indica el formato esperado; RH corrige y vuelve al paso 5. <br> **A2 · Documento no válido (paso 4).** Si el archivo no es PDF, JPG o PNG, o pesa más de 10 MB, el sistema lo rechaza con un mensaje; RH adjunta otro o continúa sin él. <br> **A3 · La persona ya tiene un registro archivado (paso 6).** El sistema muestra ese registro y permite reactivarlo en lugar de crear uno nuevo; si RH acepta, se actualiza el registro conservando su historial. |
| Postcondición | El prospecto queda registrado con estatus "prospecto", con sus datos y documentos consultables; los datos sensibles solo los ve RH. Si hubo un fallo, no se guarda ningún dato. |
| Requisitos que utiliza | RF-001, RF-003, RF-004, RF-007, RF-010 |
 
### CU-02 · Registrar vacante
 
| Campo | Contenido |
|---|---|
| Actor | Recursos Humanos (RH) |
| Objetivo | Dar de alta una vacante que hay que cubrir, con su nivel de urgencia. |
| Precondición | RH está autenticado. |
| Escenario principal | 1. RH selecciona "Registrar vacante". <br> 2. El sistema muestra el formulario con puesto, área, horario, localidad, nivel de urgencia y motivo de apertura. <br> 3. RH captura los datos. <br> 4. RH guarda. <br> 5. El sistema valida los campos obligatorios. <br> 6. El sistema guarda la vacante con estatus "disponible" y su nivel de urgencia. <br> 7. El sistema muestra la vacante en el listado. |
| Flujos alternos | **A1 · Campo obligatorio vacío (paso 5).** El sistema no guarda y marca cada campo faltante; RH lo completa y vuelve al paso 4. <br> **A2 · RH cancela (paso 4).** No se guarda ningún dato y RH regresa al listado de vacantes. |
| Postcondición | La vacante queda con estatus "disponible" y es visible para RH, Finanzas/Contaduría y Corporativo (estas dos últimas áreas solo ven estatus, puesto y urgencia). Si hubo un fallo, no se guarda ningún dato. |
| Requisitos que utiliza | RF-001, RF-002, RF-004 |
 
### CU-03 · Asignar prospecto a vacante
 
| Campo | Contenido |
|---|---|
| Actor | Recursos Humanos (RH) |
| Objetivo | Ligar un prospecto con una vacante disponible y dejarlo "en proceso". |
| Precondición | RH está autenticado; existe un prospecto registrado y una vacante con estatus "disponible". |
| Escenario principal | 1. RH abre una vacante disponible desde el listado. <br> 2. RH selecciona "Asignar prospecto". <br> 3. El sistema muestra los prospectos sin vacante activa, con filtros. <br> 4. RH selecciona un prospecto. <br> 5. El sistema muestra el resumen (prospecto y vacante) y pide confirmación. <br> 6. RH confirma. <br> 7. El sistema verifica que el prospecto no esté activo en otra vacante y que la vacante siga disponible. <br> 8. El sistema cambia el prospecto a "en proceso", lo liga a la vacante y registra usuario, fecha y hora. <br> 9. El sistema muestra la vacante con el prospecto asignado. |
| Flujos alternos | **A1 · Prospecto activo en otra vacante (paso 7).** El sistema rechaza la asignación e indica en cuál vacante está activo; RH puede cancelar o abrir esa vacante. <br> **A2 · La vacante dejó de estar disponible (paso 7).** Otra sesión la ocupó mientras RH decidía; el sistema avisa, no asigna y actualiza el listado. <br> **A3 · Prospecto archivado que vuelve a aplicar (paso 3).** RH lo busca; el sistema lo muestra con marca "archivado"; RH lo reactiva conservando su historial y continúa en el paso 4. <br> **A4 · No hay prospectos elegibles (paso 3).** El sistema lo informa y ofrece registrar un prospecto (CU-01). <br> **A5 · RH cancela (paso 6).** No se guarda ningún cambio y regresa al detalle de la vacante. |
| Postcondición | El prospecto está "en proceso" y ligado a la vacante, el cambio queda registrado y las demás áreas ven el estatus actualizado en 5 segundos o menos. Si hubo un fallo, no cambia ningún dato. |
| Requisitos que utiliza | RF-002, RF-005, RF-006 |
 
### CU-04 · Consultar historial de un prospecto
 
| Campo | Contenido |
|---|---|
| Actor | Recursos Humanos (RH) |
| Objetivo | Ver en un solo lugar los datos, documentos y vacantes anteriores de un prospecto, incluso si aplicó hace meses. |
| Precondición | RH está autenticado; existe al menos un prospecto registrado (activo o archivado). |
| Escenario principal | 1. RH abre la búsqueda de prospectos. <br> 2. RH escribe el nombre del prospecto. <br> 3. El sistema muestra las coincidencias. <br> 4. RH selecciona al prospecto. <br> 5. El sistema muestra en una sola pantalla sus datos (incluidos los sensibles), sus documentos y las vacantes a las que aplicó, con fechas. <br> 6. RH abre un documento si lo necesita. |
| Flujos alternos | **A1 · Sin coincidencias (paso 3).** El sistema lo informa y ofrece registrar al prospecto (CU-01). <br> **A2 · Varios prospectos con el mismo nombre (paso 3).** El sistema muestra puesto y localidad de cada uno para que RH distinga; RH puede usar los filtros (CU-05). <br> **A3 · Prospecto archivado (paso 5).** El sistema muestra su historial completo con la marca "archivado", con sus datos y documentos conservados. |
| Postcondición | RH consultó el historial completo sin modificar ningún dato. |
| Requisitos que utiliza | RF-007, RF-008, RF-010 |
 
### CU-05 · Filtrar vacantes y prospectos
 
| Campo | Contenido |
|---|---|
| Actor | Recursos Humanos (RH) |
| Objetivo | Encontrar las vacantes o los prospectos que cumplen ciertos criterios. |
| Precondición | RH está autenticado; existen registros en el listado. |
| Escenario principal | 1. RH abre el listado de vacantes o el de prospectos. <br> 2. El sistema muestra el listado completo; en vacantes, las urgentes aparecen primero con la etiqueta "Urgente". <br> 3. RH elige uno o más filtros: puesto, urgencia, localidad o estatus. <br> 4. El sistema muestra solo los registros que cumplen todos los filtros. <br> 5. RH abre el registro que necesita. |
| Flujos alternos | **A1 · Ningún registro cumple los filtros (paso 4).** El sistema muestra "Sin resultados" y ofrece limpiar los filtros. <br> **A2 · RH limpia los filtros (paso 4).** El sistema vuelve a mostrar el listado completo. |
| Postcondición | RH ve el listado filtrado; no se modifica ningún dato. |
| Requisitos que utiliza | RF-006, RF-011 |
 
### CU-06 · Consultar estatus de vacantes
 
| Campo | Contenido |
|---|---|
| Actor | Finanzas/Contaduría y Corporativo |
| Objetivo | Conocer el estatus vigente de las vacantes sin tener que preguntar a RH. |
| Precondición | El usuario está autenticado con el rol de Finanzas/Contaduría o de Corporativo. |
| Escenario principal | 1. El usuario inicia sesión. <br> 2. Si es de Finanzas/Contaduría, el sistema muestra en su pantalla de inicio la sección "Reabiertas recientes" con las vacantes reabiertas en las últimas 24 horas, con fecha y hora. <br> 3. El sistema muestra el listado de vacantes con estatus, puesto y urgencia; las urgentes aparecen primero con la etiqueta "Urgente". <br> 4. El usuario abre una vacante. <br> 5. El sistema muestra solo su estatus, puesto y urgencia. |
| Flujos alternos | **A1 · No hay vacantes reabiertas (paso 2).** La sección indica "Sin reabiertas recientes". <br> **A2 · El usuario intenta ver datos sensibles o documentos (paso 5).** El sistema no los muestra. <br> **A3 · El usuario es de Corporativo (paso 2).** Ve el listado de vacantes sin la sección "Reabiertas recientes", que es solo para Finanzas/Contaduría. |
| Postcondición | El usuario conoce el estatus vigente de las vacantes sin haber modificado ningún dato. |
| Requisitos que utiliza | RF-009, RF-011, RF-012 |
 
### CU-07 · Reabrir vacante ocupada
 
| Campo | Contenido |
|---|---|
| Actor | Recursos Humanos (RH) |
| Objetivo | Volver a dejar disponible una vacante que se había ocupado (por ejemplo, porque la persona renunció sin avisar) y que Finanzas/Contaduría se entere. |
| Precondición | RH está autenticado; existe una vacante con estatus "ocupada". |
| Escenario principal | 1. RH busca y abre la vacante ocupada. <br> 2. RH selecciona "Reabrir vacante". <br> 3. El sistema pide el motivo de la reapertura. <br> 4. RH indica el motivo y confirma. <br> 5. El sistema cambia la vacante a "disponible", conserva su nivel de urgencia y registra usuario, fecha, hora y motivo. <br> 6. El sistema incluye la vacante en "Reabiertas recientes" de Finanzas/Contaduría. <br> 7. El sistema muestra la confirmación. |
| Flujos alternos | **A1 · RH no indica el motivo (paso 4).** El sistema no reabre la vacante y pide el motivo. <br> **A2 · La vacante ya está disponible (paso 5).** Otra sesión la reabrió antes; el sistema lo informa y no cambia nada. <br> **A3 · RH cancela (paso 4).** No se guarda ningún cambio y regresa al detalle de la vacante. |
| Postcondición | La vacante está "disponible" con su motivo registrado, visible para todas las áreas, y Finanzas/Contaduría la ve en "Reabiertas recientes" durante 24 horas. Si hubo un fallo, no cambia ningún dato. |
| Requisitos que utiliza | RF-002, RF-012 |
 
### CU-08 · Archivar prospecto
 
| Campo | Contenido |
|---|---|
| Actor | Recursos Humanos (RH) |
| Objetivo | Apartar a un prospecto que no termina su proceso sin perder su información, para reutilizarla si vuelve a aplicar. |
| Precondición | RH está autenticado; existe un prospecto con estatus "prospecto" o "en proceso". |
| Escenario principal | 1. RH abre el registro del prospecto. <br> 2. RH selecciona "Archivar". <br> 3. El sistema pide confirmación e indica que la información se conserva. <br> 4. RH confirma. <br> 5. El sistema cambia el estatus a "archivado", libera la vacante si el prospecto estaba ligado a una y registra usuario, fecha y hora. <br> 6. El sistema muestra la confirmación; los datos y documentos del prospecto siguen consultables. |
| Flujos alternos | **A1 · RH cancela (paso 4).** No se guarda ningún cambio. <br> **A2 · El prospecto ya está contratado (paso 2).** El sistema no permite archivarlo, porque ya terminó su proceso. <br> **A3 · El prospecto ya está archivado (paso 2).** El sistema lo informa y no cambia nada. |
| Postcondición | El prospecto queda "archivado" con todos sus datos y documentos conservados, y puede reutilizarse si vuelve a aplicar (CU-01). Si hubo un fallo, no cambia ningún dato. |
| Requisitos que utiliza | RF-002, RF-003 |

## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
|---|---|---|---|
| RF-001 | Visión (alcance 1 y 2), P.5 | CU-01, CU-02 | — |
| RF-002 | Visión (alcance 3), P.12, P.4 | CU-02, CU-03, CU-07, CU-08 | P.4, P.5 |
| RF-003 | P.10, P.12 | CU-01, CU-08 | P.3 |
| RF-004 | Visión (alcance 4) | CU-01, CU-02 | — |
| RF-005 | P.11 | CU-03 | P.6 |
| RF-006 | Visión (alcance 5) | CU-03, CU-05 | P.1, P.3 |
| RF-007 | Visión (alcance 6) | CU-01, CU-04 | — |
| RF-008 | P.7, P.9 | CU-04 | — |
| RF-009 | Visión (resolución del conflicto) | CU-06 | P.7 |
| RF-010 | Visión (regla 2) | CU-01, CU-04 | P.7 |
| RF-011 | P.8 (supuesto) | CU-05, CU-06 | P.1, P.7 |
| RF-012 | P.4, P.8 | CU-06, CU-07 | P.7 |


## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
|---|---|---|---|
| 01/10/2026 | RF-001 | Se unió el registro de prospecto y vacante y se agregó el campo "motivo de apertura" a la vacante. | Por no tener demasiados requisitos funcionales, en este caso es la misma acción y el mismo proceso. La urgencia de una vacante depende de cómo se desocupó (P.5). |
| 01/10/2026 | RF-002 | Se agregó el estatus "archivado" para el prospecto y la posibilidad de reabrir una vacante ocupada. | La Visión solo tenía tres estatus y no contemplaba la reapertura; la entrevista confirmó que los prospectos no contratados se archivan (P.12) y la ficha de dominio describe la vacante urgente que se cierra de golpe (P.4). |
| 01/10/2026 | RF-003 | Se agregó archivar al prospecto que no termina su proceso y reutilizar su registro si vuelve a aplicar. | En la entrevista se confirmó que la información del prospecto se conserva y se actualiza al volver a aplicar (P.10, P.12). |
| 01/10/2026 | RF-008 | Se agregó consultar el historial profesional de un prospecto en un solo lugar. | Buscar archivo por archivo apareció como el mayor dolor, repetido en dos respuestas de la entrevista (P.7, P.9). |
| 01/10/2026 | RF-012 | Se agregó señalar a Finanzas/Contaduría las vacantes reabiertas. | Hay que avisarles con urgencia cuando una vacante se reabre (P.4, P.8) |
| 01/10/2026 | RNF-01 a RNF-06 / Visión del producto | Se agrego una métrica a los atributos de calidad "trazabilidad". | La Visión nombraba solo 3 atributos, pero observo que faltaba uno que definiera los cambios que se hacen en el sistema y que queden registrado quien fue, cuando y en que parte del mismo. |

## 8. Revisión de la dupla

| Campo | Elemento |
|---|---|
| Fecha de revisión | 01/10/2026 |
| Observaciones recibidas | 1. RF-001: el registro de prospecto y el de vacante eran dos requisitos separados, pero es la misma acción y el mismo proceso. <br> 2. Atributos de calidad: la Visión nombraba solo tres atributos y faltaba uno que registrara quién hizo un cambio, cuándo y en qué parte del sistema. |
| Cambios realizados | 1. Se unió el registro de prospecto y vacante en RF-001 y se agregó el campo "motivo de apertura" a la vacante. <br> 2. Se agregó el atributo de trazabilidad con su métrica (RNF-06). |
