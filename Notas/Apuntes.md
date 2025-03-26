# Transformers

#transformers #general #teoria


## Introducción: ¿Qué son los Transformers?

Los Transformers son una clase de modelos de aprendizaje profundo (*deep learning*) caracterizado por ciertos rasgos en su arquitectura.

La arquitectura original de Transformer es una instancia específica de los modelos encoder-decorder populares al rededor del año 2015. Hasta ese punto, la [[Atencion]] era sólo uno de los mecanismos utilizados por estos modelos, que estaban basados principalmente en LSTM (memoria de Largo Corto Plazo) y otras RNN (redes neuronales recurrentes). La idea clave del artículo original de los Transformers es, como su título lo dice, que la atención puede ser usado como el único mecanismo para derivar dependencias entre la entrada y la salida.

La entrada de los Transformers es una secuencia de tokens. La salida del **encoder** es una representación de dimensión fija para cada uno de los tokens junto con una incrustación[^1] por separado de la secuencia como un todo. El **decoder** toma la salida del **encoder** como entrada y escupe (xd) una secuencia de tokens como su salida.

Desde la publicación del artículo original, modelos populares como BERT y GPT han usado sólo las ideas del encoder o decoder de la arquitectura original.



## Arquitectura Codificador/Decodificador

Una arquitectura genérica de codificador/decodificador está compuesta de dos modelos. El codificador toma la entrada y la codifica en un vector de longitud fija. En el caso de la arquitectura original de los Transformers, tanto el codificador como el decodificador tenían 6 capas idénticas. En cada una de esas 6 capas el Encoder tenía dos subcapas:

- Una capa de atención de múltiples cabezas
- Una red neuronal de propagación directa simple.

La capa de auto-atención computa la representación de salida de cada uno de sus tokens de entrada basado en todos los tokens de entrada.

![Arquitectura original del Transformer](media/ArquitecturaOriginalTransformer.png)

La capa de atención de múltiples cabezas en el **decodificador** es ligeramente diferente que aquella del codificador. En ella se enmascaran todos los tokens a la derecha del token cuya representación esta siendo computada para asegurarse de que el decodificador solo pueda atender a los tokens que vienen antes del token que está intentando predecir. El decodificador también también tiene añadido una tercera subcapa, que se trata de otra capa de atención de múltiples cabezas sobre todas las salidas del codificador.



## Atención

Es claro de la descripción de arriba que los únicos elementos *exóticos* de la arquitectura del modelo son las capas de atención de múltiples cabeza, pero es justo ahí donde se encuentra el poder del modelo. Las múltiples cabezas de atención permiten paralelizar el procesamiento de datos, lo que permite aprovechar el poder computacional de TPUs, GPUs, entre otros, para hacer más eficiente el entrenamiento y procesamiento de datos.

Cada token en la entrada de la capa de atención se convierte en tres vectores asociados a él:

❓ Query,
🗝️ Key,
💹 Value,

usando tres matrices correspondientes. La representación de salida de cada token 1️⃣ es calculada como una suma ponderada de los 💹*values* de todos los tokens 2️⃣, donde el peso asignado a cada 💹 *value* (de los tokens 2️⃣) es calculado por una función de compatibilidad entre su vector 🗝️ *key* asociado (de los tokens 2️⃣) y el ❓ *query* del token cuya representación está siendo calculada (token 1️⃣).


---
[^1]: Un **embedding** es una representación vectorial densa de elementos discretos (como palabras, tokens o incluso ítems) en un espacio de dimensiones bajas. Al transformar elementos discretos en vectores, se pueden reflejar similitudes y relaciones entre ellos. Mientras que las representaciones discretas suelen ser muy dispersas, los embeddings permiten representar la información de forma compacta, lo que facilita el procesamiento por parte de las redes neuronales. En modelos como los Transformers, los embeddings se aprenden de forma conjunta con el resto de la arquitectura, permitiendo que la red adapte estas representaciones a la tarea específica, ya sea traducción, análisis de sentimientos, etc.