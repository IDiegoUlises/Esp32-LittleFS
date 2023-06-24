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

### Codigo para crear el archivo
```c++
#include <LITTLEFS.h>

//Ruta del archivo
String ruta = "/ruta.txt";

//Texto del archivo txt
String texto = "Este es el texto";

void setup()
{
  //Inicia el puerto serial
  Serial.begin(115200);

  //Retardo de cinco segundos
  delay(5000);

  if (!LITTLEFS.begin())
  {
    Serial.println("Error Iniciando LittleFs");
    return;
  }

  else
  {
    Serial.println("Inicio de LittleFS Correctamente");
  }

  //Crea el archivo
  File archivo = LITTLEFS.open(ruta, "w");

  if (!archivo)
  {
    Serial.println("El archivo no se puede abrir");
    return;
  }

  if (archivo.print(texto))
  {
    Serial.println("Se escribio correctamente en el archivo");
  }

  else
  {
    Serial.println("Error al escribir");
  }

  //Cerrar el archivo
  archivo.close();

  //Cerrar el gestor de archivos
  LITTLEFS.end();

}

void loop()
{

}
```

### Codigo para leer un archivo
```c++
#include <LITTLEFS.h>

//Ruta del archivo
String ruta = "/ruta.txt";

void setup()
{
  //Inicia el puerto serial
  Serial.begin(115200);

  //Retardo de cinco segundos
  delay(5000);

  if (!LITTLEFS.begin())
  {
    Serial.println("Error montando LittleFs");
    return;
  }
  else
  {
    Serial.println("Inicio de LittleFS Correctamente");
  }

  //Abre el archivo en modo lectura
  File archivo = LITTLEFS.open(ruta, "r");

  //En caso que no se pueda abrir el archivo
  if (!archivo)
  {
    Serial.println("El archivo no se puede abrir");
    return;
  }

  //Lee el archivo
  while (archivo.available())
  {
    //Guarda en una variable el texto del archivo
    char leer = archivo.read();

    //Imprime el texto en el puerto serial
    Serial.print(leer);
  }

  //Cerrar el archivo
  archivo.close();

  //Cerar el gestor de archivos
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
