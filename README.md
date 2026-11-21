# Práctica Préstamo de Equipo Odoo

**Estudiante:** Allysson Maldonado Henriquez

**Número de cuenta:** 1101200500801

## Preguntas y respuestas

## Paso 1. Catálogo de equipos

1. ¿Qué pasa con un equipo cuando se desmarca el campo Activo? ¿Por qué es mejor que borrarlo?

Cuando se desmarca el campo Activo, el equipo queda archivado y deja de mostrarse normalmente en las listas de registros activos. Es mejor archivarlo que borrarlo porque se conserva su información y su historial, lo que permite consultar sus datos posteriormente si se necesita.

2. ¿Para qué sirve copy=False en el campo codigo?

El atributo copy=False evita que el código de inventario se copie automáticamente cuando se duplica un registro. Esto es importante porque cada equipo debe tener su propio código de inventario y no se deben generar equipos con códigos repetidos por accidente.

## Paso 2. Registro de préstamos

1. Este modelo usa \_name y \_inherit al mismo tiempo. ¿Qué hace cada uno aquí?

\_name define el nombre del nuevo modelo que estamos creando, en este caso prestamo.prestamo. Por otro lado, \_inherit permite heredar las funcionalidades de otro modelo, en este caso mail.thread, para agregar el chatter y tener un historial de mensajes y cambios en el registro de préstamo.

2. ¿Qué hace tracking=True en el campo state? Lo verá funcionar en el paso 4.

tracking=True permite que Odoo registre los cambios que se realizan en ese campo dentro del chatter. En este caso, cuando el estado del préstamo cambie de Borrador a Prestado o Devuelto, se podrá visualizar ese cambio en el historial del registro.

## Paso 3. Equipos del préstamo

1. En fields.One2many("prestamo.linea", "prestamo_id"), ¿qué indica cada parámetro?

El primer parámetro (prestamo.linea) indica el modelo relacionado, que es el modelo donde se registran los equipos que pertenecen al préstamo. El segundo parámetro (prestamo_id) indica el campo Many2one de ese modelo que establece la relación con el préstamo. De esta manera, un préstamo puede tener varias líneas de equipos.

2. ¿Qué pasa con las líneas cuando se borra un préstamo? ¿Qué parte del código lo define?

Cuando se elimina un préstamo, también se eliminan automáticamente sus líneas relacionadas. Esto se define mediante ondelete="cascade" en el campo prestamo_id del modelo prestamo.linea, lo que evita que queden líneas sin un préstamo asociado.

## Paso 4: Flujo de estados

1. Los botones ya se ocultan según el estado. ¿Por qué los métodos vuelven a validar el estado?
   Porque ocultar los botones en la interfaz no es suficiente para garantizar que la operación sea válida. Las validaciones dentro de los métodos evitan que se realice un préstamo o devolución incorrecta, aunque se intente ejecutar la acción por otro medio. Así se protege la lógica del sistema.

2. ¿Qué hace prestamo.linea_ids.equipo_id.write(...)? ¿Por qué no hace falta un for para cada equipo?

Esta instrucción permite acceder a los equipos relacionados con todas las líneas del préstamo y cambiar su estado al mismo tiempo utilizando write(). No hace falta un for individual porque Odoo permite ejecutar operaciones sobre varios registros (recordsets) simultáneamente.
