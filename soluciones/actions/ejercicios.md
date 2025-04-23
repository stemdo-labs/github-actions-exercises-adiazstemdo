# Actions - Ejercicio 1

## Crear una action personalizada que sea capaz de crear un archivo de texto en el directorio raíz del repositorio con un mensaje

Este es el archivo **action.yml** con la siguiente configuración

```
name: Crear archivo de texto
description: Crea un archivo de texto con un mensaje personalizado

inputs:
  filename:
    description: Nombre del archivo a crear
    required: true
  message:
    description: Contenido del archivo
    required: true

runs:
  using: "composite"
  steps:
    - name: Crear archivo
      shell: bash
      run: |
        echo "Creando archivo '${{ inputs.filename }}' con el mensaje:"
        echo "${{ inputs.message }}"
        echo "${{ inputs.message }}" > "${{ inputs.filename }}"

```

Y este es el archivo **actions1-workflow.yml** que sería nuestro workflow que va a llamar al archivo **action.yml** para crear el archivo

```
name: Ejecutando la creación del archivo

on:
  workflow_dispatch:

jobs:
  Ejecucion-del-workflow:
    permissions: write-all
    runs-on: labs-runner
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Ejecutar acción personalizada
        uses: ./.github/actions/crear-archivo
        with:
          filename: saludo.txt
          message: "¡Hola desde una acción personalizada sin script!"


      - name: Configurar Git
        run: |
          git config user.name "github-actions [bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"

      - name: Añadir archivos y pushear
        run: |
          git add .
          git commit -m "Auto-commit desde workflow"
          git push
```

![alt text](../../auxiliar/action1.png)

# Actions - Ejercicio 2

## Crear una custom action usando composite actions que imprime el nombre del autor del último commit en los logs del workflow

