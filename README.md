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

## Paso 5. Reglas del préstamo

1. El campo equipo_id ya filtra los equipos disponibles. ¿Por qué hace falta el TODO 5?

Aunque el campo equipo_id filtra los equipos disponibles en la interfaz, es necesario realizar una validación adicional porque el estado de un equipo puede cambiar después de seleccionarlo. Por eso, antes de realizar el préstamo, el sistema vuelve a verificar que el equipo siga disponible, evitando prestar equipos que estén ocupados o en mantenimiento.

2. ¿En qué momento se ejecuta un método con @api.constrains? ¿Y la validación de action_prestar?

Los métodos con @api.constrains se ejecutan cuando se crea o modifica un registro y se cumplen los cambios en los campos que tienen especificados en el decorador, permitiendo validar los datos antes de que se guarden correctamente.

Por otro lado, las validaciones de action_prestar se ejecutan cuando el usuario presiona el botón Prestar, ya que son comprobaciones necesarias justo antes de cambiar el estado del préstamo y de los equipos.

## Paso 6. Contacto, kanban y búsqueda

1. ¿Qué pasa si arrastra una tarjeta del Kanban a otra columna? ¿Por qué?

Al arrastrar una tarjeta de una columna a otra en el Kanban, cambia el estado del equipo, porque las columnas están agrupadas por el campo estado. Esto permite modificar visualmente el estado del equipo, por ejemplo, pasar de Disponible a Prestado.

Sin embargo, hay que tener cuidado porque al cambiarlo manualmente se puede generar una inconsistencia si no se realiza desde el proceso de préstamo correspondiente.

2. El campo prestamo_ids se agregó a res.partner. ¿Se crea una columna nueva en la tabla de contactos? Explique.

No, no se crea una nueva columna en la tabla de contactos. El campo prestamo_ids es un campo One2many, por lo que permite mostrar los préstamos relacionados con un contacto.

La relación se almacena mediante el campo partner_id del modelo prestamo.prestamo, que indica a qué contacto pertenece cada préstamo. Por eso, prestamo_ids permite consultar los préstamos de un contacto sin almacenar una lista de préstamos directamente en la tabla de contactos.
