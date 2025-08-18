---
layout: post
title: "Tarea digital 3: un ejemplo comentado de Dublin Core"
date: 2021-09-22
author: Susanna Allés Torrent
---

**Fecha límite de entrega lunes 27 de septiembre antes de las 3:30pm**

A partir del ejercicio sobre la postal de Federico García Lorca con Elliot Williams debéis llevar a cabo la tarea número 3 que consiste en:

- Crear un nuevo post en vuestro blog del portfolio
- Para ello, debéis crear un documento en formato markdown en vuestro respositorio de GitHub que se llamará de la siguiente manera: `2021-09-27-metadatos.md`.
- Debéis editar los metadatos del inicio donde aparecen vuestro nombre, el título del post (podéis poner el que queráis pero que refleje de que se trata de un post que comenta un archivo de metadatos en Doublin Core), la fecha, etc.
- Este documento debe estar escrito en Markdown: https://www.markdownguide.org/basic-syntax/ y deberá contener: títulos (uno varios “headings” de distinto nivel), párrafos, enlaces, negritas o cursivas, y sobretodo “snippets” o fragmento de código.
- La plataforma con la que trabajamos el otro día os será útil para crear el archivo en Dublin Core: <http://metadataetc.org/ dctemplate.php>

El ejemplo seleccionado para la codificación y la creación del archivo Dublin Core esta [postal de Federico García Lorca](https://merrick.library.miami.edu/cdm/compoundobject/collection/chc5324/id/31/rec/19)

**Recordad escribir con cuidado y sin faltas de ortografía.**

La estructura de vuestro post debe ser parecida a la siguiente: 

1. Breve introducción sobre qué es Dublin Core, para qué sirve, quien lo hizo, quien lo utiliza, .... (unas 150 o 300 palabras). 
2. A continuación debéis comentar cada uno de los elementos que aparecen en el archivo y explicar qué función tiene cada elemento, poniendo mucha atención a escribir bien los fragmentos de código. Por ejemplo,  


En primer lugar aparece el prólogo XML ... 

````
<?xml version="1.0"?>
<metadata xmlns:dc="http://purl.org/dc/elements/1.1/">
...
</metadata>
````

El título corresponde a.. 

````
    <dc:title>El título</dc:title>
````
  
En la etiqueta "creador" se añade la información correspondiente a ...: 

 ````
    <dc:creator>
        Quien creó el recurso
    </dc:creator>
    <dc:subject>
        Palabra clave 1, Palabra clave 2, ...
    </dc:subject>
    <dc:description>
        Esta es la descripción del record...
    </dc:description>
 ````
 
 Otra explicación de otro elemento: 
 
 ````
    <dc:publisher>
        Quien lo publica
    </dc:publisher>
    <dc:contributor>
        Quienes contribuyen
    </dc:contributor>
    <dc:date>
        2021-09-22
    </dc:date>
    <dc:type>
        Imagen,...
    </dc:type>
    <dc:format>
        jpg
    </dc:format>
    <dc:identifier>
        Identificador
    </dc:identifier>
    <dc:source>
        La biblioteca
    </dc:source>
    <dc:language>
        español
    </dc:language>
````

3. En fin, debe cerrar vuestro texto una breve reflexión vuestra experiencia con la codificación de metadatos y el trabajo que se lleva a cabo en la biblioteca.
