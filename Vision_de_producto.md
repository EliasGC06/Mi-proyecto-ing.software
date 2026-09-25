# PROYECTO 1

**Autor:** Elias García Cisneros

**Fecha de la última versión:** 31/08/2026

**Repositorio:** Base de datos RH

---

## 1. Descripción del sistema

**Nombre del sistema:** Base de datos e vacantes 

**Descripción:** Es una base de datos sobre las vacantes disponibles, prospectos y el personal que tenemos activo en la empresa. cada prospecto tiene su información como su nombre, edad, puesto, empresa, horario, localidad donde vive, documentos, urgencia por cubrir la vacante, numero de teléfono, escolaridad, correo, problemas médicos, comentarios, etc...

---

## 2. Problema y usuarios


**El problema:** Principal problema es que el proceso de contratación del personal, el cual es muy tardado, complicado y casi en todos los casos hay confusiones en el personal, también entre las diferentes áreas de la empresa , entre R.H., contaduría, finanzas y corporativo  

**Cómo se resuelve hoy sin el sistema:** Con mucha comunicación entre todos los empleado en el área de R.H.

**Usuarios del sistema:** Principalmente el área de R.H., pero también estaría abierto a contaduría, finanzas y corporativo; para que están informados por cualquier circunstancia.

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
|Recursos Humanos |Son los principales usuarios del sistema en el cual ellos deben estar activos en la base de datos para actualizar las vacantes que ya están ocupadas o disponibles y dar seguimiento a los puestos que están en la empresa | Que la información que se suba a la base de datos este mal; por ejemplo mal acomodo, información incompleta, desactualizada. |
|Finanzas, Contaduría | Consultar las vacantes que ya están ocupadas para darle seguimiento a sus nominas y cualquier tema con sus cuentas de banco, sus pagos o inconvenientes que se puedan presentar. |Que la información no se actualice, por ejemplo si ya salió una persona de su puesto, si sus pagos estén completos o incompletos o que su cuenta bancaria tiene o no problemas.  |
|Corporativo |Poder tener supervisión total de la base de datos para saber como es el estado de las vacantes de la empresa. |Falta de transparencia en la base de datos, como si falta algún dato o incongruencias en las vacantes disponibles o cubiertas. |


**Un conflicto entre usuarios:** 
La área de RH de tener la información más actualizada sobre sus empleados, como algún problema con su nomina, problema de salud o familiar, lo cual es parte de su proceso de selección. Sin embargo, el área de Finanzas, Contaduría y Corporativo quieren estar lo más actualizado posible, por lo cual todas las áreas ven todo lo que pasa, así que podrían ver conflictos entre las áreas, ya que contaduría podría ver lo que hace RH, lo cual so podrían meter en áreas que no le toca a cada área de la empresa.

---

## 3. Alcance

**Dentro del alcance**

-Clasificar la información (prospectos, vacantes, personal activo)

-Separar los elementos por categoría/estatus

-Verificar los elementos (validar que los datos capturados sean consistentes)

-Registrar nuevos elementos

-Seleccionar elementos en específico

-Filtrar los elementos (por vacante, urgencia, localidad, etc.)

**Explícitamente fuera del alcance**

-Comparar entre elementos automáticamente (no hace análisis comparativo automático entre prospectos)

-Generar reportes financieros o de nómina exportables

-Enviar notificaciones automáticas por correo o SMS a prospectos o personal

**Por qué queda fuera:**

La parte de reportes financieros y nominas, es parte ya del área de contaduría y finanzas, así que ya seria hacer un sistema con ellos, haciendo que podría crea confusión o sobrecarga para la base de datos. 
La parte de auto comparación es algo más avanzado en la base de datos que todavía no esta listo en la base de datos.

---

## 4. Tipo de sistema y restricciones

**Tipo de sistema:** De información 

**Por qué es de ese tipo:** 
El propósito de la base de datos es recopilar, organizar y recuperar toda la información sobre las vacantes disponibles, prospectos y el personal activo en la empresa, la cual es para agilizar el proceso de contrataciones y seguimiento, principalmente en el área de RH. 

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
|Confidencialidad |Se registran datos personales de las personas (salud, contactos, comentarios internos), las cuales no se pueden ver todas las áreas. |Filtración de información sensible, posible daño a su integridad y reputación de la persona, además puede resultar en un problema legal. |
|Integridad |Varias áreas ven la misma información, por lo cual si un dato es incorrecto o se duplica, se puede hacer una serie da desinformación y malas decisiones. | Confusiones entre áreas. |
|Usabilidad |El área de RH necesita ser más rápido y eficiente al capturar los datos, y hacer un sistema el cual sea fácil de usar sin saber mucho de tecnología.  |Se regresa al sistema anterior, lo cual seria un paso para atrás en la eficiencia de la empresa.  |

**Reglas de negocio que ya identifiqué:**

1. Un prospecto no puede estar activo en más de una vacante al mismo tiempo, para evitar procesos duplicados o contrataciones cruzadas.
2. Solo R.H. puede capturar, editar o eliminar los datos sensibles de un prospecto (problemas médicos, comentarios internos); las demás áreas solo deben saber información tecnica (estatus de la vacante, puesto, urgencia) o solo si es necesario y con confidencialidad. 
3. Una vacante marcada como "urgente" debe ser visible con prioridad para todas las áreas involucradas, para que no se atrase por falta de comunicación entre departamentos.

---

## 5. Ciclo de vida elegido

**Modelo elegido:** Ágil 

**Por qué le conviene a este proyecto:**

El proyecto tiene requisitos generales claros (registrar, clasificar, filtrar y dar seguimiento a prospectos y vacantes), pero conserva esa decisión de diseño abierta; también el control de acceso para las áreas de R.H., Contaduría/Finanzas y Corporativo. Un modelo ágil permite avanzar con lo que ya está definido y redefinir las reglas que ya están hechas, en lugar de borrar todo y hacerlo de nuevo.

Además, se requieren entregas constantes en ciertas fechas, lo cual coincide naturalmente con ciclos cortos de trabajo y retroalimentación. La coordinación es mucho más fácil, y el riesgo del sistema es moderado, por lo que no se justifica la rigidez de un modelo secuencial.

### Alternativas descartadas

**Alternativa 1: Prototipado**

El prototipado es más útil cuando la mayor preocupación está en la interfaz o en la interacción con el usuario. En este proyecto, la preocupación principal está en las reglas de negocio y en la estructura de la información, qué datos ve cada área, no en cómo se ve la pantalla. Invertir ciclos completos en maquetas visuales no resolvería el problema central del proyecto.

**Alternativa 2: Cascada**

La cascada exige cerrar todos los requisitos antes de empezar a construir. Aquí todavía hay un conflicto de diseño sin resolver, que acceso a datos hay que dejar ver entre áreas, y esperar a resolverlo por completo antes de avanzar retrasaría el proyecto sin necesidad.
