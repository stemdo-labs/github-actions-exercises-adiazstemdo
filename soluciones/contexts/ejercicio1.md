# Contexts - Ejercicio 1

## Configura un workflow que imprima información sobre el runner en el que se está ejecutando el job, como el nombre del sistema operativo, la arquitectura, y el espacio en disco disponible

Este es el código en el yml:
con ***uname -s*** y ***uname -m*** sacamos información del SO y de la arquitectura y con ***df -h*** sabemos el espacio lobre que queda

```
name: Información del Runner

jobs:
  mostrar_información_del_runner:
    runs-on: labs-runner
    permissions: write-all

    steps:
      - name: Mostrar nombre del sistema operativo
        run: echo "Sistema operativo $(uname -s)"

      - name: Mostrar arquitectura del sistema
        run: echo "Arquitectura $(uname -m)"

      - name: Mostrar información completa del sistema
        run: uname -a

      - name: Mostrar espacio en disco disponible
        run: df -h

      - name: Mostrar información del runner
        run: env | grep RUNNER
```

Por lo que sea no me ha funcionado este workflow a la hora de subirlo no se ejecuta correctamente a diferencia de los demás ejercicios.