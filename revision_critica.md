# Revisión crítica de la práctica

Este documento revisa las limitaciones del cuaderno original y propone cómo podrían abordarse en un trabajo posterior.

Las correcciones propuestas no se han implementado ni evaluado. Se mantienen el código y los resultados originales ya que la intención es demostrar el trabajo académico realizado.

## 1. Tamaño y alcance del corpus

El corpus contiene 70 pares de segmentos. Esta cantidad permite practicar el procedimiento de ajuste, pero ofrece una base muy limitada para evaluar la traducción de contratos. A los efectos de la práctica académica era suficiente.

Los ejemplos que se muestran pertenecen al ámbito de contratos en general.

**Posible corrección:** ampliar y diversificar el corpus para un fine-tuning más eficaz.

## 2. Dos particiones diferentes

Primero se crea una partición con 56 segmentos de entrenamiento y 14 de prueba mediante scikit-learn.

Después se crea otra con `dataset["train"].train_test_split()`, sin indicar en esa llamada el tamaño ni la semilla. Esta segunda partición, de 52 y 18 segmentos, es la utilizada finalmente.

**Consecuencia:** las cifras de la primera división no describen los conjuntos usados para entrenar y evaluar.

**Posible corrección:** conservar una única partición, con proporciones y semilla explícitas, y guardar sus identificadores.

## 3. Ausencia de prueba independiente

Los 18 segmentos denominados `test` se utilizan para evaluar al finalizar cada época.

Esta evaluación permite observar el entrenamiento, pero no constituye una prueba final independiente de su seguimiento y de las decisiones que puedan tomarse a partir de él.

**Posible corrección:** con un corpus mayor, separar entrenamiento, validación y prueba. Si los segmentos proceden de varios documentos, dividir por documento para reducir el riesgo de compartir formulaciones muy próximas entre conjuntos.

## 4. Configuración de idiomas

El código mostrado no establece explícitamente los códigos de idioma de origen y destino del tokenizador mBART ni la configuración de generación del español.

La aparición de traducciones en español no basta para verificar que toda la preparación de entradas y etiquetas sea adecuada.

**Posible corrección:** configurar y comprobar los idiomas de origen y destino durante la tokenización y la generación, siguiendo la documentación del checkpoint y de la versión utilizada.

## 5. Falta de comparación con el modelo inicial

El cuaderno muestra BLEU después de cada época, pero no evalúa el modelo antes del ajuste bajo las mismas condiciones. Esto se debe a que la comparación no era el objetivo de la práctica, sino simplemente ejercitarse en el proceso de ajustar el modelo.

**Consecuencia:** no puede afirmarse cuánto mejora o empeora el modelo respecto a su punto de partida.

**Posible desarrollo futuro:** evaluar antes y después del ajuste con los mismos segmentos, referencias y parámetros de generación.

## 6. Interpretación de BLEU

La puntuación final guardada es 23,0528 sobre 18 segmentos. BLEU mide coincidencias con las referencias; no representa un porcentaje de traducciones correctas ni certifica fidelidad jurídica.

El conjunto es pequeño y contiene segmentos de distinta longitud y naturaleza.

**Posible corrección:** ampliar la evaluación, registrar la configuración completa de SacreBLEU y complementarla con revisión humana de fidelidad, terminología, omisiones y adiciones.

## 7. Truncamiento y generación incompleta

La tokenización limita tanto originales como traducciones de referencia a 128 tokens.

Además, una cláusula extensa produce una traducción muy incompleta. El cuaderno no permite atribuir este fallo a una única causa.

**Posible corrección:** comprobar qué segmentos se truncan y revisar por separado la entrada recibida, los límites de generación y la terminación de la salida.

## 8. Revisión de los ejemplos

Algunos errores requieren una descripción más precisa que la incluida en los comentarios originales:

- En la oración sobre el comienzo de la vigencia, la salida introduce «el día siguiente», información ausente del original.
- En la cláusula de validez, sustituir `term` por «partes» altera el referente. Cambiar `any` por «ninguna» también modifica la condición.
- La cláusula de terminación pierde gran parte del contenido y altera la relación entre los acuerdos mencionados.
- `Cousin` se traduce como «padre».
- `Next week` se transforma en «el próximo fin de semana».

La fluidez de una salida no garantiza que conserve el significado.

**Posible corrección:** clasificar los errores por categorías y gravedad, utilizando criterios explícitos y ejemplos completos de original, referencia y traducción generada.

## 9. Explicaciones de los fallos

El cuaderno atribuye algunos errores al reducido tamaño del corpus y sugiere que más datos los solucionarían.

Es una hipótesis razonable, pero no está comprobada. También deben revisarse la configuración de idiomas, la tokenización, la generación y la calidad de los datos.

Asimismo, la capacidad de traducir palabras ausentes de este pequeño corpus puede proceder del preentrenamiento; no implica que el modelo las haya aprendido durante la práctica.

**Posible corrección:** separar observaciones de hipótesis y comprobar cada explicación mediante comparaciones controladas.

## Alcance de la publicación

Este repositorio muestra una práctica académica de preparación de datos, ajuste de un modelo y examen de sus resultados.

No demuestra que el modelo esté preparado para traducir contratos de forma fiable ni que las mejoras propuestas hayan sido verificadas. Simplemente se pretende demostrar el ejercicio realizado.
