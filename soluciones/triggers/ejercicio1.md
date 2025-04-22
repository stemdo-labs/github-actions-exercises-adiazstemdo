# Triggers - Ejercicio 1

## Configura un workflow para que se ejecute cuando se abre un Pull Request

En la carpeta .github/workflows he creado el archivo *triggers1.yml* con la siguiente configuración:

```
name: Ejercicio Trigger 1

on:
  pull_request:
    branches:
      - main

jobs:
  Primer workflow:
    runs-on: labs-runner
    
    steps:
     - name: Mostrar mensaje
       run: echo "Workflow creado"
```

El workflow va a tener el nombre "Ejercicio Trigger 1" para la rama *main* en un contenedor *labs-runner* que nos ha dado Stemdo.
Creamos un nuevo pull-request y esta sería la vista desde la pantalla "Actions"

Para que esto se ejecute he creado la rama *prueba* , he añadido los nuevos archivos y he hecho un commit. 
Se ha ejecutado correctamente!
![alt text](../../auxiliar/trigger.png)