# Triggers - Ejercicio 3

## Configura un workflow para que se ejecute cuando se cree una nueva Issue

Para los issues necesitamos el evento ***on: issues***. con ***types: [opened]*** especificamos que el workflow se ejecutará cuando se abra una nueva issue

```
name: Ejecutar en creación de Issue

on:
  issues:
    types: [opened]  # Esto ejecuta el workflow cuando se abre una nueva Issue

jobs:
  ejecutar-al-crear-issue:
    runs-on: ubuntu-latest
    steps:
      - name: Imprimir mensaje cuando se crea una issue
        run: |
          echo "Se ha creado una nueva issue con el título: ${{ github.event.issue.title }}"     # Variable predefinida de issues
          echo "Descripción de la issue: ${{ github.event.issue.body }}"                        # Variable predefinida de issues

```

