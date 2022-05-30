## HTB-ACADEMY (Windows Fundamentals/Estructura del Sistema Operativo) 
##### Ivan Medina
---
La raiz del sistema de archivos, se define con una letra que corresponde al controlador del disco, usualmente es C,y a las demas unidades que se vayan conectando se les va agregando otra letra. En la raiz es donde se encuentra instalado el sistema operativo y donde se encuentra el programa de booteo.

La estructura es la siguiente:

**Perflogs**: Guarda logs del rendimiento del sistema.

**Program Files**: Aqui se instalan los progrmas 64 bits si la arquitectura es de 64 bits, o programas de 32 bits si el sistema lo es.

**Program Files(x86)**: Los programas que no corresponden a la arquitectura original del sistema (16 o 32 bits).

**Program Data**: Almacena informacion necesaria para los programas, es accesible por el programa sin importar el usuario.

**Users**: Las carpetas correspondientes a los perfiles en el sistema. Ademas de un folder Public y uno Default.

**Default**: La estructura de este fichero, define la estrucura que tendra el folder de los nuevos usuarios creados.

**Public**: Permite compartir archivos entre los usuarios del sistema o de la red, siempre y cuando tengan acceso.

**AppData**: Cada carpeta de usuario contiene un folder AppData oculto, el cual almacena información especial del usuario para las aplicaciones, como configuraciones personalizadas por ejemplo. Adicionalmente dentro existen tres carpetas...

**AppData/Roaming**: Información independiente de la maquina y no afecta al sistema.

**AppData/Local**: Información que es especificamente solo para esa computadora.

**AppData/LocalLow**: Similar al caso anterior. Pero la integridad de la informacion es menor.

**Windows**: La mayoria de los archvios necesarios para el funcionamiento del sistema estan aqui.

**Windows/(System||System32||SysWOW64)**: Contiene los DLL´s requeridos para las funcionalidades de Windows y el API de Windows. El sistema operativo revisa en esta carpeta cuando se solicitan DLL´s que no tienen una ruta absoluta especificada.

**WinSxS**: Se conoce como Componente de Almacenamiento de Windows y contiene una copia de todas las actualizaciones, componentes y packs del sistema.

Podemos ver un listado organizado de nuestros ficheros ejecutando el siguiente comando:
```tree c:\ /f | more```
