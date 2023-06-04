# Esp32-Littlefs
Es para guardar datos en la memoria pero a diferencia de spiff, spiff es una libreria obsoleta y no recibira mas actualizaciones.

En segundo lugar, hay un límite de 32 caracteres en total para los nombres de archivo. Un '\0'carácter está reservado para la terminación de la cadena C, por lo que nos deja con 31 caracteres utilizables.

Advertencia : ese límite se alcanza fácilmente y, si se ignora, los problemas pueden pasar desapercibidos porque no aparecerá ningún mensaje de error en la compilación ni en el tiempo de ejecución.

ejemplo, el nombre del archivo /website/images/bird_thumbnail.jpg tiene 34 caracteres y causará algunos problemas si se usa
