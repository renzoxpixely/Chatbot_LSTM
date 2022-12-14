# Chatbot conversacional usando LSTM
Proyecto desarrollado usando Collab en el semillero de **Data Science Research Perú.**
Este proyecto es para crear un chatbot conversacional usando Sequence para secuenciar modelos LSTM. El aprendizaje de secuencia a secuencia consiste en entrenar modelos para convertir de un dominio a secuencias en otro dominio.

**Código y recursos utilizados**

**Lenguaje:** Python 3.8

**Conjunto de datos:** [Conjunto de datos en inglés de Chatterbot Kaggle](https://www.kaggle.com/kausr25/chatterbotenglish)

**Paquetes usados:** numpy, tensorflow, pickle, keras

**Modelo utilizado:** Modelo Seq2Seq LSTM

**API creada:** API funcional de Keras

----
#### Paso 1: Extracción y preprocesamiento de datos
- El conjunto de datos proviene de [chatterbot/english on Kaggle](https://www.kaggle.com/kausr25/chatterbotenglish).com de [kausr25](https://www.kaggle.com/kausr25). - Contiene pares de preguntas y respuestas basadas en varios temas como comida, historia, IA, etc.

- Analiza cada archivo .yml:
1. Concatenar dos o más oraciones si la respuesta tiene dos o más de ellas.
2. Elimine los tipos de datos no deseados que se producen al analizar los datos.
3. Agregue <START> y <END> a todas las respuestas.
4. Cree un Tokenizer y cargue todo el vocabulario (preguntas + respuestas) en él.

- Tres matrices requeridas por el modelo son encoder_input_data, decoder_input_data y decoder_output_data
- Encoder_input-data: Tokenize las preguntas y rellénelas a la longitud máxima.
- Decoder_input-data: Tokenize las respuestas y rellénelas a la longitud máxima.
- Decoder_output-data: Tokenize las respuestas y elimine el primer elemento de todas las tokenized_answers. Este es el elemento <START> que agregamos anteriormente.

#### Paso 2: Definición del modelo de codificador-decodificador
- El modelo tendrá capas incrustadas, LSTM y densas. La configuración básica es la siguiente:
1. 2 capas de entrada: una para encoder_input_data y otra para decoder_input_data.
2. Capa de incrustación: para convertir vectores token en vectores densos de tamaño fijo. (Nota: no olvide el argumento ask_zero=True aquí)
3. Capa LSTM: proporciona acceso a las celdas de largo y corto plazo.

#### Laboral :

- El encoder_input_data viene en la capa de incrustación (encoder_embedding).
- La salida de la capa de incrustación va a la celda LSTM que produce 2 vectores de estado (h y c, que son encoder_states)
- Estos estados se establecen en la celda LSTM del decodificador.
- El decoder_input_data entra a través de la capa de incrustación.
- Las incrustaciones van en la celda LSTM (que tenía los estados) para producir secuencias.

#### Memoria a corto plazo (LSTM):
- Las redes Long Short-Term Memory (LSTM) son un tipo de red neuronal recurrente capaz de aprender la dependencia del orden en problemas de predicción de secuencias.
- Este es un comportamiento requerido en dominios de problemas complejos como la traducción automática, el reconocimiento de voz y más.
- El éxito de los LSTM radica en su pretensión de ser uno de los primeros implementos en superar los problemas técnicos y cumplir la promesa de las redes neuronales recurrentes.
- La red LSTM se compone de diferentes bloques de memoria llamados celdas
(los rectángulos que vemos en la imagen).
- Hay dos estados que se están transfiriendo a la siguiente celda; el estado celular y el estado oculto.
- Los bloques de memoria son los encargados de recordar las cosas y las manipulaciones a esta memoria se realizan a través de tres grandes mecanismos, llamados compuertas.
- La clave de los LSTM es el estado de la celda, la línea horizontal que atraviesa la parte superior del diagrama.
- El estado de la celda es como una cinta transportadora. Se ejecuta directamente a lo largo de toda la cadena, con solo algunas interacciones lineales menores. Es muy fácil que la información fluya sin cambios.
- El LSTM tiene la capacidad de eliminar o agregar información al estado de la celda, cuidadosamente regulado por estructuras llamadas puertas.
- Las puertas son una forma de dejar pasar información opcionalmente. Están compuestos por una capa de red neuronal sigmoidea y una operación de multiplicación puntual.
- La capa sigmoidea genera números entre cero y uno, que describen la cantidad de cada componente que se debe dejar pasar. Un valor de cero significa "no dejar pasar nada", mientras que un valor de uno significa "¡dejar pasar todo!"
- Las LSTM se explican con mucho más detalle en esta publicación de blog [Blog de Colah] (https://colah.github.io/posts/2015-08-Understanding-LSTMs/)

#### Paso 4: Entrenamiento del modelo
- Entrenamos el modelo para un número de 150 épocas con el optimizador RMSprop y la función de pérdida de categorical_crossentropy.
- Precisión del entrenamiento del modelo = 0,96, es decir; 96%

#### Paso 5: Definición de modelos de inferencia
- **Modelo de inferencia del codificador:** Toma la pregunta como entrada y emite estados LSTM (h y c).

- **Modelo de inferencia del decodificador:** Toma 2 entradas, una son los estados LSTM (salida del modelo del codificador), la segunda son las secuencias de entrada de respuesta (las que no tienen la etiqueta <start>). Emitirá las respuestas a la pregunta que alimentamos al modelo del codificador y sus valores de estado.
  
#### Paso 6: Hablando con nuestro Chatbot
- Primero, definimos un método str_to_tokens que convierte preguntas str en tokens Integer con relleno.
1. Primero, tomamos una pregunta como entrada y predecimos los valores de estado usando enc_model.
2. Establecemos los valores de estado en el LSTM del decodificador.
3. Luego, generamos una secuencia que contiene el elemento <start>.
4. Ingresamos esta secuencia en dec_model.
5. Reemplazamos el elemento <start> con el elemento que fue predicho por dec_model y actualizamos los valores de estado.
6. Realizamos el paso anteriorpandao/editor.md.svg) ![]

#### Resultados
Video en el repositorio.
#### Referencias
1. https://colah.github.io/posts/2015-08-Understanding-LSTMs/
2. https://medium.com/predict/creating-a-chatbot-from-scratch-using-keras-and-tensorflow-59e8fc76be79
3. https://machinelearningmastery.com/gentle-introduction-long-short-term-memory-networks-experts/
4. https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21
5. https://www.analyticsvidhya.com/blog/2017/12/fundamentals-of-deep-learning-introduction-to-lstm/