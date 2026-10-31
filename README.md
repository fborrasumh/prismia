# PRISMIA

Revisión sistemática con tres revisores de IA de proveedores distintos. Aplicación web de un solo fichero, en español, inglés y portugués.

**Usar la app:** https://fborrasumh.github.io/prismia/

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23144807.svg)](https://doi.org/10.5281/zenodo.23144807)

## Qué hace

- **Registros.** Busca en PubMed y OpenAlex o importa exportaciones RIS y CSV (PubMed, Scopus, Web of Science). Detecta duplicados por DOI, PMID y título normalizado.
- **Cribado por título y resumen.** El revisor 1 y el revisor 2 evalúan cada registro por separado y a ciegas. «Quizá» cuenta como paso a la fase siguiente. Si discrepan, decide un tercer modelo (el árbitro), que ve sus razonamientos de forma anónima y en orden aleatorio. Ante la duda se incluye el registro.
- **Texto completo.** Lo recupera de Europe PMC cuando el artículo es de acceso abierto. Para el resto ofrece el enlace de acceso abierto (Unpaywall, OpenAlex) y la subida manual de PDF, también en bloque con reconocimiento del DOI de la primera página. Repite el cribado doble con árbitro sobre el texto completo y registra el motivo de cada exclusión.
- **Extracción de datos.** Plantilla base editable. Los dos revisores extraen cada campo con una cita literal; el árbitro resuelve solo los campos discrepantes.
- **Riesgo de sesgo.** RoB 2, ROBINS-I, QUADAS-2 y Newcastle-Ottawa, elegidas según el diseño extraído (se puede cambiar por estudio). La IA juzga los dominios; el juicio global se calcula en el código con las reglas de cada herramienta.
- **Informe.** Diagrama de flujo PRISMA 2020 (SVG y PNG), semáforo de riesgo de sesgo, texto de métodos y declaración de uso de IA generados a partir de lo que realmente ocurrió, informe completo en Word (pregunta, métodos, diagrama, características de los estudios, sesgo, excluidos con motivo, verificación humana y uso de IA), tablas en CSV, Excel y RIS, lista PRISMA 2020 prerrellenada y un **paquete de reproducibilidad** en ZIP con todo lo anterior, los prompts usados y el proyecto.

## Novedades de la versión 2.0

- **Varios proyectos** en el mismo navegador (Inicio → Mis proyectos): abrir, renombrar, duplicar para probar otros modelos, exportar, copias de seguridad automáticas tras cada fase y papelera de 30 días. Importar un proyecto siempre crea uno nuevo; el ejemplo vive en su propio proyecto.
- **Deshacer** (botón y Ctrl+Z) en todas las acciones que borran o cambian algo.
- **Fuentes**: cada búsqueda o importación se puede quitar; se avisa si un fichero ya se importó o si una búsqueda se repite, y se puede **actualizar una búsqueda** para traer solo lo nuevo (revisiones vivas).
- **Volver a ejecutar una fase** con otros modelos, sobre todos los registros, solo los desacuerdos, los seleccionados o los que fallaron. Los resultados anteriores se archivan y se pueden **comparar corridas** (concordancia, κ, registros que cambian) como análisis de sensibilidad. Si una decisión cambia, lo que dependía de ella se invalida en cascada.
- **Ficha de registro** con las decisiones y razonamientos de los tres modelos, el texto completo, la extracción con citas, el sesgo, una nota y el historial. Tablas con búsqueda, filtros y paginación.
- **Control humano**: corregir cualquier decisión (se conserva la de la IA), verificación a ciegas de una muestra aleatoria con sensibilidad, especificidad y κ, envío de la muestra a un coautor y comparación entre personas, y colas para confirmar exclusiones a texto completo y campos con ⚠.
- **Texto completo**: ver, descargar, quitar o reemplazar el texto de cada estudio; revisión de la subida en bloque antes de aplicarla; volver a buscar los no recuperados; aviso de texto recortado; transcripción opcional de PDF escaneados con un modelo con visión.
- **Metaanálisis**: campos numéricos opcionales y exportación con un guion de R para el paquete *meta*.
- **Coste**: una clave por proveedor con botón de prueba, tokens reales devueltos por cada proveedor, precios configurables y estimación de coste antes de cada fase; tiempo restante durante la ejecución.
- **Integridad de datos**: los textos completos se guardan por proyecto (en la 1.0 podían pasar de un proyecto a otro), cambiar un PDF invalida la extracción y el sesgo que se hicieron con el anterior, y el texto de métodos nombra los modelos que realmente se usaron. Los proyectos de la 1.0 se migran solos.

## Qué comprueba el código y qué hace la IA

La IA propone; el código decide y comprueba.

- Decide el acuerdo entre revisores, cuándo interviene el árbitro y la decisión final, y deja constancia de quién decidió cada cosa.
- Calcula el kappa de Cohen, el acuerdo observado y PABAK.
- Calcula todos los contadores del diagrama PRISMA.
- Comprueba que cada cita de la extracción y del riesgo de sesgo aparece en el texto del estudio, y que las cifras extraídas también aparecen. Lo que no se puede comprobar se marca con ⚠; no se oculta.
- Calcula el juicio global de riesgo de sesgo.
- Se detiene solo si hay varios errores seguidos (clave, saldo, modelo sin acceso) y permite reintentar solo lo que falló.
- Calcula la sensibilidad, la especificidad y κ de la IA frente a la verificación humana.

## Cómo se usa la IA

Con la clave propia de cada persona en OpenAI, Google Gemini y Anthropic Claude. Cada rol (revisor 1, revisor 2, árbitro) usa un proveedor y un modelo elegidos por fase. Las claves se guardan solo en el navegador (la de OpenAI es la misma que usa el resto del catálogo) y no se incluyen en ningún fichero exportado. No hace falta servidor. Cada persona paga su propio uso.

## Ejemplo sin clave

El botón «Ver un ejemplo» carga 14 registros y 5 textos **ficticios** y ejecuta todo el proceso con revisores **simulados por un guion**: no son artículos reales ni modelos de IA. Incluye a propósito un desacuerdo que decide el árbitro, un recuento erróneo de un revisor que el árbitro corrige y una cita que no está en el texto y se marca con ⚠.

## Privacidad

Registros, decisiones y textos completos se guardan solo en el navegador (IndexedDB). Hacia los proveedores de IA salen el título y el resumen de cada registro y, después, el texto completo de los estudios incluidos, con correos, teléfonos e identificadores enmascarados. Antes de cada fase se muestra una muestra de lo que sale y una estimación de llamadas, y hay que confirmarlo. También se hacen consultas a PubMed, OpenAlex, Europe PMC y Unpaywall.

## Límites

- **La IA decide por defecto.** La verificación humana es opcional: si no se hace, el texto de métodos lo declara como limitación.
- κ mide el acuerdo entre modelos, no su acierto. Dos modelos pueden equivocarse a la vez y de la misma manera. Con pocos estudios incluidos, κ puede ser bajo aunque el acuerdo sea alto.
- La recuperación automática del texto completo solo llega a los artículos de acceso abierto de Europe PMC, porque los navegadores bloquean la descarga directa de la mayoría de los PDF. No hay OCR local: los PDF escaneados solo se pueden transcribir con un modelo con visión, y las citas se comprueban contra esa transcripción.
- Solo se leen el texto y las tablas que se extraen del PDF; las figuras no se analizan.
- El texto de cada estudio se recorta al límite de caracteres configurado: lo que quede fuera no lo ven los revisores.
- Los mensajes de error de los proveedores aparecen en español.
- RoB 2 se valora para el efecto de la asignación. La herramienta de sesgo depende del diseño que extraigan los revisores: si lo clasifican mal, hay que cambiarla a mano.
- No sustituye el juicio de quien firma la revisión ni sustituye una revisión sistemática hecha con doble revisión humana.

## Pruebas

`tests/nucleo_test.js` (lógica pura, en Node) y `tests/smoke_test.py` (navegador con IA simulada de los tres proveedores). La versión 2.0 se probó en Chromium con el ejemplo, con los tres proveedores simulados (incluidos los tokens), con la migración de proyectos de la 1.0 y en español, inglés y portugués; **no se ha probado con claves reales**.

## Autoría

Fernando Borrás Rocher y Adriana López-Pineda (Universidad Miguel Hernández de Elche).

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573) · Adriana López-Pineda [0000-0002-2117-0178](https://orcid.org/0000-0002-2117-0178)

## Cómo citar

Borrás Rocher, F. y López-Pineda, A. (2026). *PRISMIA* (v2.0.0) [Software]. DOI: [10.5281/zenodo.23144807](https://doi.org/10.5281/zenodo.23144807)

## Licencia

MIT. Véase [LICENSE](LICENSE).
