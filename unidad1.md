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
| App móvil que aplica filtros artísticos a fotos en tiempo real (<100ms) |GAN|Las GANs son perfectas aquí porque funcionan en una sola pasada, lo que las hace súper rápidas. En un móvil necesitas que sea instantáneo, y las GANs de transferencia de estilo te dan resultados visuales muy buenos sin hacerte esperar.|
| Plataforma de generación de arte digital de alta calidad con control por texto |Difusión|Ahora mismo los modelos de difusión son los reyes en arte generado por texto - piensa en Stable Diffusion o DALL-E. Dan una calidad impresionante y te permiten ajustar muchísimo con prompts, aunque son más lentos. Para una plataforma web, ese tiempo extra no es problema.|
| Sistema de detección de anomalías en imágenes médicas que necesita un espacio latente interpretable |VAE|Los VAE crean un espacio latente más ordenado y fácil de entender que otras opciones. Esto es clave en medicina porque puedes ver qué tan "rara" es una imagen o qué tan bien se reconstruye, lo que ayuda a detectar casos anormales de forma más transparente.|
| Generador de datos sintéticos para entrenar modelos de reconocimiento facial preservando privacidad |GAN |Las GANs se han hecho famosas por crear rostros que no existen pero parecen totalmente reales. Son geniales para crear datos de entrenamiento sin usar fotos de gente real, y puedes controlar cosas como edad o iluminación para tener un dataset más variado.|
| Asistente virtual que responde preguntas sobre documentación técnica |LLM|Esto es justo para lo que están hechos los LLMs - entender preguntas y dar respuestas coherentes sobre texto. Si le añades un sistema RAG que busque en tu documentación, tienes el asistente perfecto para ese trabajo.|
| Herramienta de interpolación entre estilos artísticos para animación |VAE|Los VAE son ideales cuando quieres hacer transiciones suaves porque su espacio latente es continuo. Básicamente puedes "mezclar" dos estilos poco a poco y generar los frames intermedios de forma muy natural, perfecto para animaciones fluidas.|

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

El modelo base es básicamente una máquina de completar texto - ha aprendido a predecir la siguiente palabra basándose en millones de ejemplos. No tiene concepto de bien o mal, simplemente ve "email convincente" en sus datos y genera lo que ha visto antes, sin filtrar si es ético o peligroso.

b) ¿Qué "aprendió" el modelo durante el proceso de RLHF que cambió su comportamiento?

Con RLHF el modelo recibe feedback de humanos reales que le enseñan qué está bien y qué está mal. Es como tener un profesor que te corrige: "esto no, mejor rechaza este tipo de peticiones y ofrece ayuda legítima". Así aprende a alinearse con lo que consideramos comportamiento ético y seguro.

c) ¿Puede el alineamiento ser excesivo? Da un ejemplo de "over-refusal".

Totalmente, a veces se pasan de cautelosos. Por ejemplo, si un profesor de ciberseguridad le pide que explique cómo funciona el phishing para enseñar a sus alumnos a defenderse, un modelo con over-refusal se negaría aunque el propósito sea educativo y totalmente legítimo.

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

Básicamente es porque estos modelos fueron entrenados sobre todo con texto en inglés, así que "conocen" mejor ese idioma. Cuando llega una palabra en español o en japonés, la tienen que partir en trozos más pequeños porque les resulta menos familiar. El resultado es que dices lo mismo pero gastas más tokens, lo que te sale más caro y te deja menos espacio para tu contexto.

### Ejercicio 3.2: Experimentación con Parámetros

Usa ChatGPT, Claude u otro LLM con el siguiente prompt:

```
Escribe una descripción de 2 oraciones sobre un bosque misterioso.
```

Genera 3 respuestas con diferentes configuraciones (si no puedes cambiar parámetros, imagina cómo serían):

| Configuración | Resultado esperado/obtenido |
|---------------|---------------------------|
| Temperature = 0.2 | "El bosque antiguo permanece en silencio, con robles centenarios que proyectan sombras densas. Un camino apenas visible se adentra entre la niebla matinal." |
| Temperature = 0.8 | "Entre los árboles retorcidos, la niebla baila con misterio mientras susurros ancestrales parecen flotar en el aire. Luces tenues parpadean entre las sombras, invitando a explorar lo desconocido."  |
| Temperature = 1.5 | "Árboles-espectros danzan caóticamente bajo lunas gemelas inexistentes, mientras el viento canta ecuaciones prohibidas. Hongos bioluminiscentes recitan poemas en idiomas olvidados por civilizaciones no nacidas." |

**Pregunta**: ¿Para qué tipo de tareas usarías temperature baja vs alta? Da un ejemplo de cada una.

La temperature baja es tu amiga cuando necesitas que el modelo sea predecible y preciso - tipo cuando le pides que escriba código o responda datos objetivos ("¿cuál es la capital de Francia?"). La temperature alta es para cuando quieres creatividad y que se arriesgue: escribir un cuento de fantasía, inventar nombres de productos, o hacer brainstorming de ideas locas para tu startup.

---

## Parte 4: Reflexión Crítica

### Ejercicio 4.1: Limitaciones

Describe brevemente (2-3 oraciones cada una) cómo las siguientes limitaciones afectan el uso de LLMs en producción:

| Limitación | Impacto en Producción |
|------------|----------------------|
| Alucinaciones | Los LLMs a veces se inventan cosas con una confianza que da miedo - imagina que te da un diagnóstico médico totalmente falso pero sonando súper seguro. En producción tienes que añadir capas de verificación, RAG para anclar a datos reales, y avisos claros al usuario. Si no lo gestionas bien, la gente deja de confiar en tu producto (y con razón). |
| Conocimiento desactualizado (knowledge cutoff) | El modelo quedó "congelado" en el tiempo cuando lo entrenaron, así que no sabe nada de lo que pasó después. Si le preguntas por la película que salió la semana pasada o el nuevo iPhone, no tiene ni idea. Para arreglarlo tienes que conectarle APIs con info actual o reentrenarlo constantemente, lo que sale carísimo. Sin eso, es inútil para cosas que cambian rápido. |
| Sesgos heredados de datos de entrenamiento | Si el modelo entrenó con datos llenos de prejuicios (sexismo, racismo, etc.), va a reproducir esos sesgos. Imagina un sistema de RRHH que descarta CVs de mujeres automáticamente porque "aprendió" que los buenos candidatos eran hombres - es un problemón legal y ético enorme. Necesitas auditar constantemente, diversificar tus datos y tener mecanismos para detectar y corregir estos sesgos. |
| Ventana de contexto limitada | Aunque ahora los modelos aguantan más contexto (algunos hasta 128K tokens), sigue siendo limitado. Si quieres analizar un contrato de 500 páginas o tener un chatbot que recuerde una conversación de todo el día, te quedas corto. Hay trucos como partir el texto en trozos, hacer resúmenes o usar bases de datos vectoriales, pero todo eso complica muchísimo tu arquitectura. |

### Ejercicio 4.2: Caso Ético

Lee el siguiente escenario y responde:

> Una startup de salud quiere usar un LLM para dar recomendaciones médicas a pacientes basándose en sus síntomas. El modelo tiene un 95% de precisión en un benchmark de diagnóstico.

**Preguntas**:

a) ¿Cuáles son los riesgos principales de esta aplicación? (lista 3)

1. **Diagnósticos erróneos**: El 5% de error puede sonar bien, pero en medicina un error puede significar que alguien muera. Si la gente confía ciegamente en la IA y no va al médico de verdad, es un problema gigante.
2. **Responsabilidad legal**: Si sale mal, ¿a quién demandas? ¿A la startup? ¿Al programador? ¿Al paciente por usarla? Es un lío legal brutal porque los LLMs no pueden ir a juicio ni pagar indemnizaciones.
3. **Alucinaciones médicas**: El modelo puede inventarse tratamientos que no existen o decir que no tomes un medicamento que realmente necesitas. Y como suena tan convincente, la gente no tiene forma fácil de saber que se lo está inventando.

b) ¿Qué medidas de mitigación recomendarías? (lista 3)

1. **Disclaimer bien grande y claro**: Que diga bien clarito que esto NO sustituye ir al médico y que cualquier cosa que salga aquí hay que consultarla con un profesional de verdad antes de hacer nada.
2. **Supervisión humana real**: Que haya un médico de verdad revisando cada recomendación antes de que llegue al paciente. O sea, usar la IA como asistente del doctor, no como el doctor en sí.
3. **Limitar lo que puede hacer**: Que solo de info general tipo "parece que deberías ir a urgencias ya" o "esto probablemente puede esperar al médico de cabecera", pero sin meterse en diagnósticos concretos ni recetar nada.

c) ¿Debería desplegarse este sistema? Justifica tu posición en 3-4 oraciones.

Rotundamente no como lo plantean. Aunque el 95% suene genial, estamos hablando de vidas humanas - un error aquí no es como que Netflix te recomiende mal una peli. Dicho esto, sí podría tener sentido si lo replanteas: como herramienta para que los médicos consulten literatura rápido, o para hacer triaje muy básico tipo "¿necesitas ir a urgencias YA o puedes esperar?". La clave es que sea un ayudante para profesionales o una herramienta educativa con supervisión, nunca un sustituto del médico que toma decisiones solo.

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
