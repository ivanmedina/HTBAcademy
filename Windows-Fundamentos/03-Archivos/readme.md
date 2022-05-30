## HTB-ACADEMY (Windows Fundamentals/File System) 
##### Ivan Medina
# 
---
Existen 5 tipos de sistemas de archivos que Windows Soporta:
* FAT12
* FAT.
* FAT32
* NTFS
* exFAT

FAT12 y FAT16 no son tan grandes como las unidades modernas. 

**FAT32** (Tabla de Asiganción de archivos) es ampliamente usando en muchos tipos de almacenamiento como memoria USB y tarjetas SD pero pueden ser usadas para dar formato a discos duros. 32 en el nombre se refiere a el hecho de que FAT32 usa 32 bits de datos para identificar los cluster de datos en la unidad del dispositivo.

Las **ventajas** de este sistema es que tiene una muy aplica compatibilidad de dispositivos tal como computadoras, camaras digitales, consolas de videojuegos, smartphones, tabletas y mas. Y otra seria la alta compatibilidad entre la mayororia de sistemas operativos.

Pero las **desventajas** son que solo pueden contener archivos menores a 4gb, las particiones se limitan a 8TB, no hay proteccion de datos o caracteristicas para compresion de archivos, por lo que se deben usar herramientas de terceros para encriptar. 

**NTFS** (Nueva Tecnologia de Sistema de Sistema de archivos) es el sistema de archivos por defecto en los sistemas Windows desde la versioon NT 3.1. Esta version ademas de corregir los problemas en la version anterior, tiene bastantes mejoras en la manera que estructura los archivos y permite los metadatos. 
**Ventajas**:
+ Es confiable y se puede restaurar la consistencia del sistema de archivos en eventos como fallas en el sistema o perdida de energia.
+ Preeve seguridad permitiendo colocar permisos granulares tanto a archivos como carpetas.
+ Soporta particiones bastante grandes.
+ Se registran las operaciones en el sistema de archivos.

**Desventajas**:
+ No todos los dispositivos soportan NTFS de manera nativa.

### Permisos
Los sistemas de archivos NTFS tienen varios permisos basicos y avanzados. Algunos de ellos son:

**Full Control**: Permite lectura, escritura, actualización y eliminación de archivos y carpetas.
**Modify**: Permite lectura, escritura y elimiar archivos y carpetas
**Listar contenido de carpetas**: Permite la visualización y listado de carpetas y subcarpetas y ejecucion de archivos.
**Leer y ejecutar**: Permite lo mismo que la anterior.
**Escritura**: Permite agregar archivos a carpetas y escribir en ellos.
**Lectura**: Permite visualizar y listar contenido de carpetas y visualizar el contenido de los arhcivos en ella.
**Capeta Transversal**: Permite o deniega la habilidad de moverse a traves de los folders para alcanzar otros arhcivos o folders.

#### Lista de control de acceso de integridad (ICALCS)
Los permisos de los archivos y folders pueden ser adeministrados desde la interfaz grafica. Pero desde la linea de comandos tambien es posbible utilizando la herramienta ICALCS.
Podemos listar los permisos de alguna carpeta desde la terminal con ```icalcs <nombre_carpeta>```.
##### Niveles de acceso
**(CI)**: Contenedor heredado.
**(OI)**: Objeto heredado.
**(OI)**: Heredado solo.
**(NP)**: No propagar herencia.
**(I)**: Permisios heredados desde el contenedor padre.
##### Permisos basicos
**F**: Acceso Completo.
**D**: Acceso de eliminación.
**N**: Sin acceso.
**M**: Acceso para modificar.
**RX**: Ejecución y lectura.
**R**: Permiso de solo lecutra.
**W**: Permiso de solo escritura.
#### Agregar o remover permisos
Podemos dar permiso de control total a un usuario como Ivan con el siguiente comando:
```icacls c:\users /grant Ivan:f ```.
Y para removerlo:
```icacls c:\users /remove joe```.

icalcs es una herramienta muy poderosa que permite administrar los permisos de carpetas y archivos de usuarios y grupos.
[Lista completa de comandos icalcs](https://ss64.com/nt/icacls.html).

