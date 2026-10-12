# PLAN.md · Webinar

**Título:** Del vibe coding a la ciencia reproducible: Markdown, Quarto y pruebas para una investigación transparente
**Duración:** 40 min (38 de contenido + 2 de colchón)
**Ponente:** Guillermo Barrios del Valle · IER-UNAM
**Formato:** diapositivas Quarto (revealjs), mismo estilo del deck anterior (reutiliza `.pregunta`, `.cards`, `.tiny`, `.notes`)

---

## Objetivo

Que la audiencia salga con tres cosas:

1. Una definición honesta de vibe coding (Karpathy la acuñó **y la acotó** a proyectos desechables).
2. Un flujo de trabajo concreto: Markdown + Quarto + Python + pruebas + estructura de carpetas.
3. Un método para trabajar con agentes: `PLAN.md` como contrato, ejecución por partes, humano que revisa y firma.

**Arco narrativo:** del caos (proyecto desorganizado por tanto prompt) al orden (spec-driven con Quarto), terminando en que el criterio humano es irreemplazable.

---

## Estructura y tiempos

### Bloque 1 · ¿Qué es el vibe coding? (4 min, diap. 2–4)

Se reutilizan las tres diapositivas del deck anterior, con la **narrativa invertida**: primero la definición ("¿Qué es el vibe coding?", sin el "entonces"), luego el tuit, y al final quién es Karpathy — se nombra la práctica antes de darle autor y peso:

- **¿Quién es Andrej Karpathy?** — credenciales: doctorado en Stanford con Fei-Fei Li, CS231n, miembro fundador de OpenAI, director de IA en Tesla, "Software 2.0", Zero to Hero, Eureka Labs. El punto: no es un influencer; construyó sistemas de producción y enseña desde los fundamentos, por eso su definición pesa.
- **Febrero de 2025: el tuit** — citas: *"fully give in to the vibes, embrace exponentials, and forget that the code even exists"*, *"I 'Accept All' always, I don't read the diffs anymore"*. Subrayar: él mismo lo pensó para **proyectos desechables** de fin de semana.
- **Definición operativa** — "Describir en lenguaje natural lo que quieres, aceptar lo que el modelo escribe **sin leerlo**, y corregir por prueba y error hasta que 'funciona'."

Tras Karpathy, **diapositiva pivote**: "Esto no es una manera adecuada de hacer ciencia… pero el concepto se puede transformar" — aprovechar la "inteligencia" de los LLM, cambiar la confianza ciega por estructura reproducible y verificación humana. Luego, cierre del bloque con el separador azul que **reformula la pregunta**: la cuestión ya no es *si* haría el análisis de un artículo o tesis con IA, sino ***cómo*** asegurar la reproducibilidad y la certeza de que el análisis está bien ejecutado. Ese "cómo" es el hilo del resto de la charla.

### Bloque 2 · El proyecto desorganizado (4 min, diap. 5–6)

La anti-demo. Captura (o recorrido en vivo) de una carpeta real tras semanas de prompts:

- `Untitled27.ipynb`, `analisis_final_v3_AHORASI.py`, `datos_limpios(2).csv`, figuras sueltas en la raíz, sin README, datos crudos sobrescritos, sin entorno fijado (¿qué versión de pandas era?).
- Síntomas medibles: nadie —ni tú en tres meses— puede regenerar la figura 3; el resultado depende del orden en que corriste las celdas; el revisor 2 pregunta de dónde salió un número y no hay respuesta.
- **Mensaje:** el problema no es la IA, es la falta de estructura. Cada prompt agrega entropía si no hay contrato.

*TODO: decidir si la captura es de un proyecto propio anonimizado o una carpeta recreada para la demo.*

### Bloque 3 · LA PROPUESTA + Markdown y Quarto (4 min)

Abre con un separador azul, **LA PROPUESTA**: *Markdown + Quarto + Python + git — una narrativa computacional, versionada, en un ambiente virtual aislado*. Es la respuesta en una línea al caos del bloque 2; las definiciones que siguen la desarrollan.

- **Markdown** (una diapositiva): lenguaje de marcado ligero (John Gruber, 2004). Texto plano legible por humanos con marcas mínimas (`#`, `**`, `-`, `[]()`). Separa contenido de formato. Es el idioma nativo de GitHub **y de los LLMs** (los agentes leen y escriben Markdown: README, PLAN.md, AGENTS.md). Versionable con git: los diffs se leen, a diferencia de un `.docx`.
- **Quarto** (una diapositiva): sistema de publicación científica y técnica de código abierto (Posit, sucesor de R Markdown). Un `.qmd` = Markdown + bloques de código **ejecutables** (Python, R, Julia) + opciones documentadas en YAML. Un mismo archivo fuente rinde a HTML, PDF (LaTeX), docx, diapositivas revealjs, sitios web y libros. Remate: *"estas diapositivas están hechas en Quarto"*.
- **Por qué juntos** (una diapositiva): texto, código, opciones y resultados viven en un solo archivo versionado → **el análisis ES el documento**. No hay "el script por un lado y el Word por otro".
- **¿Qué es el versionado?** (una diapositiva, con el meme "El proceso de tu tesis"): todos versionamos a mano (`tesis_final_por_fin.docx`); git guarda la historia completa (qué, cuándo, por qué) y cada commit es un punto de retorno. Precisión: **git es software libre**; GitHub es la plataforma donde se publica, gratuita para la comunidad académica (GitHub Education).

### Bloque 4 · Configuración, estructura y fragmentación (6 min)

- Estructura propuesta (diapositiva con el árbol):

```
proyecto/
├── PLAN.md            # el contrato: qué se va a hacer con los datos
├── AGENTS.md          # reglas del espacio de trabajo para el agente
├── pyproject.toml     # entorno fijado con uv
├── uv.lock
├── data/
│   ├── 001_raw/           # inmutable: los datos crudos nunca se tocan
│   ├── 002_intermediate/  # datos procesados
│   └── 003_results/       # resultados del análisis
├── src/<paquete>/         # funciones importables, cada una con prueba
├── tests/                 # pruebas físicas (pytest)
├── analysis/
│   └── index.qmd          # la narrativa computacional (un solo qmd)
└── tex/                   # opcional: main.tex (el artículo) + figuras y
                           # tablas .tex que index.qmd declara finales
```

- **Aviso importante en voz alta:** esta estructura es una *narrativa*, no una receta. Se adapta al proyecto y a los datos (series de tiempo, imágenes, encuestas… cada uno pide carpetas distintas). Lo invariante: datos crudos intocables, entorno fijado, funciones con pruebas, análisis en `.qmd`.
- Contraste lado a lado: la carpeta caótica del Bloque 2 vs. esta.
- **La fragmentación** (diapositiva propia, reemplaza al diagrama de flujo): se analiza en una compu → se genera la figura → se manda por correo → se agrega a mano al `.tex` → se publica. Cada flecha es manual y sin registro; todos tenemos la anécdota del paper con la figura incorrecta. La cura: repositorio compartido y cada flecha convertida en comando.
- `PLAN.md` concentra contrato, bitácora de IA y decisiones de la persona autora (no hay `docs/`).
- **Colaboración**: todo vive en un repositorio compartido (GitHub); datos, código y resultados no se fragmentan entre computadoras/personas — cualquier colaborador hace `git clone` + `uv sync --locked` y reproduce.

### Bloque 5 · ¿Qué es un agente? Local o en la nube (4 min)

Se presenta al **actor antes que el contrato**: definición y dónde corre, para que el bloque siguiente (PLAN.md, demos) no use el término sin definirlo.

- **Definición:** un agente = LLM + bucle de herramientas (leer y escribir archivos, ejecutar comandos, correr pruebas) que itera hasta cumplir una tarea. A diferencia del chat, **actúa sobre tu repositorio**: por eso necesita reglas (`AGENTS.md`) y un contrato (`PLAN.md`) — anuncia el bloque siguiente.
- **En la nube:** mi caso, Claude (Claude Code). Modelos más capaces, sin hardware propio, costo por uso; los datos salen de tu máquina (considerarlo con datos sensibles).
- **Local:** también uso Qwen3.8 27B (`qwen3.8:27b-mlx`, vía MLX en Apple Silicon). Ventajas: los datos no salen, sin costo por token. **Restricción honesta:** se necesita hardware capaz (GPU con VRAM suficiente o Mac con memoria unificada amplia) y el modelo rinde menos que los de frontera.
- Práctica recomendada: nube para el trabajo pesado; local cuando los datos no deben salir o para tareas acotadas.

### Bloque 6 · PLAN.md: el contrato con el agente (7 min)

El corazón de la charla.

- **Qué es:** el documento donde escribes *qué* quieres hacer con los datos **antes** de pedir una sola línea de código: preguntas de investigación, variables y unidades, supuestos, criterios de aceptación.
- **Cómo se escribe con el LLM:** prompt tipo "Escribe `PLAN.md` para [objetivo]. **No escribas código.**" El borrador de la IA puede traer supuestos erróneos; el humano los corrige (unidades, convenciones, casos límite). `git commit` del plan = firma del contrato.
- **Cómo se ejecuta:** parte por parte. Una sección del plan → código → `quarto render` → revisar el diff → commit. Nunca "hazme todo el análisis de una vez".
- **Leer el diff** (diapositiva propia, tras "cómo se ejecuta"): definición (líneas que salen `-` y entran `+`; la unidad de revisión con agentes) y caso ficticio: el agente "simplifica" `ghi_diaria` cambiando `sum()` por `mean()` — corre sin error, números plausibles, solo el diff lo delata (ya no es energía, es potencia promedio).
- **El concepto de prueba** (diapositiva propia): función `calcular_energia(potencia, freq)` que integra potencia [W] → energía [Wh] con frecuencia de resample como parámetro; la prueba usa datos **inventados por la persona autora** con respuesta conocida con papel y lápiz (1200 W constantes × 2 h → 1200 Wh/h, 2400 Wh total; verificado que los asserts pasan). El ground truth lo pone la persona, no el código.
- **Las pruebas son parte del plan** (aquí entra el "pruebas" del título): cada afirmación verificable del plan se convierte en un test físico con pytest — número de filas esperado, variables no negativas, cantidades nulas donde la física lo exige, intervalos de validez de los modelos. Las pruebas físicas detectan lo imposible; la referencia externa detecta lo plausible pero incorrecto.

El ejemplo es el dataset de **ClimaLab** (ver Decisiones tomadas); el PLAN.md de ejemplo se muestra en pantalla con los supuestos erróneos de la IA corregidos por la persona autora.

### Bloque 7 · La transición al spec-driven: de Quarto al artículo (5 min)

- **Nombrar lo que ya hicimos:** PLAN.md + ejecución por partes **es** desarrollo guiado por especificación (*spec-driven development*). La especificación manda; el código la sigue. El agente ejecuta el spec; el humano lo escribe y lo verifica. Es el opuesto exacto del vibe coding del Bloque 1: allá el código no se lee, acá el spec se firma.
- **Quarto como columna vertebral del spec:** todo queda ordenado y auditable — el código a la vista en el documento, las opciones documentadas (YAML y opciones de celda: `echo`, `fig-cap`, `tbl-cap`...), los resultados regenerables con un solo comando.
- **Del `.qmd` a la versión final del artículo** (personalizaciones):
  - El artículo vive en `tex/main.tex`; cuando una figura o tabla se declara **final**, `index.qmd` la guarda en `tex/` (`savefig`, `.to_latex()`) y `main.tex` la importa con `\input{}` / `\includegraphics{}`.
  - El `.qmd` queda como **suplemento reproducible**: cualquier revisor regenera todo con `uv sync --locked && quarto render`.
- Reiterar: la narrativa (PLAN.md + estructura) se adapta a cada proyecto y a sus datos.

### Bloque 8 · Recomendaciones a futuro: paquetes y MCP (3 min, diap. 21–22)

- **Si acumulas muchas funciones** en `src/`, el siguiente paso natural es un **paquete instalable**: documentado, versionado, citable. Caso en casa: **EnerHabitat en PyPI** (`pip install enerhabitat`), del IER-UNAM — de scripts de investigación a herramienta que cualquiera instala y cuyas versiones quedan fijadas en el `uv.lock` de quien la usa.
- **MCP (Model Context Protocol):** estándar abierto para exponer tus funciones y datos como herramientas que cualquier agente puede invocar directamente. Tiene sentido cuando tu código ya es biblioteca: el agente deja de reescribir tus cálculos y pasa a *llamarlos*. Es recomendación a futuro, no requisito.

### Bloque 9 · Cierre: human in the loop (2 min, diap. 23–24)

- **Saber programar es la licencia de conducir:** sin ello no puedes auditar lo que el agente escribe, ni entender el problema, ni guiar la investigación. La IA sube el piso; el techo — criterio de dominio, verificación, firma — sigue siendo humano.
- Retomar el arco: vibe coding (código sin leer) → ciencia reproducible (spec firmado, código auditado, pruebas, reproducción desde cero). El humano firma; la IA no.
- Diapositiva final con pregunta para el Q&A: *¿Qué parte de tu próximo análisis **no** delegarías, y por qué?* + contacto (gbv@ier.unam.mx).

---

## Presupuesto de tiempo

| Bloque | Tema | Min |
|---|---|---|
| 1 | Vibe coding: Karpathy, tuit, definición | 4 |
| 2 | El proyecto desorganizado | 4 |
| 3 | Markdown y Quarto | 4 |
| 4 | Configuración, estructura y fragmentación | 6 |
| 5 | Agentes: qué son, nube vs. local | 4 |
| 6 | PLAN.md, diff y pruebas | 7 |
| 7 | Spec-driven: de Quarto al artículo | 5 |
| 8 | Paquetes y MCP | 3 |
| 9 | Cierre: human in the loop | 2 |
| | Colchón | 2 |
| | **Total** | **40** |

---

## Decisiones tomadas

- [x] Dataset: **ClimaLab** — `data/ClimaLab_2026-04-08_2026-08-05 (1).parquet` (172 433 filas minutales, abr–ago 2026; tdb, rh, ghi/dni/dhi, ws/wd, p_atm, rain_acc, uv). Verificado: GHI nocturna = 0, 2 huecos > 1 min, NaN en p_atm (17 %) y rh.
- [x] Demos: **en vivo**, tres — Demo 1 (≤2 min, bloque PLAN.md): pedir el PLAN.md al agente y corregir supuestos; Demo 2 (≤3 min, bloque PLAN.md): ejecutar la sección de temperatura del plan con un **tabset** de dos pestañas (promedio diario vs. *running mean* de 30 días) para decidir con cuál quedarse conservando ambas opciones en el documento para el equipo; Demo 3 (≤2 min, bloque spec-driven): generar la tabla de **temperatura promedio mensual** y mandarla como `.tex` a `tex/` para el `\input{}` de `main.tex`. El desorden del bloque 2 se muestra solo con los listados en las diapositivas (sin demo).
- [x] Publicación: `quarto publish gh-pages` (por eso el deck es `index.qmd`).
- [x] Bloque 4 ahora incluye configuración con `uv` (ambiente virtual, lock, y que PLAN.md declare "instalar con uv") — 6 min; colchón queda en 1 min.

- [x] Recorte de tiempo (revisión crítica): **comentadas** en `index.qmd` (no borradas) "Los síntomas", "Una narrativa, no una receta" y "Quarto mantiene todo ordenado y auditable". Sus mensajes clave se rescataron: el remate "el problema no es la IA…" pasó a "El nivel sutil"; la línea "narrativa, no receta + invariantes" pasó al pie de "Estructura propuesta"; las opciones de celda se dicen de palabra en la Demo 3. Deck: 30 diapositivas, 36 min + 4 de colchón. MCP queda como fusible en vivo.

## Pendientes por confirmar

- [x] Modelo local: `qwen3.8:27b-mlx` (Qwen3.8 27B vía MLX en Apple Silicon).
- [x] Imágenes copiadas: `img/karpathy.png` e `img/karpathy-tweet.png`.
- [x] Fecha del webinar: jueves 16 de octubre de 2026 (ya en el YAML).
- [x] Paquete EnerHabitat en PyPI: `enerhabitat` (https://pypi.org/project/enerhabitat/).
- [x] Proyecto desorganizado real para la Demo 1: `~/paper_enerhabitat` — ilustra el **nivel sutil** del desorden (scripts numerados 00–05/07/90–92, pero `results/` con `_descartado_`, logs `_shard_`, datos de entrada `.epw`, 4 versiones de `comparison_table` y 180 CSVs en `series/`). El bloque 2 ahora tiene dos niveles: el caos evidente (listado ficticio) y este, con diapositiva propia de dos columnas.
- [ ] QRs: repo del ejemplo y diapositivas en GitHub Pages (tras `quarto publish gh-pages`).
