# Esp32-Littlefs

### Libreria
<img src="https://github.com/IDiegoUlises/Esp32-LittleFS/blob/main/Images/Libreria-LittleFS.jpg" />

Es para guardar datos en la memoria pero a diferencia de spiff, spiff es una libreria obsoleta y no recibira mas actualizaciones.

En segundo lugar, hay un límite de 32 caracteres en total para los nombres de archivo. Un '\0'carácter está reservado para la terminación de la cadena C, por lo que nos deja con 31 caracteres utilizables.

Advertencia : ese límite se alcanza fácilmente y, si se ignora, los problemas pueden pasar desapercibidos porque no aparecerá ningún mensaje de error en la compilación ni en el tiempo de ejecución.

ejemplo, el nombre del archivo /website/images/bird_thumbnail.jpg tiene 34 caracteres y causará algunos problemas si se usa

### Codigo para formatear la memoria
```c++
#include <LITTLEFS.h>

//true para restablecer, false para no restablecer(o se puede dejar vacio)
bool restablecer = true;

void setup()
{
  Serial.begin(115200);
  delay(5000);

  if (!LITTLEFS.begin(restablecer))
  {
    Serial.println("Error montando LittleFs");
    return;
  }
  else
  {
    Serial.println("Formateado correctamente");
  }
  
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
