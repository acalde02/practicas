# Práctica Unidad 5 – Sistema RAG

## 1. Introducción

En esta práctica se ha desarrollado un sistema de tipo **Retrieval-Augmented Generation (RAG)** con el objetivo de construir un asistente capaz de responder preguntas basadas exclusivamente en documentación interna de una empresa ficticia (TechCorp).

El sistema combina técnicas de procesamiento de lenguaje natural, embeddings semánticos y recuperación de información para generar respuestas contextualizadas y precisas.

---

## 2. Arquitectura del sistema

El sistema sigue el siguiente flujo:

1. **Carga de documentos** desde archivos `.txt`
2. **Segmentación (chunking)** de los documentos
3. **Generación de embeddings** (representaciones vectoriales)
4. **Almacenamiento en base vectorial (ChromaDB)**
5. **Recuperación de contexto relevante (retriever)**
6. **Generación de respuesta con un modelo de lenguaje (LLM)**

---

## 3. Tecnologías utilizadas

- Python
- LangChain
- ChromaDB
- Sentence Transformers (embeddings locales)
- OpenRouter (modelo de lenguaje)
- Gradio (interfaz web)

---

## 4. Embeddings y recuperación

Se utilizaron embeddings locales mediante el modelo:

```
sentence-transformers/all-MiniLM-L6-v2
```

### Justificación

- Es un modelo ligero y eficiente
- No requiere conexión a APIs externas
- Permite mantener el sistema completamente gratuito
- Tiene buen desempeño en tareas de búsqueda semántica

---

## 5. Modelo de lenguaje (LLM)

Se utilizó un modelo gratuito a través de OpenRouter:

```
openrouter/free
```

### Parámetros utilizados

- **temperature = 0.3**

### Justificación de la temperatura

Se eligió una temperatura baja para:

- Reducir la generación de respuestas aleatorias
- Priorizar respuestas más deterministas
- Evitar alucinaciones del modelo
- Asegurar consistencia en respuestas empresariales

---

## 6. Reflexión crítica

Para la implementación de este asistente se utilizó LangChain junto con modelos gratuitos/open source debido a restricciones económicas, ya que el uso de modelos propietarios de alta gama implica costes elevados que no son viables en el contexto de esta práctica académica.

A pesar de esta limitación, el sistema cumple correctamente con los objetivos planteados, demostrando que es posible construir un RAG funcional utilizando herramientas accesibles.

Sin embargo, desde un punto de vista técnico, existen varias mejoras claras:

### Limitaciones actuales

- Menor calidad semántica frente a modelos avanzados
- Posible pérdida de contexto en preguntas complejas
- Dependencia de la calidad del chunking
- Respuestas menos naturales en comparación con modelos de última generación

### Mejoras propuestas

Si se dispusiera de recursos, se migraría a modelos más avanzados como:

- GPT-5.3
- Claude Opus

En particular, modelos de la familia Claude (como Claude Opus) serían especialmente adecuados para este tipo de implementación RAG, ya que están optimizados para reducir alucinaciones y mantener una mayor fidelidad al contexto proporcionado. Esto es crítico en sistemas empresariales, donde la precisión de la información es prioritaria sobre la creatividad del modelo.

Además, Claude destaca por su capacidad de seguir instrucciones de forma estricta, lo que permitiría mejorar significativamente el comportamiento del asistente en escenarios donde se requiere responder únicamente con información documentada, evitando la generación de contenido no verificado.

Esto permitiría:

- Mejor comprensión del contexto
- Respuestas más naturales y fluidas
- Menor tasa de alucinación
- Mejor capacidad de razonamiento

### Otras mejoras posibles

- Implementar memoria conversacional
- Optimizar el sistema de chunking
- Ajustar dinámicamente el número de documentos recuperados
- Incorporar reranking de documentos
- Añadir logs y trazabilidad del sistema
- Implementar evaluación automática de respuestas

### Justificación del diseño (enfoque humano)

El sistema ha sido diseñado priorizando simplicidad, eficiencia y accesibilidad. La elección de herramientas gratuitas no solo responde a una limitación económica, sino también a una intención de demostrar que es posible construir soluciones funcionales sin depender de infraestructuras costosas.

Además, el enfoque adoptado permite comprender en profundidad cada componente del pipeline RAG, lo cual resulta fundamental en un entorno académico y de aprendizaje.

---

## 7. Ejemplos de funcionamiento

A continuación se muestran ejemplos reales de interacción con el sistema:

Ver archivo: `respuesta_chatbot.md`

---

## 8. Conclusión

El sistema desarrollado cumple con los objetivos de la práctica, integrando correctamente los conceptos de recuperación de información y generación de lenguaje natural.

A pesar de utilizar herramientas gratuitas, el rendimiento es adecuado para un entorno académico, y el sistema es fácilmente escalable hacia soluciones más avanzadas en un entorno profesional.