# Deteccion_de_Tono_Y_Texto-

Video de Explicación y Funcionalidad: https://youtu.be/bxf7ZYOXqn8

Video de Manual de Usuario: 

---
Este documento es una Guia básica de como se realizo la implementación para el reconocimiento automático del habla utilizando SPICE de TensorFlow. Adicionalmente la implementacion de SpeechRecongnition el cual transcribe los audios en texto, se realizado en Google Colab. 

---
# Detección de tono SPICE
---
El archivo de entrada de audio
---
Ofrecemos cuatro métodos para obtener un archivo de audio:

1) Grabe audio directamente en colab
2) Sube desde tu computadora
3) Utilice un archivo guardado en Google Drive
4) Descarga el archivo de la web


```
INPUT_SOURCE = 'RECORD'

print('You selected', INPUT_SOURCE)

if INPUT_SOURCE == 'RECORD':
  uploaded_file_name = record(5)
elif INPUT_SOURCE == 'UPLOAD':
  try:
    from google.colab import files
  except ImportError:
    print("ImportError: files from google.colab seems to not be available")
  else:
    uploaded = files.upload()
    for fn in uploaded.keys():
      print('User uploaded file "{name}" with length {length} bytes'.format(
          name=fn, length=len(uploaded[fn])))
    uploaded_file_name = next(iter(uploaded))
    print('Uploaded file: ' + uploaded_file_name)
elif INPUT_SOURCE.startswith('./drive/'):
  try:
    from google.colab import drive
  except ImportError:
    print("ImportError: files from google.colab seems to not be available")
  else:
    drive.mount('/content/drive')
    # don't forget to change the name of the file you
    # will you here!
    gdrive_audio_file = 'YOUR_MUSIC_FILE.wav'
    uploaded_file_name = INPUT_SOURCE
elif INPUT_SOURCE.startswith('http'):
  print("Aqui esta")
  #!wget --no-check-certificate 'https://storage.googleapis.com/download.tensorflow.org/data/c-scale-metronome.wav' -O c-scale.wav
  uploaded_file_name = 'prueba.wav'

else:
  print('Unrecognized input format!')
  print('Please select "RECORD", "UPLOAD", or specify a file hosted on Google Drive or a file from the web to download file to download')
```
