# CSS AVANZADO - SASS - FLEXBOX - GRID

## PILARES DEL DISEÑO WEB

### - RESPONSIVE DESIGN

O diseño adaptable significa crear un sitio web que funciona a la perfección en todos los tamaños de pantallas en todos los dispositivos.
Fundamentos:

- Fluid layouts
- Media queries
- Responsive images
- Correct units
- Desktop-first vs mobile-first

### - CODIGO MANTENIBLE Y ESCALABLE

- Escribir código limpio
- Fácil de entender
- Mantenible a futuro
- Reutilizable
- Arquitectura CSS, en la forma de organizar los archivos, nombramiento de clases y estructura del HTML

### - RENDIMIENTO WEB

Sitio web más eficaz, más rápido y reducir su tamaño para que el usuario tenga que descargar menos datos

- Menor cantidad de solicitudes HTTP
- Incluir la menor cantidad posible de archivos
- Escribir la menor cantidad posible de código
- Comprimir el código con el preprocesador Sass
- Reducir el uso de imágenes, sólo las necesarias

---

## COMO TRABAJA CSS, EL DETRÁS DE ESCENA

Lo que sucede cuando un usuario abre una página web
El punto de partida es cuando el navegador se inicia para cargar el archivo index.html, toma el código HTML cargado y lo decodifica línea por línea. En este proceso el navegador crea el DOM que básicamente describe todo el documento web con elementos padres, hijos y hermanos y es donde se almacena le código HTML decodificado.
En el análisis del html encuentra las hojas de estilos y también las carga. Css también se analiza aunque de una manera mas compleja. En el proceso hay dos pasos principales: a- se resuelven los conflictos en las declaraciones de css, b- se procesan los valores finales de CSS (margen de 50% en computadoras o en telefonos tendran valores distintos que se resuelven en esta etapa). Finalizado el proceso de analisis, CSS se almacena en el Modelo de Objetos CSS, similar al DOM.
DOM y Modelo de Objetos de CSS forman un Arbol Renderizado que contiene todo lo necesario para representar la página web.
Para renderizar realmente una página web, el navegador usa el Modero de formato visual (Visual Formatting Model), finalizado el proceso se muestra el sitio web final

---

## COMO SE PROCESA CSS EN EL NAVEGADOR: CASCADA Y ESPECIFICIDAD

- Primera fase del análisis de CSS: reolver los conflictos en las declaraciones de CSS (cascada)
- Segunda fase del análisis de CSS: procesar los valores finales de CSS

Cascada es el proceso de combinar diferentes hojs de estilo y resolución de conflictos entre diferentes reglas y declaraciones CSS cuando mas de una regla se aplica a un determinado elemento.

CSS puede PROVENIR DE DISTINTAS FUENTES:

- el mas común es el que escriben los desarrolladores llamadas DECLARACIONES DE AUTOR
- Otras declaraciones de CSS proceden del usuario (ejemplo cuando el usuario cambia el tamaño de la fuente del navegador)
- Las declaraciones por defecto del navegador (User Agent)

Cascada combina las declaraciones de CSS provenientes de todas las fuentes.
Los conflictos en la cascada se resuelven segun la importancia, la especificidad del selector y el orden de las declaraciones, asi determinar cual tiene prioridad

IMPORTANCIA:

- 1- Usar `!important` por el usuario
- 2- Usar `!important` por el desarrollador
- 3- Declaraciones del desarrollador
- 4- Declaraciones del usuario
- 5- Declaraciones por defecto del navegador

ESPECIFICIDAD
Cuando todas las declaraciones tienen la misma IMPORTANCIA CSS calcula y compara la especificidad de los selectores segun la importancia de los mismos:

- 1- Estilos Inline
- 2- IDs
- 3- Clases, pseudoclases, atributos
- 4- Elementos, pseudoelementos
- El selector universal no tiene valor de especificidad (su valor es (0, 0, 0, 0))

EJEMPLOS:

(0, 0, 1, 0) -> (Inline, IDs, Clases, Elementos) se cuenta el numero de ocurrencias en el selector

`.button { } -> (0, 0, 1, 0)`

- una sola clase

`nav#nav div.pull-right .button { } -> (0, 1, 2, 2)`

- elemento#IDs elemento.clase clase

ORDEN DE DECLARACION
Si todos los valores tienen la misma importancia, la misma especificidad entonces la última declaración es la que se utilizará para diseñar el elemento seleccionado.

CONSIDERACIONES IMPORTANTES

- Usar !important en caso de mucha necesidad
- Los estilos en linea tienen mayor prioridad que los estilos de las hojas externas
- Un selector con ID tiene mayor especificidad que uno con 1000 clases
- Un elector con 1 clase tiene mayor especificidad que uno con 1000 elementos
- El selector universal no tiene ESPECIFICIDAD
- Es mejor confiar siempre en una mayor especificidad que en el orden de los selectores
- Es muy importante el orden en que se declaran las hojas de estilos externas ya que sobreescriben a la anterior

---

## COMO SE PROCESAN LOS VALORES EN UNA FASE DE ANALISIS DE CSS

```
<div class="section">
  <p class="amazing"> Css is absolutely amazing <p/>
</div>
```

```
.section {
  font-size: 1.5rem;
  width: 280px;
  backfround-color: orangered;
}

p {
  width: 140px;
  background-color: green;
}

.amazing {
  width: 66%;
}
```

- VALORES DECLARADOS: width (parrafo) entran en conflicto dos declaraciones las del elemento de 140px y la de la clase el 66%
- VALOR DE CASCADA: segun la especificidad tiene mas peso el valor declarado en la clase
- VALOR ESPECIFICO: width: 66%; es el valor predeterminado de una determinada propiedad CSS
- VALOR COMPUTADO: en este paso los valores con unidades relativas se convierten en pixeles para que puedan heredarse, las palabras claves como naranja por ejemplo se reemplazan. 66% no es técnicamente una unidad no pasa nada en este paso.
- VALOR USADO: en este paso de cálculo, el motor de CSS utiliza la renderizacion para descubrir los valores restantes, por ejemplo darle valor a los valores porcentuales que depedenden del diseño. En este caso toma el 66% del valor del padre y le otorga ese ancho al párrafo (280px x 66% = 184.8px) y este es el valor usado
- VALOR ACTUAL: las restricciones del navegador y los distintos dispositivos hacen que se usen valores sin coma decimal, por lo que se redondea, en este caso el valor actual es 185px

Hay propiedades a las que no se les ha declarado un valor no tienen un VALOR DECLARADO y tampoco un VALOR EN CASCADA, estas tienen un valor por defecto o valor inicial que es un VALOR ESPECIFICADO. Este valor va a ser el VALOR COMPUTADO, VALOR USADO Y VALOR ACTUAL.

Las propiedades que tienen unidades relativas, estas unidades son el VALOR DECLARADO, el VALOR DE CASCADA y el VALOR ESPECIFICADO, en la etapa del VALOR COMPUTADO es donde se le otorga el equivalente a valores en pixeles y este valor es el VALOR USADO y el VALOR ACTUAL.
Los elementos hijos del padre heredan estos valores y los aplican. Estos valores heredados son los VALORES ESPECIFICADOS, COMPUTADOS, USADOS Y ACTUALES.

---

## COMO SE CONVIERTEN LAS UNIDADES RELATIVAS A ABSOLUTAS

Hay diferencia entre:

- % de fuente
- % de longitud

Una fuente del 150% de un elemento, sera el 150% del tamaño de la FUENTE de su elemento padre

Un % de longitud es el que se aplica a la altura, padding, margin etc
la referencia es siempre el ANCHO del elemento padre

Valores basados en la FUENTE:

- em (font) la referencia es el valor de la fuente del elemento padre
- em (lengths) la referencia es el valor de la fuente del elemento actual
- rem la referencia el el valor de la fuente raíz del documento tanto para la fuente como para la longitud

  - em -> utlilian el elemento principal o actual como referencia
  - rem -> utiliza el tamaño de fuente de la raíz como referencia
  - em para fuente la referencia es el tamaño de fuente calculado del padre (similar a lo que ocurre con los %)
  - em para longitudes como por ejemplo el padding, usa como referencia el tamaño de FUENTE ACTUAL presente en el elemento al que se le aplica el padding, si la fuente actual es 24px y padding: 2em; entonces 2em x 24px = 48px (no considera el tamaño de fuente del padre)

Valores basados en el VIEWPORT:

- vh: valor del 1% del alto del area de visualización
- vw: valor del 1% del ancho del area de visualización

---

## HERENCIA

La herencia es una forma de propagar los valores de las propiedades de los elementos padres a sus hijos. Hay algunas propiedades que se heredan y otras no.
Si no hay un valor especificado o por cascada, entonces se hereda el valor de la propiedad siempre que sea heredable, en caso que no lo sea, el valor especificado sera el valor inicial de la propiedad del elemento que también es específico de cada propiedad, por ejemplo en la propiedad padding el valor es 0.
Como regla general, las propiedades relacionadas con texto se heredan. Propiedades como margin, padding, etc no se heredan
Lo que se hereda es el VALOR CALCULADO no se hereda el valor declarado.
La herencia de una propiedad solo funciona si ni el desarrollador ni el navegador declara un valor para esa propiedad.
Se puede usar la palabra "inherit" para forzar la herencia de una determinada propiedad.
Se puede usar la palabra "initial" para restablecer la propiedad a su valor inicial

---

## PASAR DE PX A REM

Es una manera fácil para cambiar todas las medidas de la página con una configuracion simple (cuando llegamos a un punto de quiebre)

---

## THE VISUAL FORMATTING MODEL (MODELO DE FORMATO VISUAL DE CSS)

Es un algoritmo que calcula cajas y determina el diseño de estas cajas, para cada elemento del árbol renderizado, para determinar el diseño final de la página.
El algoritmo tiene en cuenta factores como:

- Dimensiones de las cajas: box model
- Tipo de caja: inline, block e inline-block
- Esquema de Posicionamiento: floats, posicionamiento absoluto y relativo
- Contexto de apilamiento
- Otros elementos presentes en el árbol de renderizado como hermanos o padres
- Informacion externa como tamaño del viewport, dimensiones de la imágenes, etc

Al juntar todos estos elementos, el navegador determina como se verá el sitio web final para el usuario.

BOX MODEL:

- Content: el contenido de una caja que puede ser texto, imagenes, etc
- Height y width: el alto y el ancho que se definen segun propiedades, si no se definen se adaptan al contenido de la caja
- Padding: es un área transparente alrededor del contenido, dentro de la caja. Se usa para generar espacios en blanco dentro de una caja
- Border: rodea el padding y el contenido
- Margin: el espacio entre cajas, esta fuera de la propia caja
- Fill area: el área que ocupa cuando se establece la propiedad background-color o background-image, el relleno incluye el contenido, el padding y el borde pero no al margin

El tamaño de una caja es la suma de los bordes, el padding y el contenido, esto estira el tamaño de la caja definido. Para solucionarlo se utiliza la propiedad box-sizing: border-box.

Cuando se definen height y width se definen para el contenido, el padding y el borde. Al usar el modelo border-box se achica el área de contenido debido al agregado de padding y bordes quedando el tamaño de la caja el definido en las propiedades width y height

### TIPOS DE CAJAS

- BLOCK:
  - los elementos se visualizan en bloque, uno debajo de otro
  - toman el 100% del ancho del elemento padre, crea espacios antes y después de la caja
  - se aplica box-model
  - display: block | display: flex | display: list-item | display: table
- INLINE:
  - los elementos se disponen en líneas, solo ocupa el espacio que necesita para su contenido
  - no genera saltos de línea antes y después del contenido
  - no se puede aplicar propiedades de height y width
  - padding y margin solo se puedens estableces de manera horizontal, es decir, a la izquierda y derecha del elemento
  - display: inline
- INLINE-BLOCK:
  - es una caja inline pero que se pueden aplicar las propiedades como a los elementos block. Como son inline usan el espacio de su contenido y no causan saltos de línea pero son elementos block en su interior por lo que se aplica box-model
  - display: inline-block

### ESQUEMAS DE POSICIONAMIENTO

- NORMAL FLOW: no se aplica ningun estilo en absoluto
  - Esquema de posicionamiento por defecto
  - No aplica floats
  - No aplica posicionamiento absoluto y relativo
  - Los elementos siguen el orden naturan el el que se declaran y se presentan en la página
  - Default -> position: relative
- FLOATS: hace que un elemento se tome por completo fuera del flujo normal y se mueva a la izquierda o derecha hasta que toque el borde de su caja contenedora, u otro elemento flotante
  - Los elementos son removidos del flujo normal
  - Textos y elementos inline se ajustan alrededor del elemento flotante
  - Cuando un elemento flota, su contenedor no ajusta su altura al elemento
  - Default -> float: left | float:right
- POSICIONAMIENTO ABSOLUTO: cuando se establece la propiedad absoluta o fixed el elemento se saca del flujo normal
  - El elemento no tiene impacto con el contenido alrededor o elementos con posicionamiento absoluto, incluso puede superponerlos
  - Para posicionar un elemento con posicion absoluta usar las propiedades top, bottom, left, right para desplazarlo por su contenedor relativamente posicionado.
  - CONTEXTO DE APILAMIENTO: se utiliza la propiedad z-index para posicionar elementos sobre otros

---

## ARQUITECTURA CSS - COMPONENTES - BEM

- Pensar el diseño que se quiere hacer, para esto crear componentes que unidos crean el diseño general de la página. Los componentes deben ser rehutilizables en el proyecto y para otros proyectos, deben ser independientes, es decir, no deben depender de sus elementos padres. Componentes es una parte del Atomic Design
- Crear el HTML y CSS de la página usando BEM

  BEM -> Block Element Modifier

  ```
  .block{}
  .block__element {}
  .block__element--modifier{}
  ```

  BLOCK -> es un componente independiente que es significativo por sí solo

  ELEMENT -> es una parte del bloque y no tiene significado por si solo. Si se saca uno de estos elementos del bloque no serían útiles en absoluto

  MODIFIER -> es una bandera que podemos poner en un bloque o un elemento para hacerlo diferente de los bloques o elementos regulares, hacer una versión diferente de los mismos

- Arquitectura CSS: crear una carpeta de estructura de archivos, PATRON 7-1:

  - 7 carpetas diferentes para archivos Sass parciales
  - 1 archivo principal Sass donde importar todos los archivos parciales en una hojas de estilos CSS compilada
  - las 7 carpetas son:
    - base/ -> definiciones básicas
    - components/ -> un archivo para cada componente
    - layouts/ -> donde se define el dieseño general del proyecto
    - pages/ -> estilos para páginas específicas del proyecto
    - themes/ -> si se impementan diferentes temas visuales
    - abstracts/ -> código que no genera ningún CSS como variables o mixins
    - vendors/ -> todo el CSS de terceros
      No siempre se utilizan todas las carpetas, dependerá del tamaño y alcance del proyecto

---

# SASS

SASS es un preprocesador de CSS, una extensión de CSS que agrega mucha potencia y elegancia al lenguaje básico
Se usa SASS para solucionar los problemas que tenemos con CSS que se vuelve muy complicado, muy rápido.
Como funciona: en vez de escribir un solo archivo CSS con código CSS normal, se escribe SASS en un archivo SASS, luego se ejecuta el compilador y éste convierte el código SASS en código CSS normal

Características

- Variables: para valores rehusables
- Nesting: selectores dentro de otros para escriibir menos código
- Operators: para operaciones matemáticas dentro de CSS
- Partials and imports: para escribir CSS en diferentes archivos e importalos a un solo archivo
- Mixins: para escribir piezas de código rehusables
- Functions: similar a los mixins, la diferencia es que producen un valor que se puede utilizar mas tarde
- Extends: para hacer que diferentes selectores hereden declaraciones que son comunes a todos ellos
- Control directives: permiten a los desarrolladores escribir código complejo usando condicionales y bucles

Hay dos sintaxis de SASS, una que es SASS y la otra SCSS
SASS usa sangria
SCSS usa llaves y ;

## INSTALAR SASS

TERMINAL DEL PROYECTO

- Instalar Node
- En consola del proyecto ejecutar `npm init`
- Crear el archivo package.json que solicita
- Instalar Sass como dependecia de desarrollo `npm install node-sass --save-dev` ya que es una herramienta de desarrollo que solo ayudará a construir el proyecto.
  Se incluye esta dependencia de desarrollo en el arhivo package.json ya que al compartir el proyecto no es necesario compartir la carpeta node-modules. Quien recibe el proyecto, ejecuta `npm install` y todas las dependencias se instalan automaticamente
- Crear una carpeta en el proyecto con el nombre scss, crear un archivo en la carpeta con el nombre main.scss el cual contendra todo el codigo en scss
- Para que compile a css, incluir en el archivo package.json un script con el compilador, el archivo de origen, el archivo de destino y la bandera -w que significa watch lo que hace es estar atento a todos los cambios del codigo.

```
"scripts": {
  "compile:sass": "node-sass archivo.scss archivo.css -w"
},
```

- Para correr el script ejecutar en la terminal

```
npm run compile:sass
```

- Instalar live server a nivel global ` npm install live-server -g`

  Para ejecutar live server en la carpeta del proyecto abrir la terminal y escribir ` live-server`

## PATRÓN 7-1

Se crean 7 carpetas y un archivo SASS principal para importar todos los archivos que se encuentran en estas carpetas

- CARPETA BASE: se crea dentro de la carpeta SASS, alli es donde se ponen las definiciones básicas de proyectos. Crear dentro el archivo base.scss que contendrá los conceptos básicos reales como resets y estilos para el HTML y selectores.
  Este archivo debe ser parcial para que luego se pueda importar al archivo principal de SASS
  Los archivos parciales comienzan con un guión bajo, para llamar a este archivo base escribir `_base.scss`
  En el archivo main.scss se importa el archivo base escribiendo `@import "base/base` asi se llama al archivo \_base.scss. sin guión bajo ni .scss
  En esta carpeta se incluyen archivos como \_animations.scss, \_typography.scss, \_utilities.scss

- CARPETA ABSTRACTS: el código que incluye no genera ningún css. Incluye variables, mixins, etc

- CARPETA COMPONENTS: se agrega un archivo para cada uno de los componentes. Los componentes son bloques de construcción reutilzables que componen un sitio web o aplicaciones. También deben poder utilizarse en cualquier lugar de la página para que sean completamente independientes y se mantengan unidos por el diseño de la página.

- CARPETA LAYOUTS: cada pieza del diseño global de todo el proyecto. Contiene archivos como footer, encabezado, etc. Los componentes son los elementos rehutilizables y en esta carpeta se juntan para formar una web

- CARPETA PAGES: para los archivos específicos como una página de inicio se puee crear un nuevo archivo para esa página específica y se realiza en la carpeta pages
