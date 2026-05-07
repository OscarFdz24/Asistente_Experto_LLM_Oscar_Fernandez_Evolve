# Asistente Experto en Analítica de Fútbol Profesional

Proyecto final de IA Generativa orientado a construir un asistente experto capaz de responder preguntas de scouting y rendimiento futbolistico usando Gemini, RAG, ChromaDB, LangGraph y consultas SQL sobre datos reales de jugadores.

El objetivo principal es combinar dos tipos de conocimiento:

- Documentacion conceptual sobre metricas avanzadas de futbol, indexada en una base vectorial.
- Datos estructurados de jugadores, consultados mediante SQL generado por el LLM y ejecutado sobre un CSV con pandas/SQLite.

## Dominio elegido

El dominio del asistente es la analitica de futbol profesional aplicada a scouting, comparacion de jugadores y evaluacion de rendimiento.

Este dominio es adecuado para un proyecto RAG porque combina:

- Conceptos tecnicos que requieren explicacion, como `xG`, `xAG`, `PSxG+/-`, `SCA`, `PrgC` o `SoT%`.
- Datos tabulares reales de jugadores, equipos, ligas, posiciones y metricas.
- Preguntas mixtas donde el usuario necesita tanto interpretacion futbolistica como calculos exactos.

Ejemplos de preguntas que puede resolver:

- Que significa xG y como se interpreta en scouting?
- Cuales son los jugadores con mas xG?
- Que delantero tiene mayor precision de tiro?
- Compara a Mohamed Salah y Raphinha usando xG, xAG, PrgC y SCA.
- Que significa PSxG+/- para evaluar porteros?

## Arquitectura del sistema

El notebook implementa un pipeline completo de RAG y agente conversacional:

```text
Pregunta del usuario
        |
        v
Retriever ChromaDB
        |
        v
Contexto conceptual de metricas
        |
        v
Gemini genera SQL sobre el CSV
        |
        v
SQLite / pandas ejecuta la consulta
        |
        v
LangGraph combina RAG + SQL + memoria
        |
        v
Respuesta final con Gemini
```

Componentes principales:

- **LLM**: Google Gemini.
- **Embeddings**: Gemini Embeddings.
- **Vector store**: ChromaDB.
- **Framework RAG/Agentes**: LangChain y LangGraph.
- **Memoria**: `MemorySaver` de LangGraph junto con `HumanMessage` y `AIMessage`.
- **Datos tabulares**: CSV de jugadores convertido a tabla SQL temporal con SQLite.
- **Entorno**: Jupyter Notebook / VS Code / Google Colab.

## Estructura del proyecto

```text
.
|-- data/
|   |-- 01_metricas_base_tiro_xg.pdf
|   |-- 02_metricas_pase_creacion_posesion.pdf
|   |-- 03_metricas_defensa_porteros_modelado_rag.pdf
|   |-- glosario_metricas_futbol.md
|   `-- players_data-2024_2025.csv
|-- notebooks/
|   `-- 01_asistente_experto_analitica_futbol_rag_gemini.ipynb
|-- chroma_db/              # Generado localmente, no se sube a GitHub
|-- .env                    # Claves privadas, no se sube a GitHub
|-- .gitignore
`-- README.md
```

## Requisitos

Necesitas Python 3.10 o superior y una API key de Gemini.

Dependencias principales:

```bash
pip install langchain langchain-core langchain-community langchain-google-genai langchain-text-splitters langgraph chromadb python-dotenv pypdf pandas tabulate
```

Dependencias usadas por el proyecto:

- `langchain`
- `langchain-google-genai`
- `langchain-community`
- `langchain-text-splitters`
- `langgraph`
- `chromadb`
- `python-dotenv`
- `pypdf`
- `pandas`
- `tabulate`
- `sqlite3` incluido en Python

Justificación de las dependencias:

- **`langchain` / `langchain-core`**: se usan como base para estructurar componentes del sistema, documentos, mensajes y llamadas al modelo.
- **`langchain-google-genai`**: permite conectar el proyecto con Gemini tanto para generacion de respuestas como para embeddings.
- **`langchain-community`**: proporciona integraciones comunitarias usadas en el notebook, especialmente la conexion con ChromaDB.
- **`langchain-text-splitters`**: permite dividir los PDFs en chunks antes de crear embeddings, paso necesario en el pipeline RAG.
- **`langgraph`**: se usa para construir el agente como un grafo de pasos: recuperar contexto, consultar datos y generar respuesta.
- **`chromadb`**: almacena los embeddings de los documentos PDF y permite recuperar contexto por similitud semantica.
- **`python-dotenv`**: carga las variables del archivo `.env`, evitando escribir la API key directamente en el codigo.
- **`pypdf`**: permite leer y extraer texto de los PDFs usados como base documental.
- **`pandas`**: carga y prepara el CSV de jugadores, que contiene los datos estructurados sobre rendimiento.
- **`sqlite3`**: permite convertir el CSV en una tabla SQL temporal para ejecutar consultas generadas por Gemini de forma sencilla.
- **`tabulate`**: permite mostrar resultados de pandas en formato tabla dentro de los prompts y salidas del notebook.

Aunque `pandas`, `sqlite3`, `pypdf`, `python-dotenv` y `tabulate` no son librerias de IA generativa, son necesarias para que el sistema sea útil de extremo a extremo: cargar documentos, proteger claves, consultar datos reales y presentar resultados de forma interpretable.

## Configuración de la API key

El proyecto usa variables de entorno para evitar hardcodear claves privadas.

Crea un archivo `.env` en la raiz del proyecto con este formato:

```env
GOOGLE_API_KEY=tu_api_key_de_gemini
GEMINI_MODEL=gemini-2.5-flash
GEMINI_EMBEDDING_MODEL=gemini-embedding-001
```

Importante:

- No subas `.env` a GitHub.
- `.env` ya debe estar incluido en `.gitignore`.
- Si se agota la cuota de Gemini, el notebook puede fallar temporalmente con errores `429 RESOURCE_EXHAUSTED`.

## Como ejecutar el notebook

Abre el notebook:

```text
notebooks/01_asistente_experto_analitica_futbol_rag_gemini.ipynb
```

Orden recomendado:

1. Ejecutar instalacion/imports/configuracion.
2. Cargar `.env`, Gemini, CSV y PDFs.
3. Preparar documentos PDF y hacer chunking.
4. Crear ChromaDB solo la primera vez.
5. Si ChromaDB ya existe, saltar la indexacion y usar la celda de recarga.
6. Probar el retriever.
7. Probar RAG basico.
8. Crear la herramienta SQL sobre el CSV.
9. Compilar el agente final con LangGraph y memoria.
10. Ejecutar las pruebas o el chat interactivo.

Nota sobre ChromaDB:

- La primera indexacion crea la carpeta `chroma_db/`.
- Si ya existe, no es necesario volver a generar embeddings.
- Para ahorrar cuota, se recomienda saltar la celda de indexacion y ejecutar la celda de recarga de la coleccion existente.

## System prompt

El asistente se configura como un experto en analitica de futbol profesional:

```text
Eres un asistente experto en analitica de futbol profesional,
especializado en scouting, rendimiento de jugadores y metricas avanzadas.
```

El prompt define que el agente debe usar dos fuentes:

1. Contexto recuperado desde ChromaDB para explicar metricas y conceptos.
2. Resultados calculados desde el CSV para rankings, filtros numericos y comparaciones exactas.

Tambien incluye reglas de comportamiento:

- Responder en español con tono claro, profesional y didactico.
- No inventar datos de jugadores ni valores numericos.
- Indicar cuando no hay informacion suficiente.
- Explicar métricas relevantes para interpretar la respuesta.
- Comparar jugadores teniendo en cuenta posición, minutos, liga y rol.
- No hacer predicciones futuras como certezas.
- No dar recomendaciones de apuestas.

## Justificacion del system prompt

El system prompt esta disenado para reducir alucinaciones y hacer que el asistente use correctamente sus fuentes.

Las decisiones principales son:

- **Rol especifico**: se limita el asistente al dominio de analitica de futbol para mantener respuestas coherentes.
- **Uso explicito de fuentes**: obliga a diferenciar entre conocimiento conceptual recuperado por RAG y datos exactos calculados desde el CSV.
- **Limitacion ante falta de datos**: evita que Gemini invente estadisticas cuando la consulta SQL no devuelve informacion.
- **Criterio futbolistico**: las comparaciones no se hacen solo por una metrica aislada, sino considerando posicion, minutos, competicion y rol.
- **Tono didactico**: facilita que el proyecto sea comprensible en un contexto academico.

## Funcionalidades implementadas

- Ingesta de documentos PDF.
- Chunking con `RecursiveCharacterTextSplitter`.
- Embeddings con Gemini.
- Persistencia en ChromaDB.
- Recuperacion semantica de contexto.
- Generacion de respuestas con Gemini.
- Consulta de CSV completo mediante SQL generado por prompting.
- Ejecucion segura de SQL solo con consultas `SELECT`.
- Agente final con LangGraph.
- Memoria conversacional con `MemorySaver`.
- Chat interactivo en notebook.

## Seguridad y buenas practicas

- Las claves API se cargan desde `.env`.
- La base ChromaDB se genera localmente y no se versiona.
- La SQL generada por Gemini se valida antes de ejecutarse.
- Solo se permiten consultas `SELECT`.
- Se bloquean instrucciones como `INSERT`, `UPDATE`, `DELETE`, `DROP`, `ALTER` o `CREATE`.
- El agente recibe instrucciones explicitas para no inventar datos.

## Resultado esperado

Al finalizar la ejecución, el usuario puede conversar con un agente experto que:

- Explica métricas avanzadas de fútbol usando RAG.
- Consulta jugadores reales del CSV completo.
- Genera rankings y comparaciones mediante SQL.
- Mantiene memoria de la conversacion.
- Responde con un tono profesional y basado en evidencia.

## Autor

Proyecto desarrollado por Óscar Fernández como trabajo final de IA Generativa dentro del Master en Data Science e Inteligencia Artificial.
