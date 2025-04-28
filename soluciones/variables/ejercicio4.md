# Variables y Outputs - Ejercicio 4

## Configura un workflow que valide un archivo de configuración (config.json) y realice un despliegue condicional a develop o producción basado en la rama desde la cual se hace el push.

### Pasos:

- Verificar que el archivo config.json exista y que tenga el formato JSON válido.
- Extraer un valor específico del archivo config.json (por ejemplo, la versión de la aplicación) y utilizarlo como un output.
- Si el push se hace en la rama main, el workflow debería simular un despliegue de la aplicación en producción.
- Si el push se hace en una rama develop, el workflow debería simular un despliegue de la aplicación en develop.
- Si el archivo config.json no es válido, el workflow debería fallar y detenerse sin realizar el despliegue.

---

En lineas generales, lo que pide este ejercicio es crear un workflow que automatize el proceso de validación de un archivo de configuración y realizar un despliegue condicionalen función de la rama desde la cual se hace el push.

> Con la herramienta *jq* podemos procesar y manipular archivos JSON mediante linea de comandos. En este ejercicio lo utilizo un par de veces y es de suma importancia

***El ejercicio pide que se despliegue un mensaje en caso de que el push se realice desde la rama develop, pero en mi caso lo configuré para que lo hiciera desde la rama feature que es la rama en la que estoy trabajando todo el rato***

```
name: Variables 4. Validación de json y condicional 

on:
  push:       # Se ejecuta cada vez que se hace push desde la rama main o develop
    branches:
      - main
      - feature

jobs:
  validar-config:
    name: Version config.json
    runs-on: labs-runner
    outputs:
      app_version: ${{ steps.get_version.outputs.app_version }}

    steps:
      - name: Checkout code       # clona el código del repositorio para que el workflow pueda acceder a los archivos (como config.json).
        uses: actions/checkout@v3

      - name: comprobar si existe el archivo config.json
        run: |
          if [ ! -f config.json ]; then
            echo "config.json no existe"
            exit 1
          fi

      - name: Validar archivo .json
        run: |
          jq . config.json > /dev/null || { echo "Invalid JSON"; exit 1; }   # Con la herramienta jq Verifica si el archivo JSON es válido. Si no lo es, se imprime un mensaje de error y se sale con un código de error.

      - name: Version
        id: get_version
        run: |
          VERSION=$(jq -r '.version' config.json)    # Con jq, extrae el valor de la clave "version" del archivo config.json y lo almacena en la variable VERSION.
          echo "App version: $VERSION"
          echo "app_version=$VERSION" >> $GITHUB_OUTPUT    # Luego, se imprime el valor de la versión y se guarda en la variable de salida app_version para que pueda ser utilizada en otros trabajos del flujo de trabajo.

  deploy:
    name: Condicional if
    needs: Validar archivo .json
    runs-on: labs-runner

    steps:
      - name: Despliegue a Producción
        if: github.ref == 'refs/heads/main'    # Se ejecuta si la rama es main
        run: echo "Desplegar version ${{ needs.validate-config.outputs.app_version }} a PRODUCCIÓN"

      - name: Despliegue a Desarrollo
        if: github.ref == 'refs/heads/feature'    # Se ejecuta si la rama es develop
        run: echo "Desplegar version ${{ needs.validate-config.outputs.app_version }} a DESARROLLO"
```

- El workflow se asegura de que el archivo config.json exista y tenga el formato válido
- Si el archivo no es válido o no existe, el proceso se detiene para evitar errores en el despliegue
- Extrae la versión de la aplicación del archivo config.json
- Dependiendo de la rama en la que se haga push nos aparecerá un mensaje u otro