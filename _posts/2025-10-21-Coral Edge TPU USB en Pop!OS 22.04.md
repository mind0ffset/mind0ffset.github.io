---
layout: post
title: Coral Edge TPU USB en Pop!OS 22.04
tags: Pop!OS Coral Edge TPU USB Google
categories: Artículo
---
#### NO USAR EN PRODUCCIÓN
#### SÓLO USO EXCLUSIVO PERSONAL


Se añaden repositorio y certificado.
```
$echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list

$curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/coral-edgetpu.gpg > /dev/null
```

Se actualizan los paquetes.
```
$sudo apt update
```

Se crea un directorio de trabajo y se accede.
```
$mkdir coral_usb_tpu && cd coral_usb_tpu
```

Se crea un entorno virtual con una versión de Python < 3.10 .
```
$uv venv --python=python3.9
```

Se activa el entorno.
```
$source .venv/bin/activate
```

Se añaden las librerías específicas para la TPU desde el repositorio oficial.
```
$uv pip install --extra-index-url https://google-coral.github.io/py-repo/ pycoral~=2.0
```

El bundle del repositorio oficial incluye numpy 2.0 para el ejemplo de prueba está compilado con numpy 1.0, con lo cual se desinstala la Ver. 2 y se instala la Ver. 1, después se puede revertir una vez verificado el test de ejemplo .
```
$uv pip uninstall numpy

$uv pip install numpy == 1.21.0
```

Se instala la librería del runtime para el Edge TPU, hay dos versiones, la std{standard} o la max{overclocked}. Dependiendo de como se quiera hacer funcionar, en la documentación explica que max hace que el Edge TPU coja mucha más temperatura.
```
$sudo apt install libedgetpu1-std

$sudo apt install libedgetpu1-max
```

Hay que añadir el usuario que vaya a usar el Edge TPU al grupo plugdev como sudo porque el dispositivo necesita acceder a la librería del runtime `/usr/lib/x86_64-linux-gnu/libedgetpu.so.1`.
```
sudo usermod -a -G plugdev $USER
```

Se cierra sesión en shell y se abre una nueva. Se activa el entorno virtual de nuevo y se accede al directorio `/coral_usb_tpu` para clonar el repositorio de ejemplo .
```
$git clone https://github.com/google-coral/pycoral.git
```

Se accede a la raíz del repositorio clonado .
```
$cd pycoral
```

Se instalan las dependencias.
```
$bash examples/install_requirements.sh classify_image.py
```

Se ejecuta un clasificador de ejemplo con el comando .
```
$python3 examples/classify_image.py --model test_data/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite --labels test_data/inat_bird_labels.txt --input test_data/parrot.jpg
```

Debería aparecer una clasificación de la imagen de ejemplo .

```
 test_data/inat_bird_labels.txt --input test_data/parrot.jpg
----INFERENCE TIME----
Note: The first inference on Edge TPU is slow because it includes loading the model into Edge TPU memory.
12.9ms
4.3ms
4.3ms
4.2ms
4.3ms
-------RESULTS--------
Ara macao (Scarlet Macaw): 0.75781
```
