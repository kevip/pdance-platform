# Glosario

Vocabulario del dominio de Allegra Pole Dance. Úsalo en issues, nombres de código, tests y docs. Los términos marcados como **(inferido)** salen del esquema de la base legacy (`pdance-admin`) y conviene confirmarlos con `/grill-with-docs`.

## Personas

### Alumna
Persona inscrita en la academia que toma clases. Tiene nombre, apellido, teléfono, email, contraseña y un estado.
- **Código:** `Student` · **Tabla:** `students`
- **Evitar:** "usuario" (reservado para el admin) y "alumno". Las rutas legacy dicen `alumnos` y `usuario`.

### Admin
Único usuario del backoffice, con acceso total. Los roles múltiples (por ejemplo, `viewer`) están fuera de alcance.
- **Tabla:** `users`

## Oferta de clases

### Curso
Disciplina o tipo de clase que ofrece la academia.
- **Código:** `Course` · **Tabla:** `courses` (`name`, `flag`)

### Grupo de clases (inferido)
Programación recurrente de un curso: rango de fechas (`from`/`to`), horario (`start_time`/`end_time`), días de la semana (`days`), mes (`month`), días no laborables (`non_working_days`) y una etiqueta (`label`). A partir de él se generan las clases. "Programar clases" en el backoffice crea un grupo.
- **Código:** `ClassGroup` · **Tabla:** `classes_groups`

### Clase
Sesión concreta de un curso, con fecha y hora de inicio y fin. Pertenece a un grupo de clases.
- **Código:** `Class` (legacy: `Classes`) · **Tabla:** `classes`

### Cupo
Lugares disponibles en una clase: `vacancies − enrolled`. Es el punto donde hay que evitar sobreventa cuando dos alumnas reservan a la vez.
- **Columnas:** `classes.vacancies` (capacidad) y `classes.enrolled` (inscritas)

### Calendario
Vista de las clases por día. No es una entidad propia.

## Matrícula

### Matrícula
Paquete que habilita a una alumna a tomar clases durante un periodo. Tiene un estado, una vigencia (`enabled_from`/`enabled_to`), un número de clases admitidas (`admited_classes`, 8 por defecto), los cursos permitidos y el monto pagado.
- **Código:** `Enrollment` · **Tabla:** `enrollments`

### Cursos permitidos (inferido)
Cursos que una matrícula habilita a reservar.
- **Tabla:** `allowed_courses`

### Reserva (inferido)
Asignación de una clase concreta a una matrícula. Puede marcarse como editada; la matrícula cuenta cuántas veces se editó (`edited_times`).
- **Código:** `EnrollmentDetail` · **Tabla:** `enrollment_details`

### Pago
Registro manual del monto que pagó la alumna por una matrícula. No hay pasarela de pagos.
- **Columna:** `enrollments.fee` (se expone como `amount` en las consultas legacy)
- **Evitar:** "cobro" o "transacción", que sugieren pagos en línea.

### Habilitar matrícula
Acción del admin que activa una matrícula (ruta legacy `enable-enrollment`).

## Estados

| Entidad | Tabla de estados |
| --- | --- |
| Alumna | `status` |
| Matrícula | `enrollment_status` |

Los valores concretos de cada estado no están en el código legacy; hay que leerlos de la base de datos.
