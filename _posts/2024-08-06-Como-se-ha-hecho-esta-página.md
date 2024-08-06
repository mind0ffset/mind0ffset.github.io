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
- Un poco más arriba aparece que nuestro sítio está activo y aparece un enlace con el formato "https://nombredeusuario.github.io".
- Copiamos ese enlace.
- Vamos a la página inicial de código o code del repositorio, a la derecha aparece acerca de o about se clicka en la rueda de ajustes. Donde aparezca sitio web o website se pega el enlace.
- Ahora estaria configurado el repositorio para servir páginas estáticas.

## Integrar un tema Jekyll

- En el apartado de código del repositorio se crea un fichero con nombre _config.yml.
- Ese sirve la información global del blog.

1. línea #Site settings
2. línea title: el título del blog
3. línea author:
4. línea indentado dos espacios name: nombre de la persona que lo crea
5. línea indentado dos espacios email: correo del blog
6. línea description: "Descripción del blog." (entre comillas)
7. 
8. #Build settings
9. línea remote_theme: jekyll/minima <- este es el tema que tiene este blog pero se puede elegir otro.

