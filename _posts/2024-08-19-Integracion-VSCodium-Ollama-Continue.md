---
layout: post
title: AI local en IDE - Como usar LLM en VSCodium (Ollama y Continue) en Linux **Ubuntu 24.04**
tags: Ai AI IDE local LLM Ollama Continue Visual Studio
categories: Artículo
---

##Si se usa esta guía, la persona asume los riesgos que conlleva modificar el entorno y el producto de esas modificaciones.

##No se garantiza ningún resultado óptimo.

##Esta guía asume un entorno Ubuntu 24.04 fresco.

##No se garantiza la estabilidad de la máquina.

##No es un diseño para producción. 

##Exclusivo 

##USO PERSONAL.

Esta guía es una recopilación del artículo de *The Register* [*Who needs GitHub Copilot when you can roll your own AI code assistant at home*](https://www.theregister.com/2024/08/18/self_hosted_github_copilot). En el cual se describe como integrar un asistente virtual en un IDE para complementar la tarea de programación.
Los pasos a seguir en el artículo inicial, estaban segmentados en tres capítulos. Con enlaces a los diferentes secciones para llegar a completarlo.
Mi idea fue integrar todos los pasos en un único artículo, eso sí, sin la excelente prosa de *The Register*.

## Pre-requisitos a instalar *CUDA*.

- [GPU compatible con *CUDA*.](https://developer.nvidia.com/cuda-gpus)
```
lspci | grep -i nvidia
```

- [Sistema *Linux* con soporte.](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/#system-requirements)
```
uname -m && cat /etc/*release
```

- Sistema con los Kernel Headers correctos y Development Packages instalados.
```
sudo apt update; sudo apt upgrade
uname -r
sudo apt install linux-headers-$(uname -r) build-essential
```

- *GCC* instalado.
```
gcc --version
```

##Instalar *CUDA* Toolkit desde repositorio en red para *Ubuntu*.

- Instalar nuevo paquete cuda-keyring.
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
```
- Instalar *CUDA* Toolkit.
```
sudo apt update
sudo apt install cuda-toolkit-12-6
```

- Instalar los controladores de *Nvidia* más recientes.
```
nvidia-smi
apt search nvidia-driver
```

- Si están los últimos ya está, si no instalarlos con *sudo apt install [nombre del paquete del driver]* y reiniciar.


##Instalar *Ollama*.

```
curl -fsSL https://ollama.com/install.sh | sh
```
- Los modelos que se van a utilizar son:
- Llama 3 8B: LLM de uso general desarrollado por **Meta**, usado para comentar, optimizar y/o refactorizar código.
- Nomic-embed-text: Modelo integrado que se usa para indexar el codebase localmente y poder referenciarlo al chatbot.
- Starcoder2:3B: Modelo de generación de código desarrolladp por **BigCode**, para activar el autocompletado de **Continue**
```
ollama pull llama3
ollama pull nomic-embed-text
ollama pull starcoder2:3b
```
- Alrededor de ~7GB de almacenamiento se utilizaran.

##Instalar *VSCodium*.

- Se instala desde la propia tienda de aplicaciones de *Ubuntu*.

- Si se quiere instalar sin snap hay instrucciones en la página [*VSCodium*](https://vscodium.com/).

##Instalar *Continue* en *VSCodium*.

- Se abre el IDE.
- Se selecciona el icono de la izquierda que abre el panel de extensiones.
- Se busca *Continue*.
- Se instala y aparece la página de bienvenida a la izquierda que nos indica si es a través de API o local, seleccionamos **Local Models** y se selecciona abajo **Continue**.

- Los comandos para usar *Continue* están listados como:
-Cmd/Ctrl + L = Seleccionar código.
-Cmd/Ctrl + Shift + L = Seleccionar código para seguirlo.
-Cmd/Ctrl + I = Editor rápido.
-Cmd/Ctrl + Shift + R = Terminal de debug automática.

- Si se pulsa Cmd/Ctrl + I y se tiene seleccionado *Continue* a la izquierda. Aparece un prompt que permite verbalizar el código que se quiere representar en la página abierta.

#Si se quiere deshabilitar la telemetría de *Continue*.
- En el apartado de plug-ins aparece una rueda de ajustes debajo de *Continue*. Al pulsar aparecen los ajustes del plug-in.
- Hacia abajo aparece *Continue-Telemetry Enabled*. Pulsamos para deshabilitar la telemetría.
