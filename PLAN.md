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

Se **reutilizan tal cual** tres diapositivas del deck anterior:

- **¿Quién es Andrej Karpathy?** — credenciales: doctorado en Stanford con Fei-Fei Li, CS231n, miembro fundador de OpenAI, director de IA en Tesla, "Software 2.0", Zero to Hero, Eureka Labs. El punto: no es un influencer; construyó sistemas de producción y enseña desde los fundamentos, por eso su definición pesa.
- **Febrero de 2025: el tuit** — citas: *"fully give in to the vibes, embrace exponentials, and forget that the code even exists"*, *"I 'Accept All' always, I don't read the diffs anymore"*. Subrayar: él mismo lo pensó para **proyectos desechables** de fin de semana.
- **Definición operativa** — "Describir en lenguaje natural lo que quieres, aceptar lo que el modelo escribe **sin leerlo**, y corregir por prueba y error hasta que 'funciona'."

Cierre del bloque con el separador azul que **reformula la pregunta**: la cuestión ya no es *si* haría el análisis de un artículo o tesis con IA, sino ***cómo*** asegurar la reproducibilidad y la certeza de que el análisis está bien ejecutado. Ese "cómo" es el hilo del resto de la charla.

### Bloque 2 · El proyecto desorganizado (4 min, diap. 5–6)

La anti-demo. Captura (o recorrido en vivo) de una carpeta real tras semanas de prompts:

- `Untitled27.ipynb`, `analisis_final_v3_AHORASI.py`, `datos_limpios(2).csv`, figuras sueltas en la raíz, sin README, datos crudos sobrescritos, sin entorno fijado (¿qué versión de pandas era?).
- Síntomas medibles: nadie —ni tú en tres meses— puede regenerar la figura 3; el resultado depende del orden en que corriste las celdas; el revisor 2 pregunta de dónde salió un número y no hay respuesta.
- **Mensaje:** el problema no es la IA, es la falta de estructura. Cada prompt agrega entropía si no hay contrato.

*TODO: decidir si la captura es de un proyecto propio anonimizado o una carpeta recreada para la demo.*

### Bloque 3 · LA PROPUESTA + Markdown y Quarto (4 min)

Abre con un separador azul, **LA PROPUESTA**: *Markdown + Quarto + Python — una narrativa computacional en un ambiente virtual aislado*. Es la respuesta en una línea al caos del bloque 2; las definiciones que siguen la desarrollan.

- **Markdown** (una diapositiva): lenguaje de marcado ligero (John Gruber, 2004). Texto plano legible por humanos con marcas mínimas (`#`, `**`, `-`, `[]()`). Separa contenido de formato. Es el idioma nativo de GitHub **y de los LLMs** (los agentes leen y escriben Markdown: README, PLAN.md, AGENTS.md). Versionable con git: los diffs se leen, a diferencia de un `.docx`.
- **Quarto** (una diapositiva): sistema de publicación científica y técnica de código abierto (Posit, sucesor de R Markdown). Un `.qmd` = Markdown + bloques de código **ejecutables** (Python, R, Julia) + opciones documentadas en YAML. Un mismo archivo fuente rinde a HTML, PDF (LaTeX), docx, diapositivas revealjs, sitios web y libros. Remate: *"estas diapositivas están hechas en Quarto"*.
- **Por qué juntos** (una diapositiva): texto, código, opciones y resultados viven en un solo archivo versionado → **el análisis ES el documento**. No hay "el script por un lado y el Word por otro".

### Bloque 4 · El flujo de trabajo: estructura + Quarto + Python (5 min, diap. 10–12)

- Estructura propuesta (diapositiva con el árbol):

```
proyecto/
├── PLAN.md            # el contrato: qué se va a hacer con los datos
├── AGENTS.md          # reglas del espacio de trabajo para el agente
├── pyproject.toml     # entorno fijado con uv
├── uv.lock
├── data/
│   ├── 001_raw/       # inmutable + FUENTES.md (URL, fecha, sha256)
│   └── 002_interim/
├── src/<paquete>/     # funciones importables, cada una con prueba
├── tests/             # pruebas físicas (pytest)
├── analysis/          # .qmd numerados: 01_eda.qmd, 02_modelo.qmd, ...
├── figures/           # salida de figuras con nombres estables
├── outputs/tex/       # tablas .tex para el artículo
└── docs/              # bitácora de IA, decisiones
```

- **Aviso importante en voz alta:** esta estructura es una *narrativa*, no una receta. Se adapta al proyecto y a los datos (series de tiempo, imágenes, encuestas… cada uno pide carpetas distintas). Lo invariante: datos crudos intocables, entorno fijado, funciones con pruebas, análisis en `.qmd`.
- Contraste lado a lado: la carpeta caótica del Bloque 2 vs. esta.
- El flujo: datos crudos → funciones en `src/` (probadas) → `.qmd` que importan de `src/` → `quarto render` → figuras y tablas a sus carpetas → artículo.
- **Colaboración**: todo vive en un repositorio compartido (GitHub); datos, código y resultados no se fragmentan entre computadoras/personas — cualquier colaborador hace `git clone` + `uv sync --locked` y reproduce.

### Bloque 5 · PLAN.md: el contrato con el agente (7 min, diap. 13–15)

El corazón de la charla.

- **Qué es:** el documento donde escribes *qué* quieres hacer con los datos **antes** de pedir una sola línea de código: preguntas de investigación, variables y unidades, supuestos, criterios de aceptación.
- **Cómo se escribe con el LLM:** prompt tipo "Escribe `PLAN.md` para [objetivo]. **No escribas código.**" El borrador de la IA trae supuestos erróneos; el humano los corrige (unidades, convenciones, casos límite). `git commit` del plan = firma del contrato.
- **Cómo se ejecuta:** parte por parte. Una sección del plan → código → `quarto render` → revisar el diff → commit. Nunca "hazme todo el análisis de una vez".
- **Las pruebas son parte del plan** (aquí entra el "pruebas" del título): cada afirmación verificable del plan se convierte en un test físico con pytest — número de filas esperado, variables no negativas, cantidades nulas donde la física lo exige, intervalos de validez de los modelos. Las pruebas físicas detectan lo imposible; la referencia externa detecta lo plausible pero incorrecto.

*TODO: decidir si el ejemplo es el caso Torreón (EPW + confort adaptativo + GHI, reusando material del deck de 50 min) u otro dataset; el PLAN.md de ejemplo debe mostrarse en pantalla con un error de la IA corregido por el humano.*

### Bloque 6 · ¿Qué es un agente? Local o en la nube (4 min, diap. 16–17)

- **Definición:** un agente = LLM + bucle de herramientas (leer y escribir archivos, ejecutar comandos, correr pruebas) que itera hasta cumplir una tarea. A diferencia del chat, **actúa sobre tu repositorio**: por eso necesita reglas (`AGENTS.md`) y un contrato (`PLAN.md`).
- **En la nube:** mi caso, Claude (Claude Code). Modelos más capaces, sin hardware propio, costo por uso; los datos salen de tu máquina (considerarlo con datos sensibles).
- **Local:** también uso Qwen3 27B *(TODO: confirmar nombre y tamaño exactos del modelo; las tallas publicadas de Qwen3 son 14B/32B/30B-A3B — ¿o es Gemma 3 27B?)* servido localmente. Ventajas: los datos no salen, sin costo por token. **Restricción honesta:** se necesita hardware capaz (GPU con VRAM suficiente o Mac con memoria unificada amplia) y el modelo rinde menos que los de frontera.
- Práctica recomendada: nube para el trabajo pesado; local cuando los datos no deben salir o para tareas acotadas.

### Bloque 7 · La transición al spec-driven: de Quarto al artículo (5 min, diap. 18–20)

- **Nombrar lo que ya hicimos:** PLAN.md + ejecución por partes **es** desarrollo guiado por especificación (*spec-driven development*). La especificación manda; el código la sigue. El agente ejecuta el spec; el humano lo escribe y lo verifica. Es el opuesto exacto del vibe coding del Bloque 1: allá el código no se lee, acá el spec se firma.
- **Quarto como columna vertebral del spec:** todo queda ordenado y auditable — el código a la vista en el documento, las opciones documentadas (YAML y opciones de celda: `echo`, `fig-cap`, `tbl-cap`...), los resultados regenerables con un solo comando.
- **Del `.qmd` a la versión final del artículo** (personalizaciones):
  - Figuras: se escriben a `figures/` con nombres estables → el manuscrito las incluye desde ahí.
  - Tablas: se exportan como `.tex` a `outputs/tex/` (`to_latex()`, GT) → `\input{}` en el LaTeX de la revista.
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
| 4 | Estructura y flujo de trabajo | 5 |
| 5 | PLAN.md y pruebas | 7 |
| 6 | Agentes: local vs. nube | 4 |
| 7 | Spec-driven: de Quarto al artículo | 5 |
| 8 | Paquetes y MCP | 3 |
| 9 | Cierre: human in the loop | 2 |
| | Colchón | 2 |
| | **Total** | **40** |

---

## Decisiones tomadas

- [x] Dataset: **ClimaLab** — `data/ClimaLab_2026-04-08_2026-08-05 (1).parquet` (172 433 filas minutales, abr–ago 2026; tdb, rh, ghi/dni/dhi, ws/wd, p_atm, rain_acc, uv). Verificado: GHI nocturna = 0, 2 huecos > 1 min, NaN en p_atm (17 %) y rh.
- [x] Demos: **en vivo** — Demo 1 (bloque 2, ≤2 min): recorrido por proyecto desorganizado real; Demo 2a/2b (bloque 5, ≤5 min): PLAN.md con el agente → ejecutar una sección → pytest → render.
- [x] Publicación: `quarto publish gh-pages` (por eso el deck es `index.qmd`).
- [x] Bloque 4 ahora incluye configuración con `uv` (ambiente virtual, lock, y que PLAN.md declare "instalar con uv") — 6 min; colchón queda en 1 min.

## Pendientes por confirmar

- [x] Modelo local: `qwen3.8:27b-mlx` (Qwen3.8 27B vía MLX en Apple Silicon).
- [x] Imágenes copiadas: `img/karpathy.png` e `img/karpathy-tweet.png`.
- [x] Fecha del webinar: jueves 16 de octubre de 2026 (ya en el YAML).
- [x] Paquete EnerHabitat en PyPI: `enerhabitat` (https://pypi.org/project/enerhabitat/).
- [ ] Proyecto desorganizado real para la Demo 1 (Guillermo lo pasa en su momento).
- [ ] QRs: repo del ejemplo y diapositivas en GitHub Pages (tras `quarto publish gh-pages`).
