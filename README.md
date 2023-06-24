# Esp32 LittleFS

### Libreria
<img src="https://github.com/IDiegoUlises/Esp32-LittleFS/blob/main/Images/Libreria-LittleFS.jpg" />

Es para guardar datos en la memoria pero a diferencia de spiff, spiff es una libreria obsoleta y no recibira mas actualizaciones.

En segundo lugar, hay un límite de 32 caracteres en total para los nombres de archivo. Un '\0'carácter está reservado para la terminación de la cadena C, por lo que nos deja con 31 caracteres utilizables.

Advertencia : ese límite se alcanza fácilmente y, si se ignora, los problemas pueden pasar desapercibidos porque no aparecerá ningún mensaje de error en la compilación ni en el tiempo de ejecución.

ejemplo, el nombre del archivo /website/images/bird_thumbnail.jpg tiene 34 caracteres y causará algunos problemas si se usa

### Codigo para formatear la memoria
```c++
#include <LITTLEFS.h>

//false para no restablecer(Por defecto esta definido como false)
//true para restablecer
bool restablecer = true;

void setup()
{
  //Inicia el puerto serial
  Serial.begin(115200);

  //Retardo de cinco segundos
  delay(5000);

  if (!LITTLEFS.begin(restablecer))
  {
    Serial.println("Error Iniciando LittleFs");
    return;
  }

  else
  {
    Serial.println("Formateado Correctamente");
  }

  //Cerrar gestor de archivos
  LITTLEFS.end();

}

void loop()
{

}
```

### Codigo para escribir un archivo 
```c++
#include <LITTLEFS.h>

//Esta variable true para formatear
#define FORMAT_LITTLEFS_IF_FAILED false

String ruta = "/ruta.txt";
String texto = "Este es el texto que se escribio";

void setup()
{
  Serial.begin(115200);

  if (!LITTLEFS.begin())
  {
    Serial.println("Error montando LittleFs");
    return;
  }

  else {
    Serial.println("Demo de LittleFS");
  }

  delay(5000);

  File archivo = LITTLEFS.open(ruta, "w");

  if (!archivo)
  {
    Serial.println("El archivo no se puede abrir");
    return;
  }

  if (archivo.print(texto))
  {
    Serial.println("Se escribio en el archivo");
  }

  else {
    Serial.println("Error al escribir");
  }

  archivo.close();
  LITTLEFS.end();

}

void loop()
{

}
```

### Codigo para leer un archivo
```c++
#include <LITTLEFS.h>

//Esta variable true para formatear
#define FORMAT_LITTLEFS_IF_FAILED false

String ruta = "/ruta.txt";
//String texto = "Este es el texto que se escribio";

void setup()
{
  Serial.begin(115200);

  if (!LITTLEFS.begin())
  {
    Serial.println("Error montando LittleFs");
    return;
  }
  else {
    Serial.println("Demo de LittleFS");
  }

  delay(5000);

  File archivo = LITTLEFS.open(ruta, "r");

  if (!archivo)
  {
    Serial.println("El archivo no se puede abrir");
    return;
  }

  while (archivo.available())
  {
    Serial.write(archivo.read()); //Escribe lo que ahi en el archivo
  }

  archivo.close();
  LITTLEFS.end();

}

void loop()
{

}
```



r Abrir archivo de texto para lectura. La secuencia se coloca al principio del archivo.

r+ Abierto para lectura y escritura. La secuencia se coloca al principio del archivo.

w Truncar el archivo a cero o crear un archivo de texto para escribir. La secuencia se coloca al principio del archivo.

w+ Abierto para lectura y escritura. El archivo se crea si no existe, de lo contrario se trunca. La secuencia se coloca al principio del archivo.

a Abrir para anexar (escribir al final del archivo). El archivo se crea si no existe. La secuencia se coloca al final del archivo.

a+ Abrir para leer y agregar (escribir al final del archivo). El archivo se crea si no existe. La posición inicial del archivo para lectura es al principio del archivo, pero la salida siempre se agrega al final del archivo.
