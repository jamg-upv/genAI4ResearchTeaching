# File:API_PROMPT_ART-749-screeningSLR.ipynb

## Historia de Usuario - Script de Procesamiento Masivo con Múltiples Modelos de IA

### Descripción
Como investigador/analista, necesito procesar automáticamente un conjunto de datos a través de múltiples modelos de IA de lenguaje (LLMs) para comparar sus respuestas y analizar sus resultados de forma sistemática.

### Funcionalidad Principal

#### ¿Qué hace el script?
El script automatiza el envío de preguntas/prompts a diferentes modelos de inteligencia artificial y guarda todas las respuestas en archivos Excel separados para su posterior análisis.

#### Proceso paso a paso:
1. **Lee los datos de entrada**: Toma un conjunto de filas de un DataFrame (tabla de datos) que contiene los prompts/preguntas a enviar
2. **Itera por cada modelo de IA**: Procesa la misma información con diferentes modelos (Claude, GPT, Gemini, DeepSeek, etc.)
3. **Envía cada fila al modelo**: Para cada fila del rango especificado, envía el prompt al modelo actual
4. **Maneja errores automáticamente**: Si falla una petición, reintenta hasta 3 veces con pausas progresivas
5. **Guarda las respuestas**: Captura la respuesta del modelo, tokens utilizados y otros metadatos
6. **Genera archivos por modelo**: Crea un archivo Excel independiente para cada modelo con todas sus respuestas
7. **Nombra archivos con metadatos**: Incluye el modelo usado, temperatura, fecha y hora en el nombre del archivo

### Parámetros Configurables

- **Modelos a procesar**: Lista de modelos de IA que se ejecutarán secuencialmente
- **Rango de filas**: `start_row` y `end_row` definen qué filas del dataset se procesarán
- **Temperatura**: Controla la creatividad/aleatoriedad de las respuestas (0-1)
- **Nombre base**: Prefijo para identificar los archivos generados
- **Pausas**: Delays entre peticiones para evitar límites de API

### Características de Robustez

- **Reintentos automáticos**: Hasta 3 intentos con pausas incrementales (2s, 4s, 6s)
- **Guardados periódicos**: Guarda progreso cada 50 filas para evitar pérdida de datos
- **Manejo de interrupciones**: Si se detiene manualmente, guarda el progreso hasta ese momento
- **Registro de errores**: Marca filas con error pero continúa el proceso
- **Continuidad entre modelos**: Si un modelo falla completamente, continúa con el siguiente

### Salida Esperada

Para cada modelo procesado se genera un archivo Excel con:
- Datos originales de entrada
- Respuesta del modelo (`GPTContent`)
- Metadata completa de la API (`GPTResponse`)
- Uso de tokens (`GPTUsage`)
- Prompt enviado (`PromtAgregado`)
- Identificador de fila (`GPTId`)

**Formato de nombre**: `{nombre_base}-{Modelo}-{Temperatura}_{fecha}_{hora}.xlsx`

### Criterios de Aceptación

✅ Procesa todas las filas del rango especificado para cada modelo
✅ Genera un archivo Excel por cada modelo
✅ Reintentar automáticamente en caso de errores temporales
✅ Permite interrumpir sin perder todo el progreso
✅ Registra información completa de uso de tokens y respuestas
