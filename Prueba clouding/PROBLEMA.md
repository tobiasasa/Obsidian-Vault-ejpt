No puedo importar los campos de la tabla de wpimp_ hacia la de wp_, no obtengo 2 usuarios administradores, ni los comentarios, post o funcionalidades de mi wp, todo se queda en not found.

Entiendo, parece que estás tratando de fusionar los datos de dos bases de datos de WordPress, una de las cuales tiene un prefijo de tabla diferente (`wpimp_`). Si estás importando la base de datos a través de phpMyAdmin y estás viendo que se crean nuevas tablas con el prefijo `wpimp_`, es probable que estés importando la base de datos como una nueva base de datos en lugar de fusionarla con la base de datos existente.

Para fusionar los datos de ambas bases de datos sin perder las entradas existentes y manteniendo los datos con los mismos ID, puedes seguir estos pasos:

1. **Exporta la tabla `wpimp_users` desde la base de datos actual:**
    
    - Ve a phpMyAdmin.
    - Selecciona la base de datos que contiene la tabla `wpimp_users`.
    - Haz clic en la pestaña "Exportar".
    - Selecciona la tabla `wpimp_users` para exportar y elige el formato de exportación deseado (por ejemplo, SQL).
    - Exporta la tabla.
2. **Importa la tabla `wpimp_users` a la base de datos de WordPress existente (`wp_users`):**
    
    - Ve a phpMyAdmin.
    - Selecciona la base de datos de WordPress en la que deseas importar los datos.
    - Haz clic en la pestaña "Importar".
    - Selecciona el archivo SQL que exportaste en el paso anterior.
    - Asegúrate de seleccionar la opción "Eliminar tablas antes de importar" o "Sobrescribir tablas existentes" si deseas reemplazar los datos existentes.
    - Importa el archivo.
3. **Actualiza los IDs si es necesario:**
    
    - Si ambas tablas tienen columnas `id` y algunos IDs coinciden, tendrás que manejar este conflicto manualmente.
    - Puedes actualizar los IDs de una de las tablas para evitar conflictos. Por ejemplo, puedes incrementar los IDs de la tabla `wpimp_users` antes de importarla para evitar que coincidan con los de la tabla `wp_users`.
    - Esto se puede hacer con una consulta SQL utilizando la función `AUTO_INCREMENT` o manualmente mediante la edición del archivo SQL exportado.
4. **Verifica los resultados:**
    
    - Después de la importación, verifica que los datos se hayan fusionado correctamente y que no se hayan perdido entradas.

Al seguir estos pasos, deberías poder fusionar los datos de ambas tablas sin perder las entradas existentes y manteniendo los datos con los mismos IDs. Recuerda hacer una copia de seguridad de tus datos antes de realizar cambios importantes en la base de datos.