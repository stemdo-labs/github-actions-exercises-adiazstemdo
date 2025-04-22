# Triggers - Ejercicio 2

## Configura un workflow para que se ejecute cuando se haga un push en la rama develop y simplemente imprima "Hello, World!" en la consola

Similar al primer ejercicio pero he cambiado el archivo yml con la siguiente configuración:

```
name: Hola Mundo en Develop

on:
  push:
    branches:
      - develop

jobs:
  Hola-Mundo:
    runs-on: labs-runner

    steps:
      - name: Imprimir Hola Mundo
        run: echo "Hello, World!"
"
```

Adjunto las imágenes que muestran el proceso:

![alt text](../../auxiliar/trigger2.png)

![alt text](../../auxiliar/trigger2.1.png)

![alt text](../../auxiliar/trigger2.2.png)