**Automatización del proceso de reclamaciones veterinarias en BarKibu**

![David Periscal](media/image1.png){width="1.3541666666666667in"
height="1.3541666666666667in"}David Periscal

Last updated Apr 21

**🐶 Contexto**

En Barkibu ofrecemos seguros de salud para mascotas. Cuando un pet
parent lleva a su perro o gato al veterinario, puede solicitar un
reembolso a través de nuestra app móvil.

Para ello, debe enviarnos ciertos documentos relacionados con la visita:

- Una factura, con las líneas de coste detalladas.

- Un informe veterinario, que describe los síntomas, el diagnóstico y el
  tratamiento.

- En algunos casos, el historial clínico de la mascota, cuando es
  necesario comprobar si la condición estaba presente antes de contratar
  el seguro.

Nuestro equipo veterinario revisa manualmente esta documentación para
decidir:

- Si lo reclamado está cubierto por la póliza del seguro.

- Si la reclamación está afectada por preexistencias (condiciones
  previas a la contratación, no cubiertas).

- Si se encuentra dentro de algún periodo de carencia (intervalo de
  tiempo tras la contratación durante el cual ciertas coberturas aún no
  aplican).

- Qué cantidad debe reembolsarse al usuario.

Este proceso puede ser largo, y actualmente depende en gran parte de la
revisión manual.

**🎯 Objetivo del reto**

Queremos avanzar en la automatización de este proceso, manteniendo la
precisión en las decisiones, mejorando la experiencia del usuario y
aliviando la carga sobre el equipo veterinario.

**✍️ Lo que te pedimos**

Queremos que analices este reto y nos cuentes cómo lo abordarías si
fueses parte del equipo técnico de BarKibu. Lo que nos interesa es ver
cómo piensas, cómo abordas el problema y cómo planteas una solución
técnica estructurada.

Tu propuesta debería incluir:

**Análisis del problema**

- ¿Cómo entiendes el reto y sus objetivos?

> El reto esta basado en analizar datos claves de documentos (factura,
> informe veterinario e historial clínico), no necesariamente
> estandarizados, escritos en diferentes formatos y estructuras.
> Idealmente habría que, de alguna forma, extraer datos clave de los
> documentos mencionados, para poder compararlos contra ciertas
> condiciones, y así decidir si se percibe como reembolso o no.

- ¿Qué partes consideras clave para resolver?

> Desde luego, la parte clave y más importante, es la de la
> interpretación y extracción de los datos proporcionados.

- ¿Qué restricciones, riesgos o complejidades detectas?

> En cuanto a la automatización de la ingesta de datos a través de los
> documentos que se presentan, suponen varios desafíos:

- ¿Están en formato digital? Si es así se le podría pasar un OCR. Si
  llevan anotaciones manuales complica mucho más la ingestión de datos.

- Que palabras clave se pueden utilizar para detectar el concepto de los
  datos que están siendo procesados.

- ¿Hay posibilidad de que el usuario introduzca estos datos manualmente,
  previamente a adjuntar los documentos? Siendo así, podríamos con
  confianza utilizar cierta información para hacer parte del filtrado.

**  
Descomposición del problema**

- Divide el reto en partes más pequeñas y manejables.

> El reto podría ser dividido en cuatro partes;

- tres de ellas se basarían en cada documento aportado: interpretación
  de factura, informe veterinario e historial clínico.

- La parte de la creación de las condiciones para filtrar las
  solicitudes.

<!-- -->

- Prioriza los subproblemas en función de su impacto y viabilidad.

> Hay un dato muy importante que no esta proporcionado en el contexto, y
> es el tiempo que conlleva cada comprobación del equipo de
> veterinarios. A mi parecer, habría que priorizar las tareas que
> potencialmente ahorren más tiempo dividido complejidad o tiempo de
> desarrollo. A mayor numero de la formula anterior, mayor prioridad
> (dependiendo también del contexto).
>
> Aunque esta claro que el paso para procesar historiales clínicos
> probablemente tendría menos prioridad que el resto, dado que es un
> documento que no siempre se solicita.

**  
Diseño del sistema**

- Propón una arquitectura o enfoque técnico general para abordar el
  problema.

> Hay dos componentes clave para tener una solución funcionando
> mínimamente, el resto podría implementarse siguiendo CI/CD. Estos dos
> componentes serian la primera ingestión de datos (se elegiría en
> función de la prioridad mencionada anteriormente) y las condiciones
> que se pueden aplicar para facilitar el filtrado de solicitudes.

- Explica cómo interactuarían los distintos componentes o etapas del
  sistema.

> La mejor forma de resumir los componentes y etapas del sistema en este
> momento del proyecto seria probablemente con un diagrama:
>
> ![](media/image2.png){width="5.895833333333333in"
> height="4.802083333333333in"}

**  
Pros y contras de tu enfoque**

- ¿Qué ventajas tiene tu planteamiento?

  - Divide responsabilidades dentro de un contexto lógico.

  - Cualquier estado relevante de la devolución queda guardado en el
    CRM.

  - El servidor OCR podría entrenarse para que, con más datos, mejorara
    su análisis.

- ¿Dónde podrían surgir problemas o puntos débiles?

  - El programa OCR no es infalible, hay muchas variables de como pueden
    estar estructurados los documentos/informes.

  - Como parte del proceso de decisión, habría que introducir un estado
    de decisión a confirmar por un compañero, para evitar falsos
    positivos/negativos en los casos que no haya un claro resultado de
    la decisión de la devolución.

- ¿Qué alternativas consideraste y por qué descartaste alguna?

> Desgraciadamente no podemos confiar plenamente en el software OCR,
> sería lo ideal dado que evitaría tener que pedir datos extras, o
> involucrar en parte del proceso, pasos manuales.
>
> El impacto de este inconveniente se intenta disminuir con:

- esos datos extra que el usuario podría introducir en la app móvil
  (importe, fecha, operación/enfermedad);

- Los casos dudosos siendo enviados al personal de Barkibu para análisis
  manual.

**  
Planificación**

- ¿Qué pasos seguirías para implementar tu solución?

> Crear primero el servidor de solicitudes, si no existe ya.
>
> Segundo utilizaría los datos que sean proporcionados por el pet parent
> que sean relevantes a la hora de poder tomar la decisión del reembolso
> y de comunicarlo a las partes relevantes.
>
> Ya por último utilizaría el servicio de OCR sobre los documentos
> escaneados para potenciar la información que tenemos acerca del
> proceso y así tener una mejor "fotografía" de cada caso.

- ¿Cómo dividirías el trabajo en fases o entregas?

Una primera fase, y luego entregas basadas en CI/CD.

La primera fase cubriría los componentes clave para tener una solución
funcionando mínimamente. El resto podría implementarse siguiendo CI/CD.

- ¿Qué construirías primero y por qué?

Hay un dato muy importante que no está proporcionado en el contexto, y
es el tiempo que conlleva cada comprobación del equipo de veterinarios.
A mi parecer, habría que priorizar las tareas que potencialmente ahorren
más tiempo dividido complejidad o tiempo de desarrollo. A mayor numero
de la formula anterior, mayor prioridad (dependiendo también del
contexto).

Aunque está claro que el paso para procesar historiales clínicos
probablemente tendría menos prioridad que el resto, dado que es un
documento que no siempre se solicita.

**✨ Bonus**

También nos interesa saber si se te ocurren maneras de aprovechar los
datos estructurados que se generan a lo largo del proceso para crear
nuevas funcionalidades o productos que aporten valor a nuestros
usuarios.

- Se podrían aprovechar los datos existentes de este proceso para
  analizar nuevas necesidades de mercado: ¿por qué motivo se rechazan
  más solicitudes de reembolso?  
  ¿Hay alguna clausula extra que podamos añadir como un servicio para
  que se cubran las necesidades de los pet parents?

- Tener un buen servicio OCR podría abrir nuevas posibilidades de
  automatización al resto de Barkibu consiguiendo así que esa inversión
  tenga un mejor retorno.

**🧪 Formato de entrega**

Puedes enviarnos tu propuesta en el formato que prefieras:

- Documento (PDF, Notion, Google Docs\...)

- Presentación

- Diagrama

- Markdown en un repositorio

No buscamos una respuesta perfecta ni definitiva. Lo que queremos es ver
tu razonamiento, tu capacidad para estructurar problemas complejos y tu
criterio técnico.

¿Listo para el desafío?
