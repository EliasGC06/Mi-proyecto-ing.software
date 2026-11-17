# Análisis de impacto de SC-001

> Plantilla de análisis de impacto · Ingeniería de Software I · SIS3407
> Guárdala en tu repositorio como `doc/cambios/ai-001-nombre-del-cambio.md`
> Lo escribe **el dueño del sistema**, sobre la solicitud que le entregó su dupla.

---

## Solicitud que se analiza

Referencia a la solicitud: `sc-001-nombre-del-cambio.md`, quién la pidió y
en qué fecha.

## Traducción a términos del sistema

La solicitud viene en palabras del solicitante. Aquí la traduces a lo que
significa dentro de **tu** sistema, con los nombres que tú le pusiste a las
cosas.

> Ejemplo. Lo pedido: "quiero que al dueño le llegue un aviso por WhatsApp".
> Traducido: el componente Notificador necesita una implementación nueva de
> la interfaz IEnviable que hable con un servicio de mensajería externo, y
> la clase Dueño necesita un atributo para el número de teléfono.

Si no puedes traducirlo, no puedes estimarlo. Pregunta antes de seguir.

## Impacto directo

Lo que la solicitud toca **por definición**: lo que hay que modificar para
que el cambio exista.

| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| Requisito RF-xx | 2 | |
| Caso de uso | 2 | |
| Diagrama de clases | 4 | |
| Diagrama de secuencia | 4 | |

## Impacto indirecto

Lo que se rompe o deja de ser cierto **por culpa del cambio directo**, aunque
la solicitud no lo mencione. Esta sección es la que separa una estimación
seria de una adivinanza, y es la que todo el mundo olvida.

| Artefacto | Unidad | Por qué se ve afectado |
|---|---|---|
| Registro de decisión ADR-xx | 3 | |
| Diagrama de contenedores C4 | 3 | |
| Máquina de estados | 4 | |

> Recorre la lista completa y escribe "sin impacto" donde no lo haya.
> Un renglón vacío no distingue entre *lo revisé y no se toca* y
> *se me olvidó revisarlo*.

## Lo que queda sin tocar

Nómbralo explícitamente. Decir qué **no** se mueve es tan valioso como decir
qué sí, porque acota el riesgo y permite que alguien más te corrija.

## Riesgos

Qué podría salir mal al hacer este cambio, y qué tan probable es. Incluye
los riesgos de no hacerlo si los hay.

## Alternativas consideradas

Casi siempre hay más de una forma de resolver una solicitud. Escribe al menos
dos y di por cuál te inclinas:

1. **<Opción A>** · qué implica, qué cuesta, qué deja pendiente.
2. **<Opción B>** · lo mismo.

## Estimación de esfuerzo

| Concepto | Esfuerzo |
|---|---|
| Cambios de diseño y documentación | |
| Implementación | |
| Pruebas | |
| **Total** | |

**De dónde sale este número.** Explica el criterio: con qué lo comparaste,
qué supusiste, qué no incluiste. Una estimación sin su razonamiento no se
puede discutir ni corregir, y por lo tanto no sirve.

## Recomendación

Aceptar, rechazar o aplazar, **y por qué**. Si recomiendas aplazar, di qué
tendría que ocurrir para retomarlo.

---

*El objetivo de este documento no es dar un número exacto. Es que cualquiera
pueda seguir tu razonamiento y señalarte dónde te equivocaste.*
