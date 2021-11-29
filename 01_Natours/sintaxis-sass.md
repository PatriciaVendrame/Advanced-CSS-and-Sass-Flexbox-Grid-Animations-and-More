# SINTAXIS DE SASS

- ANIDAMIENTO
  Cuando se usa BEM y se repite la en encabezado de las clases (block), se reemplaza con `&`

  ```
  header{}
  header__logo-box {}
  header__logo {}

  Sintaxis Sass anidamiento

  header {
    &__logo-box{}
    &__logo{}
  }
  ```
