# Evaluación de Sistema RAG con RAGAS
## Fundación Universitaria Konrad Lorenz — Desarrollo de Aplicaciones con IA
**Estudiante:** Laura Valentina López García  
**Docente:** Leandro Pájaro Fuentes  
---
## Descripción
Evaluación del desempeño de un pipeline RAG (Retrieval Augmented Generation) implementado sobre el Reglamento Académico Institucional de la Fundación Universitaria Konrad Lorenz, usando la librería RAGAS para medir las métricas de calidad.
## Contenido del repositorio
| Archivo | Descripción |
|---|---|
| evaluacion_rag_konrad_k5.ipynb | Pipeline RAG con k=5 fragmentos recuperados |
| evaluacion_rag_konrad_k3.ipynb | Pipeline RAG con k=3 (experimento comparativo) |
| reglamento-academico-institucional.pdf | Documento fuente — Acuerdo No. 01, enero 2025 |
## Tecnologías
- **LangChain** - orquestación del pipeline
- **ChromaDB** - base vectorial con similitud coseno
- **Google Gemini** - embeddings (gemini-embedding-001) y LLM generador
- **RAGAs 0.4.3** - evaluación automática de métricas
## Configuración del Pipeline
| Parámetro | Valor |
|---|---|
| Documento | reglamento-academico-institucional.pdf (64 páginas) |
| Modelo de embeddings | gemini-embedding-001 |
| chunk_size / overlap | 500 / 50 caracteres |
| k (chunks recuperados) | 5 (principal) y 3 (comparativo) |
| LLM generador | gemini-3.1-flash-lite-preview (temperature=0.0) |
| LLM juez RAGAS | gemini-3.1-flash-lite-preview |
## Resultados
| Experimento | Faithfulness | Answer Relevancy | Context Precision |
|---|---|---|---|
| k=5 | 1.0000 | 0.8350 | 0.9750 |
| k=3 | 1.0000 | 0.8347 | 0.3333 |
## Requisitos previos
Este notebook está diseñado para ejecutarse en **Google Colab** únicamente.
Antes de ejecutar, asegúrate de:
1. Tener una cuenta de Google
2. Abrir el notebook desde [Google Colab](https://colab.research.google.com)
3. Subir el archivo reglamento-academico-institucional.pdf a tu **Google Drive** 
   (en la raíz de "Mi unidad", no dentro de ninguna carpeta)
4. Tener una API Key válida de Google Gemini — créala en [Google AI Studio](https://aistudio.google.com)
5. Reemplazar la variable API_KEY en la primera celda con tu propia key
Cuando el notebook llegue a la celda de **Paso 1 - Carga del PDF**, te pedirá 
autorización para acceder a tu Google Drive — acéptala con tu cuenta de Google.
## Cómo ejecutar
1. Abre el notebook en Google Colab
2. Sube el PDF reglamento-academico-institucional.pdf a tu Google Drive
3. Ejecuta las celdas en orden
4. La celda de ChromaDB tarda varios minutos por los lotes de embeddings
## Casos de prueba evaluados
8 preguntas cubriendo 4 tipos obligatorios:
| Tipo | Preguntas |
|---|---|
| Textual directa | P1, P2 |
| Vocabulario diferente | P3, P4 |
| Multi-chunk | P5, P6 |
| Anti-alucinación | P7, P8 |
## Análisis de resultados

El pipeline demostró un rendimiento excelente en **Faithfulness (1.0000)** en ambos 
experimentos, confirmando que el LLM nunca generó información fuera del documento fuente.

El experimento comparativo entre **k=5 y k=3** reveló un hallazgo importante:
- Con **k=5**, Context Precision = **0.9750** - el retriever recuperó chunks altamente relevantes
- Con **k=3**, Context Precision = **0.3333** - menos chunks pero de menor precisión relativa

Esto indica que recuperar más contexto (k=5) mejora significativamente la calidad 
del retrieval sin afectar la fidelidad de las respuestas.

Los casos de **anti-alucinación** funcionaron correctamente: el sistema respondió 
"No encontré información sobre esto en el documento" para preguntas sobre valores de 
matrícula y becas socioeconómicas, información que no existe en el reglamento.

El análisis completo con la tabla de resultados por pregunta está disponible en el 
documento PDF de entrega.
