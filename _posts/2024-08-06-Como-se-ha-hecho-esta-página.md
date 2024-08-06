---
layout: post
title: Como se ha hecho este blog - GitHub Pages y Jekyll.
tags: githubpages jekyll markdown
categories: Artículo
---

## Temas de Jekyll en githubpages con MarkDown

Esto es una descripción de un [tutorial](https://evanwill.github.io/go-go-ghpages-b/content/3-blog.html) "Exploring Jekyll Basic via a Blog".

## Crear un repositorio público en github.

- Se accede a GitHub, se pulsa en el "+" de arriba a la derecha.
- Se hace click en "Nuevo repositorio".
- Se puede hacer como se indica en [GitHub Pages](https://pages.github.com) añadiendo como el formato "nombredeusuario.github.io" al nombre de repositorio 
- En este caso "nombredeusuario" es el nombre del perfil de github.
- Se puede clickar en "Add a README file" con la descripción, para que el repositorio no esté vacío.
- Dentro del repositorio en la parte de arriba hay una rueda de configuración. Ahí se puede encontrar el apartado "Pages" en las categorias de la izquierda.
- Una vez seleccionado "Pages", se indica la Fuente "Source" - Deploy from Branch. En el apartado "Branch" se puede seleccionar la ruta, origen o root debería estar seleccionado si el repositorio no está vacío.
- Se puede seleccionar cualquier otro directorio que tengamos. En este caso se ha seleccionado origen o root como punto de partida.
- Un poco más arriba aparece que el sitio está activo y aparece un enlace con el formato "https://nombredeusuario.github.io".
- Copiamos ese enlace.
- En la página inicial de código o code del repositorio, a la derecha aparece acerca de o about se clicka en la rueda de ajustes. Donde aparezca sitio web o website se pega el enlace.
- Ahora estaria configurado el repositorio para servir páginas estáticas.

## Integrar un tema Jekyll

- En el apartado de código del repositorio se crea un fichero con nombre _config.yml.
- Ese sirve la información global del blog.
- Se edita el fichero con el siguiente contenido.

```
# Site settings
title: título del blog
author:
  name: quien edite el blog
  email: correo del blog
description: "descripción del contenido del blog."

# Build settings
remote_theme: jekyll/minima

```
- remote_theme: jekyll/minima <- se puede modificar el tema que se quiera usar. En este caso es "minima"
- Se describe el commit y se realiza el commit.
- Ahora se crea otro fichero con nombre index.md
- Se edita el fichero con el contenido.

```
---
layout: home
---

# Bienvenidos al blog

```
- Se describe el commit y se realiza el commit.

## Agregar posts o artículos

- Los posts tienen que tener un formato de nombre como aaaa-mm-dd-título.md (fecha antes del título seguidos de '-' )
- Se crea un nuevo fichero.
- En el campo del nombre se ingresa como _posts/ esto crea un subdirectorio.
- Ahora aparece de nuevo el campo hábil para nombrar el fichero, ingresamos el formato aaaa-mm-dd-título.md del nuevo post
- A continuación se muestra una plantilla básica de un post o artículo.

```
---
layout: post
title: el nombre del post
tags: etiquetas del post
categories: temas o categorias del post
---

# Encabezado pesado
## Encabezado ligero

Este es el primer párrafo del artículo.
Comienza el artículo.

Fuente puede ser *Itálica* o **Consistente**

Listas de puntos se crean con '*', '+' o '-' delante de cada línea

- ToDo
- Primera cosa
- Segunda cosa
- Tercera cosa

Las listas numeradas son con un número seguido de '.'

1. uno
2. dos
3. tres
4. cuatro

Bloques de texto son párrafo hasta la siguiente línea vacía.
Mantener una línea vacía entre encabezado y párrafos.

Trozos de código se pueden resaltar con 'comillas'
Enlaces tienen un formato [Texto](https://)

```
- Se describe el commit y se realiza el commit.
- And, that's it! esto sería lo básico para crear artículos en un blog alojado como repositorio en GitHub Pages con formato MarkDown gracias a Jekyll
- Para más información sobre como personalizar el blog [evanwill.github.io](https://evanwill.github.io/go-go-ghpages-b). Blog es en inglés.
