# Manual de usuario para p2-dogProgram

## 1. Consideraciones iniciales
El software p2-dogProgram tiene como propósito el manejo de una base de datos de mascotas, con una serie de funciones disponibles para su manipulación. Consta de un programa servidor encargado del manejo de los datos y el registro de las operaciones realizadas, y un programa cliente desde el cual un usuario final puede solicitar operaciones al servidor.
## 2. Especificaciones
El software **únicamente** funciona en sistemas operativos de la familia UNIX que posean la capacidad de manejar hilos, escritura y lectura desde archivos, y paso de mensajes a través de red. Esto también incluye una capacidad de emplear librerías estándar de UNIX (e.g. POSIX). El programa únicamente ha sido probado en versiones actualizadas de Ubuntu.
## 3. Datos técnicos
### 3.1. La estructura dogType
Todas las operaciones de almacenamiento y lectura de información del programa dependen de una estructura de datos llamada dogType. A continuación se detallan sus campos:
1. `id`: Número entero
2. `nombre`: Arreglo de caracteres de 32 posiciones
3. `tipo`: Arreglo de caracteres de 32 posiciones
4. `edad`: Número entero
5. `raza`: Arreglo de caracteres de 16 posiciones
6. `estatura`: Número entero
7. `peso`: Número decimal de 8 bytes
8. `sexo`: Caracter

Algunas consideraciones a tener en cuenta:
- Los tamaños de los arreglos `nombre`, `tipo` y `raza` especifican cual es la extensión máxima de información que dichos campos son capaces de almacenar. Esto es importante al realizar operaciones de entrada de información, pues cualquier dato que supere su la cantidad de espacio que el programa tiene definido para su campo no tiene ninguna garantía de ser almacenado correctamente.
- El campo `sexo` solo tiene **dos** valores posibles: m y f. No es posible insertar algún carácter distinto de estos dos, pues el programa no lo permite.
- El campo `id` no puede ser modificado directamente de ninguna manera. El programa asigna un ID único a cada elemento que es insertado en la base de datos para mantener orden y manejar algunas de sus operaciones.
### 3.2. Almacenamiento
El programa almacena toda la información de las mascotas que se le proporcionan en un archivo llamado *dataDogs.dat*, ubicado en el mismo directorio donde se encuentra el programa servidor. Este archivo es de tipo binario, y como tal no puede ser editado directamente.
### 3.3. Operaciones del programa
A continuación se detallan las operaciones que el programa le permite al usuario realizar:
1. **Inserción:** Al escoger ésta operación, el programa procede a solicitarle al usuario, uno por uno, todos los campos de la estructura dogType *excepto* por el campo `id` (referirse a la sección **3.1** para una explicación del funcionamiento de dicho campo). Una vez todos los campos han sido insertados, se inserta la información al archivo dataDogs.dat.
2. **Visualización:** Al escoger ésta operación, el programa le solicita al usuario un número entero correspondiente al campo `id` de la mascota cuya información desee visualizar. Al insertarse dicho número, el programa busca la mascota cuyo campo `id` tenga el mismo valor introducido por el usuario, y al encontrarlo procede a escribir la información de todos sus campos en un archivo de texto antes de abrir un editor de texto en consola y mostrárselos al usuario. *Nota:* Solo es posible insertar un número que corresponda a algún `id` existente en el programa. Insertar un número superior a la cantidad de registros en el archivo *dataDogs.dat* no es posible. Al llamar ésta función, el programa le muestra al usuario la cantidad de registros contenidos en el archivo.
3. **Búsqueda:** Al escoger ésta operación, el programa le solicita al usuario una cadena de caracteres de máximo 32 caracteres de longitud, correspondiente al campo `nombre` de la estructura dogType. Una vez se proporciona dicha información, el programa muestra en consola la información de todas las mascotas cuyo campo `nombre` es igual al dato insertado por el usuario. Si no encuentra ninguna mascota con el mismo `nombre`, el programa dice que no se encontró ninguna mascota con dicho nombre.
4. **Eliminación:** Al escoger ésta operación, el programa le solicita al usuario un número entero correspondiente al campo `id` de alguna de las mascotas registradas en el archivo *dataDogs.dat*. Al serle proporcionado un `id` válido, el programa busca en el archivo la mascota cuyo `id` corresponde al número insertado, y elimina su información de la base de datos. **Ésta operación es irreversible.** *Nota:* Al igual que con la operación de Visualización, el programa únicamente le permite al usuario insertar un `id` válido, y le muestra cuantos registros se encuentran en el programa.
### 3.4. Funcionamiento del programa
Para el funcionamiento del programa es necesario que en todo momento se esté ejecutando la aplicación de servidor. Si no se cumple ésto, la aplicación de cliente no puede conectarse a ningún servidor y se termina inmediatamente.
Al cumplirse ésto, el cliente muestra en consola una serie de operaciones posibles, y posteriormente solicita un número entero correspondiente a la operación que el usuario decida realizar. Las operaciones posibles están detalladas en la sección **3.3**, y adicionalmente el cliente le permite al usuario salir del programa. Todas las opciones de operación (menos la salida) esperan hasta que el usuario presione la tecla Enter luego de su ejecución para regresar al menú principal.
