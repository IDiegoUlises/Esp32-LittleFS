# Esp32 LittleFS

### Libreria
<img src="https://github.com/IDiegoUlises/Esp32-LittleFS/blob/main/Images/Libreria-LittleFS.jpg" />

* Anteriormente se utilizaba Spiff para guardar datos en la memoria pero actualmente esta libreria esta obsoleta y ahora debe utilizarle LittleFS 
* LittleFS permite guardar una amplia cantidad de datos

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

### Codigo para escribir en un archivo existente
```c++
#include <LITTLEFS.h>

//Ruta del archivo
String ruta = "/ruta.txt";

//Texto del archivo txt
String texto = "Para agregar texto a un archivo existente";

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

  //Abre el archivo
  File archivo = LITTLEFS.open(ruta, "a");

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

| Comando | Descripcion | 
|  :---: | :---  |          
| r | Abrir archivo de texto para lectura, la secuencia se coloca al principio del archivo | 
| r+ | Abierto para lectura y escritura, la secuencia se coloca al principio del archivo |
| w | Truncar el archivo a cero o crear un archivo de texto para escribir, la secuencia se coloca al principio del archivo |
| w+ | Abierto para lectura y escritura el archivo se crea si no existe de lo contrario se trunca, la secuencia se coloca al principio del archivo | 
| a | Abrir para anexar, el archivo es creado si no existe la secuencia se coloca al final del archivo |
| a+ | Abrir para leer y agregar, el archivo se crea si no existe la posicion inicial del archivo para lectura es al principio del archivo, pero la escritura se agrega al final del archivo |

 

