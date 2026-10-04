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
- **Informe.** Diagrama de flujo PRISMA 2020 (SVG), texto de métodos en el idioma de la interfaz, informe en Word, tablas en CSV (ZIP), registro de uso de la IA (fase, rol, proveedor, modelo y número de llamadas; nunca el contenido) y exportación e importación del proyecto en JSON.

## Qué comprueba el código y qué hace la IA

La IA propone; el código decide y comprueba.

- Decide el acuerdo entre revisores, cuándo interviene el árbitro y la decisión final, y deja constancia de quién decidió cada cosa.
- Calcula el kappa de Cohen, el acuerdo observado y PABAK.
- Calcula todos los contadores del diagrama PRISMA.
- Comprueba que cada cita de la extracción y del riesgo de sesgo aparece en el texto del estudio, y que las cifras extraídas también aparecen. Lo que no se puede comprobar se marca con ⚠; no se oculta.
- Calcula el juicio global de riesgo de sesgo.
- Se detiene solo si hay varios errores seguidos (clave, saldo, modelo sin acceso).

## Cómo se usa la IA

Con la clave propia de cada persona en OpenAI, Google Gemini y Anthropic Claude. Cada rol (revisor 1, revisor 2, árbitro) usa un proveedor y un modelo elegidos por fase. Las claves se guardan solo en el navegador (la de OpenAI es la misma que usa el resto del catálogo) y no se incluyen en ningún fichero exportado. No hace falta servidor. Cada persona paga su propio uso.

## Ejemplo sin clave

El botón «Ver un ejemplo» carga 14 registros y 5 textos **ficticios** y ejecuta todo el proceso con revisores **simulados por un guion**: no son artículos reales ni modelos de IA. Incluye a propósito un desacuerdo que decide el árbitro, un recuento erróneo de un revisor que el árbitro corrige y una cita que no está en el texto y se marca con ⚠.

## Privacidad

Registros, decisiones y textos completos se guardan solo en el navegador (IndexedDB). Hacia los proveedores de IA salen el título y el resumen de cada registro y, después, el texto completo de los estudios incluidos, con correos, teléfonos e identificadores enmascarados. Antes de cada fase se muestra una muestra de lo que sale y una estimación de llamadas, y hay que confirmarlo. También se hacen consultas a PubMed, OpenAlex, Europe PMC y Unpaywall.

## Límites

- **Ninguna decisión pasa por revisión humana.** Muchas revistas esperan verificación humana de parte del cribado; el texto de métodos lo declara como limitación.
- κ mide el acuerdo entre modelos, no su acierto. Dos modelos pueden equivocarse a la vez y de la misma manera. Con pocos estudios incluidos, κ puede ser bajo aunque el acuerdo sea alto.
- La recuperación automática del texto completo solo llega a los artículos de acceso abierto de Europe PMC, porque los navegadores bloquean la descarga directa de la mayoría de los PDF. No hay OCR: los PDF escaneados no sirven.
- Solo se leen el texto y las tablas que se extraen del PDF; las figuras no se analizan.
- El texto de cada estudio se recorta al límite de caracteres configurado: lo que quede fuera no lo ven los revisores.
- Los mensajes de error de los proveedores aparecen en español.
- RoB 2 se valora para el efecto de la asignación. La herramienta de sesgo depende del diseño que extraigan los revisores: si lo clasifican mal, hay que cambiarla a mano.
- No sustituye el juicio de quien firma la revisión ni sustituye una revisión sistemática hecha con doble revisión humana.

## Pruebas

`tests/nucleo_test.js` (lógica pura, en Node) y `tests/smoke_test.py` (navegador con IA simulada de los tres proveedores). Se probó con PubMed, Europe PMC y los tres proveedores simulados; **no se ha probado con claves reales**.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche).

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573)

## Cómo citar

Borrás Rocher, F. (2026). *PRISMIA* (v1.0.0) [Software]. DOI: [10.5281/zenodo.23144807](https://doi.org/10.5281/zenodo.23144807)

## Licencia

MIT. Véase [LICENSE](LICENSE).
