Función _find_overlap:
Se implementó una nueva función llamada _find_overlap que verifica si existen eventos que se superponen en el mismo escenario. Esta función toma como argumentos la fecha de inicio, fecha de finalización y el ID del escenario. Utiliza una consulta en la base de datos MongoDB para encontrar eventos que cumplan con las siguientes condiciones:

La fecha de inicio del evento existente es menor que la fecha de finalización del evento a crear/editar.
La fecha de finalización del evento existente es mayor que la fecha de inicio del evento a crear/editar.
El evento existente y el evento a crear/editar tienen el mismo escenario.
El evento existente no está marcado como eliminado.
El evento existente no es el mismo evento que se está editando (en caso de edición).
Función create:
Antes de crear un nuevo evento, se llama a la función _find_overlap para verificar si hay eventos existentes que se superpongan con el nuevo evento en el mismo escenario. Si se encuentra algún evento que se superponga, se lanza una excepción de tipo EventsOverlap, indicando qué eventos están superpuestos.

Función update:
Antes de actualizar un evento existente, también se llama a la función _find_overlap para verificar si la actualización causaría superposición con otros eventos existentes en el mismo escenario. Si se encuentra algún evento que se superponga, se lanza una excepción de tipo EventsOverlap.

Estas modificaciones garantizan que al crear o editar eventos, se realice una verificación para evitar la superposición de fechas y horas en el mismo escenario. De esta manera, se mejora la integridad de los datos en la base de datos y se previene la creación de eventos que podrían causar conflictos en la programación de un escenario en particular.
Configuración
Crea un archivo config.py en el directorio raíz del proyecto.

Agrega la configuración necesaria en config.py:

python
Copy code
MONGO_URI = 'mongodb://localhost:27017/nombre-de-la-base-de-datos'
# Agrega otras configuraciones si es necesario
Uso
Activa el servidor:

Copy code
python app.py
La API estará disponible en http://localhost:5000.

Endpoints
GET /events: Obtiene una lista de eventos con filtros y paginación.
GET /events/{event_id}: Obtiene los detalles de un evento específico.
POST /events: Crea un nuevo evento.
PUT /events/{event_id}: Actualiza los detalles de un evento existente.
Ejemplos de solicitudes
Obtener lista de eventos:

bash
Copy code
GET http://localhost:5000/events?limit=10&offset=0
Obtener detalles de un evento:

bash
Copy code
GET http://localhost:5000/events/5f6abecf4f3b48c7d49c63b4
Crear un nuevo evento:

bash
Copy code
POST http://localhost:5000/events
Content-Type: application/json

{
  "name": "Concierto de Verano",
  "start_date": "2023-08-01T18:00:00Z",
  "end_date": "2023-08-01T22:00:00Z",
  "stage_id": "5f6abecf4f3b48c7d49c63b5",
  "talents_ids": ["5f6abecf4f3b48c7d49c63b6"]
}
Actualizar detalles de un evento:

bash
Copy code
PUT http://localhost:5000/events/5f6abecf4f3b48c7d49c63b4
Content-Type: application/json

{
  "name": "Concierto de Otoño",
  "end_date": "2023-09-15T22:00:00Z"
}
Contribución
Si encuentras algún problema o tienes sugerencias de mejora, no dudes en abrir un issue o enviar un pull request.

