# Traducción de contratos inglés-español con mBART

Alcance limitado a una pequeña práctica académica para la asignatura Traducción Automática del máster en Procesamiento del Lenguaje Natural (Universidad de La Rioja, 2024-2026). El trabajo consistía en ajustar un modelo mBART sobre un pequeño corpus paralelo de contratos. Incluye preparación de datos, entrenamiento, evaluación con SacreBLEU y análisis de ejemplos desde una perspectiva traductológica.

Este repositorio conserva el código y los resultados originales. Los experimentos no se han vuelto a ejecutar para esta publicación ya que la intención es mostrar el trabajo tal cual fue realizado.

## Objetivo

Explorar el proceso de ajuste de un modelo preentrenado para traducción inglés-español y examinar sus resultados sobre lenguaje contractual.

Se trata de una práctica exploratoria. El volumen de datos para fine-tuning es demasiado pequeño, por lo que la evaluación realizada no pretende presentar el resultado como un sistema fiable de traducción jurídica, sino simplemente demostrar la práctica realizada.

## Corpus

El cuaderno utiliza un corpus de 70 pares de segmentos en inglés y español, alineados con LF Aligner. El corpus procede de mi propio ejercicio como traductora profesional. Antes de utilizarlo lo anonimicé para eliminar datos sensibles de mis clientes.

[Consultar el dataset en Hugging Face](https://huggingface.co/datasets/LourdesY/agreements_EN_ES).

El código realiza dos particiones diferentes. La utilizada finalmente para entrenar y evaluar contiene:

| Conjunto | Segmentos |
|---|---:|
| Entrenamiento | 52 |
| Evaluación | 18 |

Aunque el código denomina `test` al segundo conjunto, este se utiliza para evaluar durante el entrenamiento. No existe un conjunto de prueba independiente.

## Procedimiento

1. Carga del corpus desde Hugging Face.
2. División de los datos.
3. Tokenización con un máximo de 128 tokens por segmento.
4. Carga del modelo `facebook/mbart-large-50`.
5. Ajuste durante dos épocas mediante `Seq2SeqTrainer`.
6. Evaluación con SacreBLEU.
7. Publicación del modelo y prueba con ejemplos de traducción.

## Resultados originales

La tabla guardada en el cuaderno muestra:

| Época | Pérdida en evaluación | BLEU |
|---|---:|---:|
| 1 | 3,1586 | 18,5859 |
| 2 | 2,9995 | 23,0528 |

Estas cifras corresponden a los 18 segmentos utilizados durante la evaluación del entrenamiento.

BLEU no representa un porcentaje de traducciones correctas. La puntuación obtenida tampoco demuestra una mejora respecto al modelo de partida, porque el cuaderno no incluye su evaluación previa al ajuste.

El reducido tamaño del conjunto limita las conclusiones que pueden extraerse.

## Análisis de las traducciones

Los ejemplos guardados muestran traducciones parcialmente adecuadas, junto con errores relevantes:

- Selección terminológica inadecuada, como la traducción de `term` en determinados contextos.
- Alteración de condiciones y referentes en una cláusula de validez.
- Introducción de información temporal ausente del original.
- Omisión de gran parte del contenido de una cláusula extensa.
- Sustituciones semánticas, como traducir `cousin` por «padre».

En traducción jurídica, una oración fluida puede seguir siendo incorrecta si modifica una condición, una fecha, una obligación o la identidad de una parte.

Los ejemplos permiten examinar estos problemas, pero no constituyen una evaluación sistemática de calidad.

## Limitaciones identificadas

- El corpus contiene únicamente 70 pares de segmentos.
- Se crean dos particiones y solo la segunda se utiliza en el entrenamiento.
- No se reserva una prueba independiente.
- No se compara el rendimiento antes y después del ajuste.
- La configuración mostrada no establece explícitamente los códigos de idioma de origen y destino para mBART.
- El límite de 128 tokens puede truncar segmentos de entrada o de referencia.
- Algunas interpretaciones del cuaderno atribuyen los fallos a la falta de datos sin comprobar otras causas.
- No se documenta una evaluación sistemática de fidelidad, terminología y omisiones.

Estas limitaciones se conservan como parte del registro académico y se revisan sin repetir los experimentos.

[Consultar la revisión crítica y las posibles correcciones](revision_critica.md).

## Consultar el cuaderno

[Abrir el cuaderno con código y resultados](traduccion_contratos_mbart.ipynb).

Puede consultarse en GitHub sin ejecutarlo. No se ha verificado su ejecución completa en un entorno nuevo.

El cuaderno contiene operaciones de autenticación y publicación en Hugging Face. Su ejecución puede requerir acceso a esa plataforma y recursos de cálculo adicionales.

## Modelo publicado durante la práctica

[Consultar el repositorio del modelo en Hugging Face](https://huggingface.co/LourdesY/mbart-contratos-en-es).

La publicación del modelo forma parte del ejercicio académico y no implica que su calidad haya sido validada para uso profesional. Simplemente se pretende demostrar el ejercicio realizado.

## Tecnologías

Python · pandas · NumPy · scikit-learn · Hugging Face Transformers · Datasets · Evaluate · SacreBLEU · LF Aligner · Google Colab

## Autoría y contexto

**Lourdes Yagüe Lobo**

Práctica realizada como trabajo calificable de la asignatura Traducción Automática en el Máster en Procesamiento del Lenguaje Natural de la Universidad de La Rioja.

El corpus alineado y el análisis de los ejemplos forman parte del trabajo presentado. El modelo preentrenado y las bibliotecas utilizadas corresponden a sus respectivos autores.
