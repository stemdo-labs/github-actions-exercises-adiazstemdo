# Contexts - Ejercicio 3

## Configura un workflow que realice una tarea ficticia y luego envíe un mensaje de notificación a una URL (simulada con echo) usando el estado del job y el nombre del evento que lo desencadenó

En la documentación no encontré nada acerca de los estados en los workflows así que pedí ayuda tanto a chatGPT como a compañeros
Resulta que con ***if(always)*** se ejecuta el workflow sin importar si los steps anteriores funcionan o no.
Con el *curl* envío la notificación a la URL de Stemdo
Este workflow no sé por que pero me ha estado dando error a la hora de hacer el curl

```
name: Notificación a Stemdo

on:
  push:
  pull_request:

jobs:
  tarea-ficticia:
    runs-on: labs-runner

    steps:
      - name: Checkout del repositorio
        uses: actions/checkout@v3

      - name: Tarea ficticia
        run: |
          echo "Esto es solo un echo..."
          sleep 2
          echo "Tarea completada."

      - name: Enviar notificación
        if: always()  # Para que se ejecute independientemente del estado anterior
        run: |
          echo "Enviando notificación..."
          curl -X POST https://www.google.com/ \             # URL de Stemdo
            -H "Content-Type: application/json" \
            -d '{
              "estado": "${{ job.status }}",
              "evento": "${{ github.event_name }}"
            }'
          echo "Notificación enviada."
```