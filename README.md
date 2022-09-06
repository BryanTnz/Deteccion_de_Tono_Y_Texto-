# Deteccion_de_Tono_Y_Texto-

Video de Explicación y Funcionalidad: https://youtu.be/bxf7ZYOXqn8

Video de Manual de Usuario: https://www.youtube.com/watch?v=2xKDfL69Yy4

---
Este documento es una Guia básica de como se realizo la implementación para el reconocimiento automático del habla utilizando SPICE de TensorFlow. Adicionalmente la implementacion de SpeechRecongnition el cual transcribe los audios en texto, se realizado en Google Colab. 

---
# Detección de tono SPICE
---
Paso1_ El archivo de entrada de audio
---
Ofrecemos cuatro métodos para obtener un archivo de audio:

1) Grabe audio directamente en colab
```
INPUT_SOURCE = 'RECORD'
```
2) Sube desde tu computadora
```
INPUT_SOURCE = 'UPLOAD'
```
3) Utilice un archivo guardado en Google Drive
```
INPUT_SOURCE = './drive/'
```
4) Descarga el archivo de la web
```
INPUT_SOURCE = 'http'
```


Paso2_ Preparando los datos de audio
---
El modelo SPICE necesita como entrada un archivo de audio a una frecuencia de muestreo de 16kHz y con un solo canal (mono).

- Por ello se creo una función para convertir cualquier archivo de sonido que tiene que formato esperado del modelo:
```
convert_audio_for_model
```
- Convirtiendo al formato esperado para el modelo
en todos los métodos de entrada de entrada 4 anteriores, el nombre del archivo cargado está en
la variable uploaded_file_name
```
converted_audio_file = convert_audio_for_model(uploaded_file_name)
```

Paso3_Ejecutando el modelo
---
Vamos a cargar el modelo con TensorFlow Hub. SPICE nos dará dos salidas: tono e incertidumbre

- Para cargar el modelo solo necesita el módulo Hub y la URL que apunta al modelo:
```
# Cargando el modelo SPICE:
model = hub.load("https://tfhub.dev/google/spice/2")
```
- Con el modelo cargado, los datos preparados, necesitamos 3 líneas para obtener el resultado:
```
model_output = model.signatures["serving_default"](tf.constant(audio_samples, tf.float32))
pitch_outputs = model_output["pitch"]
uncertainty_outputs = model_output["uncertainty"]
```
- Los valores de tono devueltos por SPICE están en el rango de 0 a 1. Vamos a convertirlos en valores de tono absoluto en Hz. Por ello se creo la funcion
```
output2hz(pitch_output)
```


Paso4_ Conversión a notas musicales
---
Ahora que tenemos los valores de tono, ¡conviértelos en notas! Esta parte es un desafío en sí misma. Tenemos que tener en cuenta dos cosas:

1) los descansos (cuando no hay canto)
```
pitch_outputs_and_rests = [
    output2hz(p) if c >= 0.9 else 0
    for i, p, c in zip(indices, pitch_outputs, confidence_outputs)
]
```
2) el tamaño de cada nota (compensaciones)
```
A4 = 440
C0 = A4 * pow(2, -4.75)
note_names = ["C", "C#", "D", "D#", "E", "F", "F#", "G", "G#", "A", "A#", "B"]

hz2offset(freq)
```
3) Ahora escribamos las notas cuantificadas como partitura
```
showScore(sc)
print(best_notes_and_rests)
```
4) Convirtamos las notas musicales a un archivo MIDI 
 ```
converted_audio_file_as_midi = converted_audio_file[:-4] + '.mid'
fp = sc.write('midi', fp=converted_audio_file_as_midi)

wav_from_created_midi = converted_audio_file_as_midi.replace(' ', '_') + "_midioutput.wav"
print(wav_from_created_midi)
```

Paso5_ Convierto el archivo MIDI a .WAV
---
Para escucharlo en colab, se necesita convertirlo a wav. Una forma sencilla de hacerlo es utilizar Timidity.

1) Para instilar Timidity usamos el siguiente codigo
```
!sudo apt-get install -q -y timidity libsndfile1
```
2) Transformando el archivo MIDI en WAV. por medio de Timidity
```
!timidity $converted_audio_file_as_midi -Ow -o $wav_from_created_midi

```


# Detección de texto


Paso1_ Instalamos las siguientes librias
---
```
pip install pydub numba==0.48 librosa music21
pip install SpeechRecognition
```

Paso2_ El archivo de entrada de audio
---
Ofrecemos cuatro métodos para obtener un archivo de audio:

1) Grabe audio directamente en colab
```
INPUT_SOURCE = 'RECORD'
```
2) Sube desde tu computadora
```
INPUT_SOURCE = 'UPLOAD'
```
3) Utilice un archivo guardado en Google Drive
```
INPUT_SOURCE = './drive/'
```
4) Descarga el archivo de la web
```
INPUT_SOURCE = 'http'
```
Paso3_ Preparando los datos de audio
---
El modelo SPICE necesita como entrada un archivo de audio a una frecuencia de muestreo de 16kHz y con un solo canal (mono).

- Por ello se creo una función para convertir cualquier archivo de sonido que tiene que formato esperado del modelo:
```
convert_audio_for_model
```
- Convirtiendo al formato esperado para el modelo
en todos los métodos de entrada de entrada 4 anteriores, el nombre del archivo cargado está en
la variable uploaded_file_name
```
converted_audio_file = convert_audio_for_model(uploaded_file_name)
```

Paso4_ Deteccion de texto SpeechRecognition
---

- Importamos la libreria y ejecutamos la funcion del recognizer y e imprimimos el texto
```
import speech_recognition as sr

audio_ex = sr.AudioFile('converted_audio_file.wav')

text = recognizer.recognize_google(audio_data=audiodata, language='en-ES')
print(text)
```


Referencia
---

[1]  Tensorflow, «Tensorflow,» Tensorflow, [En línea]. Available: https://www.tensorflow.org/. [Último acceso: 31 08 2022]. 

[2]  B. Lutkevich, «SearchCustomerExperience,» SearchCustomerExperience, 13 09 2021. [En línea]. Available: https://www.techtarget.com/searchcustomerexperience/definition/speech-recognition. [Último acceso: 31 08 2022]. 

[3]  P. S. Foundation, «Python,» Python Software Foundation , [En línea]. Available: https://www.python.org/doc/. [Último acceso: 31 08 2022]. 


