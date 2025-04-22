# Triggers - Ejercicio 2

## Configura un workflow para que se ejecute cuando se haga un push en la rama develop y simplemente imprima "Hello, World!" en la consola

Similar al primer ejercicio pero he cambiado el archivo yml con la siguiente configuración:

```
name: Ejercicio Trigger 2

on:
  push:
    branches:
      - develop

jobs:
  build:
    runs-on: labs-runner
    
    steps:
     - name: Mostrar mensaje
       run: echo "Hello World!"
```
