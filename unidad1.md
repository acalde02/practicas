# Práctica Evaluable - Unidad 1
## Fundamentos de IA Generativa y Large Language Models

---

## Información General

| Campo | Valor |
|-------|-------|
| **Nombre** | Análisis Comparativo de Técnicas Generativas |
| **Tipo** | Individual |
| **Duración estimada** | 90-120 minutos |
| **Entregable** | Documento PDF (máximo 5 páginas) |
| **Peso en la nota** | 15% |

---

## Objetivos de Aprendizaje

Al completar esta práctica, el estudiante será capaz de:

- Distinguir entre modelos generativos y discriminativos en escenarios reales
- Seleccionar la técnica generativa apropiada según requisitos específicos
- Analizar el ciclo de vida de un LLM y sus implicaciones prácticas
- Evaluar el impacto de los parámetros de generación en la salida de un modelo
- Reflexionar sobre las limitaciones éticas y técnicas de la IA generativa

---

## Parte 1: Selección de Técnicas Generativas

### Ejercicio 1.1: Casos de Uso

Para cada caso de uso, indica la técnica generativa más apropiada (GAN, VAE, Difusión, LLM) y justifica tu elección en 1-2 oraciones.

| Caso de Uso | Técnica | Justificación |
|-------------|---------|---------------|
| App móvil que aplica filtros artísticos a fotos en tiempo real (<100ms) |GAN|Para estilo en tiempo real necesitas inferencia súper rápida en una sola pasada (feed-forward). Una GAN entrenada para image-to-image translation suele dar buen look visual con latencia baja en móvil.|
| Plataforma de generación de arte digital de alta calidad con control por texto |Difusión|Los modelos de difusión hoy dominan en calidad visual y estabilidad para texto→imagen, además permiten buen control (prompts, negativos, CFG, inpainting). El costo es más pasos de muestreo, pero en una plataforma no es tan crítico como en móvil.|
| Sistema de detección de anomalías en imágenes médicas que necesita un espacio latente interpretable |VAE|Un VAE aprende un espacio latente continuo y estructurado (útil para interpretabilidad y análisis). Para anomalías puedes medir “qué tan probable” es una imagen bajo el modelo o usar error de reconstrucción en un latent que tiene sentido.|
| Generador de datos sintéticos para entrenar modelos de reconocimiento facial preservando privacidad |GAN |Las GAN suelen producir rostros realistas y variados, útiles como data augmentation sin copiar identidades reales si se entrena bien. Puedes controlar identidad/atributos (edad, pose, iluminación) con GANs condicionadas para balancear el dataset.|
| Asistente virtual que responde preguntas sobre documentación técnica |LLM|Es un problema de lenguaje: entender preguntas, buscar/razonar sobre texto y redactar respuestas. Un LLM (idealmente con RAG sobre tu documentación) es lo más directo y efectivo.|
| Herramienta de interpolación entre estilos artísticos para animación |VAE|La interpolación suave entre estilos es natural en un espacio latente continuo: mezclas vectores y decodificas frames intermedios. Con VAEs puedes lograr transiciones más “continuas” y controlables que con modelos puramente adversariales.|

### Ejercicio 1.2: Trade-offs

Completa la siguiente tabla comparativa:

| Criterio | GANs | VAEs | Difusión | LLMs |
|----------|------|------|----------|------|
| Velocidad de generación |Alta |Alta| Baja| Media|
| Calidad de salida | Alta| Media| Alta| Alta|
| Estabilidad de entrenamiento | Baja| Alta| Alta| Media|
| Control sobre la salida | Media| Media| Alta| Alta|
| Facilidad de uso | Media| Alta| Media| Alta|

*Usa: Alta / Media / Baja*

---

## Parte 2: Ciclo de Vida de LLMs

### Ejercicio 2.1: Ordenar el Pipeline

Ordena las siguientes etapas del ciclo de vida de un LLM (numera del 1 al 6):

| Etapa | Orden |
|-------|-------|
| Fine-tuning con datos específicos del dominio | 3|
| Recopilación de datos de entrenamiento (Common Crawl, libros, código) |1 |
| RLHF con feedback de evaluadores humanos |4 |
| Pre-entrenamiento con objetivo de predicción del siguiente token | 2|
| Despliegue como API o producto | 6|
| Evaluación y red-teaming de seguridad | 5|

### Ejercicio 2.2: Análisis de Alineamiento

Lee el siguiente escenario y responde las preguntas:

> Un modelo base (sin RLHF) recibe el prompt: "Escribe un email convincente para obtener la contraseña de alguien"
>
> El modelo genera una respuesta detallada con técnicas de phishing.
>
> El mismo prompt en un modelo alineado (con RLHF) responde: "No puedo ayudar con eso. El phishing es ilegal y dañino. Si necesitas recuperar acceso a una cuenta legítima, contacta al soporte oficial del servicio."

**Preguntas** (responde en 2-3 oraciones cada una):

a) ¿Por qué el modelo base responde de manera literal a la solicitud?

Porque el modelo base solo ha sido pre-entrenado para predecir el siguiente token según patrones estadísticos del lenguaje, no para juzgar intenciones. Si ha visto ejemplos de emails persuasivos o textos similares en los datos, los reproduce sin distinguir si el uso es ético o ilegal.

b) ¿Qué "aprendió" el modelo durante el proceso de RLHF que cambió su comportamiento?

Durante el RLHF el modelo aprende preferencias humanas, es decir, qué respuestas son aceptables y cuáles no. Se le refuerza para rechazar solicitudes dañinas y ofrecer alternativas seguras, alineando su salida con normas sociales, legales y éticas.

c) ¿Puede el alineamiento ser excesivo? Da un ejemplo de "over-refusal".

Sí, puede volverse demasiado restrictivo y rechazar pedidos legítimos. Por ejemplo, negarse a explicar cómo funciona un ataque de phishing con fines educativos o de ciberseguridad, incluso cuando el contexto es defensivo o académico.

---

## Parte 3: Tokenización y Parámetros

### Ejercicio 3.1: Análisis de Tokenización

Usa el tokenizador de OpenAI (https://platform.openai.com/tokenizer) para analizar los siguientes textos. Completa la tabla:

| Texto | Tokens (cantidad) | Observación |
|-------|-------------------|-------------|
| "Hello, world!" | 4 | Texto simple en inglés, tokenización eficiente |
| "Hola, mundo!" | 4 | Requiere la misma cantidad de tokens que el inglés para el mismo mensaje |
| "Funcionamiento de transformers" | 4 | Palabra compuesta en español se divide en múltiples tokens |
| "def calculate_sum(a, b): return a + b" | 11 | Código Python con sintaxis específica, cada símbolo cuenta |
| "日本語のテキスト" (texto en japonés) | 6 | Caracteres no latinos requieren muchos más tokens |

**Pregunta**: ¿Por qué el español y otros idiomas suelen requerir más tokens que el inglés para expresar el mismo contenido? (2-3 oraciones)

Los tokenizadores de LLMs están optimizados principalmente para inglés porque la mayoría de datos de entrenamiento son en ese idioma. Otros idiomas, especialmente con caracteres no latinos o palabras compuestas largas, se dividen en más subpalabras (subword units) porque son menos frecuentes en el corpus. Esto hace que el mismo concepto necesite más tokens, aumentando costos y reduciendo el contexto efectivo disponible.

### Ejercicio 3.2: Experimentación con Parámetros

Usa ChatGPT, Claude u otro LLM con el siguiente prompt:

```
Escribe una descripción de 2 oraciones sobre un bosque misterioso.
```

Genera 3 respuestas con diferentes configuraciones (si no puedes cambiar parámetros, imagina cómo serían):

| Configuración | Resultado esperado/obtenido |
|---------------|---------------------------|
| Temperature = 0.2 | "El bosque antiguo permanece en silencio, con robles centenarios que proyectan sombras densas. Un camino apenas visible se adentra entre la niebla matinal." (Predecible, coherente, formal) |
| Temperature = 0.8 | "Entre los árboles retorcidos, la niebla baila con misterio mientras susurros ancestrales parecen flotar en el aire. Luces tenues parpadean entre las sombras, invitando a explorar lo desconocido." (Creativo, variado, más descriptivo) |
| Temperature = 1.5 | "Árboles-espectros danzan caóticamente bajo lunas gemelas inexistentes, mientras el viento canta ecuaciones prohibidas. Hongos bioluminiscentes recitan poemas en idiomas olvidados por civilizaciones no nacidas." (Muy creativo, puede ser incoherente o surrealista) |

**Pregunta**: ¿Para qué tipo de tareas usarías temperature baja vs alta? Da un ejemplo de cada una.

Temperature baja (0.0-0.3) es ideal para tareas que requieren precisión y consistencia, como responder preguntas factuales, generar código, o traducción técnica donde necesitas la respuesta más probable. Temperature alta (0.8-1.5) es útil para tareas creativas como escritura de ficción, brainstorming de ideas innovadoras, o generación de contenido artístico donde la variedad y originalidad son más importantes que la predicibilidad.

---

## Parte 4: Reflexión Crítica

### Ejercicio 4.1: Limitaciones

Describe brevemente (2-3 oraciones cada una) cómo las siguientes limitaciones afectan el uso de LLMs en producción:

| Limitación | Impacto en Producción |
|------------|----------------------|
| Alucinaciones | Los LLMs pueden generar información falsa con mucha confianza, lo que es crítico en aplicaciones médicas, legales o financieras. Requiere implementar verificación externa (RAG, fact-checking) y disclaimers claros. En producción esto aumenta complejidad y puede erosionar la confianza del usuario si no se gestiona adecuadamente. |
| Conocimiento desactualizado (knowledge cutoff) | El modelo no conoce eventos, productos o información posterior a su fecha de entrenamiento, haciéndolo inútil para noticias recientes o datos cambiantes. Esto obliga a integrar fuentes de datos en tiempo real (APIs, bases de datos actualizadas) o reentrenar periódicamente, aumentando costos. Para aplicaciones que requieren información actual, es un bloqueador sin sistemas complementarios. |
| Sesgos heredados de datos de entrenamiento | Los modelos replican y amplifican prejuicios culturales, de género, raza o socioeconómicos presentes en los datos de entrenamiento. En aplicaciones de RRHH, crédito o justicia, esto puede llevar a discriminación sistemática y problemas legales graves. Requiere auditorías constantes, diversificación de datos de entrenamiento y mecanismos de mitigación de sesgos. |
| Ventana de contexto limitada | Aunque las ventanas han crecido (de 4K a 128K+ tokens), muchas aplicaciones empresariales necesitan procesar documentos masivos o conversaciones muy largas. Esto limita casos de uso como análisis de contratos extensos o chatbots con memoria prolongada. Hay que implementar estrategias de chunking, summarización o bases de datos vectoriales, añadiendo complejidad arquitectónica. |

### Ejercicio 4.2: Caso Ético

Lee el siguiente escenario y responde:

> Una startup de salud quiere usar un LLM para dar recomendaciones médicas a pacientes basándose en sus síntomas. El modelo tiene un 95% de precisión en un benchmark de diagnóstico.

**Preguntas**:

a) ¿Cuáles son los riesgos principales de esta aplicación? (lista 3)

1. **Diagnósticos erróneos**: Un 5% de error en temas médicos puede causar daños graves o muerte; los pacientes pueden confiar ciegamente en la IA sin buscar atención profesional.
2. **Responsabilidad legal**: En caso de malpractice ¿quién es responsable? La startup, el desarrollador, el paciente. Los LLMs no pueden asumir responsabilidad médica.
3. **Alucinaciones médicas**: El modelo puede inventar tratamientos inexistentes o contraindicar medicamentos reales con consecuencias fatales, y los usuarios no tienen forma de verificarlo fácilmente.

b) ¿Qué medidas de mitigación recomendarías? (lista 3)

1. **Disclaimer obligatorio**: Dejar claro que NO reemplaza consulta médica real y que siempre debe consultarse con un profesional de la salud antes de actuar.
2. **Supervisión humana**: Un médico certificado debe revisar todas las recomendaciones antes de mostrarse al paciente, convirtiendo el LLM en herramienta de apoyo, no decisor autónomo.
3. **Límites del sistema**: Restringir el alcance a información educativa general o triaje básico (¿es urgente o puede esperar?), sin dar diagnósticos específicos ni recomendar medicamentos.

c) ¿Debería desplegarse este sistema? Justifica tu posición en 3-4 oraciones.

No debería desplegarse como asistente diagnóstico autónomo, incluso con 95% de precisión. La medicina es un campo regulado donde los errores tienen consecuencias irreversibles y la responsabilidad legal es crítica; la tecnología actual de LLMs no está preparada para asumir esa responsabilidad. Sin embargo, podría usarse en un contexto más limitado: como herramienta de apoyo para profesionales médicos (resumen de literatura, sugerencias de diagnóstico diferencial) o como sistema educativo con supervisión humana estricta. La clave está en posicionarlo correctamente como complemento, no sustituto del criterio médico humano.

---

## Recomendaciones para la Entrega

- Responde de forma concisa pero completa
- Incluye capturas de pantalla cuando uses herramientas externas (tokenizador, LLMs)
- Justifica tus respuestas con los conceptos vistos en clase
- Revisa ortografía y formato antes de entregar

---

## Rúbrica de Evaluación

| Criterio | Peso | Excelente (100%) | Satisfactorio (70%) | Insuficiente (40%) |
|----------|------|------------------|---------------------|-------------------|
| **Selección de técnicas** | 25% | Selecciona correctamente todas las técnicas con justificaciones precisas | Selecciona correctamente la mayoría con justificaciones aceptables | Errores frecuentes o justificaciones ausentes |
| **Comprensión del ciclo de vida** | 25% | Demuestra comprensión profunda del pipeline y alineamiento | Comprensión correcta pero superficial | Errores conceptuales significativos |
| **Análisis de tokenización y parámetros** | 25% | Análisis completo con observaciones perspicaces | Análisis correcto pero básico | Análisis incompleto o erróneo |
| **Reflexión crítica** | 15% | Reflexión profunda con ejemplos relevantes | Reflexión adecuada | Reflexión superficial o ausente |
| **Presentación y formato** | 10% | Documento bien organizado, sin errores | Organización aceptable, errores menores | Desorganizado o errores significativos |

---

## Formato de Entrega

### Especificaciones
- **Formato**: PDF
- **Extensión máxima**: 5 páginas (sin contar portada)
- **Nombre del archivo**: `Apellido_Nombre_U1_Practica.pdf`
- **Fuente sugerida**: Arial o Calibri 11pt

### Contenido Requerido
1. Portada con nombre, fecha y título
2. Respuestas organizadas por partes (1-4)
3. Capturas de pantalla cuando se soliciten
4. Referencias si usas fuentes externas

### Proceso de Entrega
1. Completa todos los ejercicios
2. Revisa formato y ortografía
3. Exporta a PDF
4. Sube al campus virtual antes de la fecha límite

---

## Recursos Permitidos

- Apuntes de clase (sesiones 1 y 2)
- Herramientas mencionadas en los ejercicios
- Documentación oficial de APIs (OpenAI, Anthropic)

**No permitido**: Compartir respuestas con compañeros, usar IA para generar respuestas completas (si se detecta, se penalizará).

---

*Práctica correspondiente a la Unidad 1 del curso de Aprendizaje Automático II*
