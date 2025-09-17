
---
layout: post
title: Compilar(Build) llama.cpp con soporte CUDA en Pop!_OS 22.04_amd64_nvidia_57 + gpt-oss-20b-MXFP4 + Docker Open WebUI front
tags: llama.cpp CUDA Pop!_OS gpt-oss Docker Open_WebUI
categories: Artículo
---
###### Esta guía asume un entorno Pop!_OS 22.04_amd64_nvidia_57 inicial.

###### No se garantiza la estabilidad o seguridad de la máquina.

###### No es para uso en producción.

###### Exclusivo para USO PERSONAL.

Esta guía se produce debido a que no hay binarios pre-compilados de llama.cpp con CUDA habilitado, para entornos Ubuntu.

Se utiliza la imagen pop-os_22.04_amd64_nvidia_57.iso desde cero, en un sistema con 16GB DDR5 SDRAM y GPU RTX-4060 8GB.

Como primer paso se actualiza la distribución:
```
$sudo apt update; sudo apt upgrade-y
```
Se instalan dependencias para compilar:
```
$sudo apt install build-essential cmake git libcurl4-openssl-dev -y
```
Se comprueba la versión del driver gráfico nVidia:
```
$nvidia-smi
```
En este caso devuelve:
```
NVIDIA-SMI 570.172.08             Driver Version: 570.172.08     CUDA Version: 12.8 
```
Sabiendo la versión de CUDA, se busca la opción del CUDA-toolkit apropiada en este caso 12.8:
https://developer.nvidia.com/cuda-12-8-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_local

```
$wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin

$sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600

$wget https://developer.download.nvidia.com/compute/cuda/12.8.0/local_installers/cuda-repo-ubuntu2204-12-8-local_12.8.0-570.86.10-1_amd64.deb

$sudo dpkg -i cuda-repo-ubuntu2204-12-8-local_12.8.0-570.86.10-1_amd64.deb

$sudo cp /var/cuda-repo-ubuntu2204-12-8-local/cuda-*-keyring.gpg /usr/share/keyrings/

$sudo apt update

$sudo apt install cuda-toolkit-12-8
```
Una vez instalado CUDA-toolkit apropiado se comprueba la versión de "nvcc":
```
$nvcc --version
```
Si la expresión no devuelve versión, se localiza con:
```
$ls /usr/local | grep cuda
```
Una vez localizada se añaden las rutas a las variables de usuario:
```
$nano ~/.bashrc
```
No hay que utilizar "sudo" porque es el entorno de variables de usuario.
Al final del documento en una línea libre, se añade:
```
export PATH=/usr/local/cuda-12.8/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-12.8/lib64:$LD_LIBRARY_PATH
```
Se puede cerrar la sesión de terminal y volver a abrir otra, o bien:
```
$source ~/.bashrc
```
Ahora sí debería imprimir la versión de "nvcc":
```
$nvcc --version

nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2025 NVIDIA Corporation
Built on Wed_Jan_15_19:20:09_PST_2025
Cuda compilation tools, release 12.8, V12.8.61
Build cuda_12.8.r12.8/compiler.35404655_0

```
A continuación se clona el repositorio de "llama.cpp", se busca un sitio adecuado donde se quiera alojar:
```
$git clone https://github.com/ggerganov/llama.cpp.git

$cd llama.cpp

$mkdir -p build

$cd build
```
Una vez en el directorio "build" donde se compilarán los binarios se indica el "flag" de CUDA, y se compila:
```
$cmake .. -DGGML_CUDA=ON

$cmake --build . --config Release -j$(nproc)
```
Cuando haya terminado de compilar, aparecerá un directorio con los binarios "bin" dentro de "build".
Para este caso se va a utilizar "gpt-oss-20b-MXFP4.gguf" que normalmente hay que depositarlo en el directorio "models" que está dentro del directorio "llama.cpp".
Se puede hacer un test con:
```
$./build/bin/llama-cli -m models/gpt-oss-20b-MXFP4.gguf --n-gpu-layers 15 -p "Cuál es la capital de España"
```
Nos debería devolver la respuesta impresa en la terminal.
Ahora se trata de activar el servidor para tener un punto compatible con OpenAI API disponible en local:
```
$./build/bin/llama-server --model models/gpt-oss-20b-MXFP4.gguf --port 10000 --ctx-size 4096 --n-gpu-layers 15
```
En este caso si se abre un navegador y se accede a http://localhost:10000 se accede a un "front chat" donde se puede interactuar con el modelo en cuestión.

Para utilizar un "front" con más capacidades como Open WebUI lo más directo es utilizar Docker, porque sino el "front" nativo necesita nodejs.

Para utilizar Open WebUI desde Docker, primero se instalan las dependencias:
```
$sudo apt install ca-certificates curl gnupg lsb-release -y
```
y se configura el repositorio para la versión de la distribución:
```
$curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

$echo "deb [arch=$(dpkg --print-architecture) signed by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Una vez está configurado el repositorio de Docker:
```
$sudo apt update

$sudo apt install docker-ce docker-ce-cli containerd.io -y
```
Se añade el usuario al entorno de "sudoers" y se crea el grupo de entorno "docker":
```
$sudo usermod -aG docker $USER

$newgrp docker
```
Ahora se puede habilitar el acceso a un contenedor Open WebUI:
```
$docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```
Ahora en "localhost:3000" en un navegador se accede al comienzo de configuración. Una vez creada una cuenta se puede acceder a través del "badge" en la esquina superior derecha al apartado de "Admin Panel > Settings > Connections > OpenAI API > Manage OpenAI API Connections" se crea una conexión con los valores: "URL > API Base URL: localhost:10000/v1", "Auth: None", "Model ID: gpt-oss-20b-MXFP4"

Ahora creando una nueva conversación se podrá seleccionar el modelo y empezar a interactuar con este.