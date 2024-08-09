---
layout: post
title: Crear entornos virtuales Python desde Terminal
tags: python virtualenv jekyll markdown
categories: Artículo
---

## Comandos para crear entornos virtuales Python en Debian o similares.

- Se revisa si hay alguna actualización pendiente:
```
sudo apt update; sudo apt upgrade
```

- Es necesario tener la dependencia *venv* para crear entornos, también en este caso se instala *Pip* para gestionar e instalar paquetes:
```
sudo apt install python3-venv python3-pip
```

- Una vez instalados, se actualiza *Pip*:
```
python3 -m update --upgrade pip
```
*[-m] en este caso indica manual*

- Ahora que tenemos las dependencias instaladas está lista para crear entornos.
- Se crea un directorio en el sitio apropiado donde se quieran alojar:
```
mkdir ~/entornos_python
```

- Se crea el entorno virtual:
```
python3 -m venv ~/entornos_python/nombre_del_entorno
```

- Una vez creado para activar el entorno:
```
source ~/entornos_python/nombre_del_entorno/bin/activate
```

- Ahora en la terminal aparecerá antes del prompt *(nombre_del_entorno)*, que nos indicará que estamos usando ese entorno como intérprete de Python.
- Al estar utilizando un entorno virtual, el interprete *Global* del sistema no se verá afectado por las modificaciones o instalaciones que se realicen en dicho entorno.

- Por ejemplo, ahora si se usa el comando:
```
python3 -m pip install --upgrade pip
```
- Solamente está afectando al gestor de paquetes que reside en dicho entorno. El *Pip* *Global* que se instaló previamente con *APT* no se verá afectado.
- De esta manera se pueden crear y destruir entornos, para probar dependencias y paquetes sin alterar la instalación Python del sistema.
