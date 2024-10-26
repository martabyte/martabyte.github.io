---
layout: post
title:  "Hacking iOS - Guía para Principiantes sobre Hacking de Apps iOS [Edición 2022]"
date:   2022-03-14 22:00:00 +0100
categories: ios hacking
published: true
---

Mi primer post será sobre hacking en iOS, un tema sobre el que actualmente estoy trabajando, por lo que esta guía será una recopilación de toda la información que he encontrado en mi investigación. Cabe destacar que no estaré usando ninguna herramienta en MacOS, ya que el ordenador que utilizo es un *host* **Linux**, específicamente con una distro basada en Debian, en concreto, Kali Linux. También usaré '**checkra1n**' para realizar el jailbreak del dispositivo, pero existen otras alternativas como 'unc0ver' o 'Taurine', así que dejo a vuestra elección escoger aquel método que os guste más o se adapte mejor a la versión de vuestro dispositivo. Para más contexto, mi dispositivo es un iPhone 6s actualizado a la **última versión de iOS 14.x** a fecha de Marzo de 2022 (14.8). Si no me equivoco, aún no hay un jailbreak que funcione completamente para iOS 15 a fecha actual.

Este post es puramente educacional y con la intención de formar en Hacking Ético únicamente. Si encontráis errores o pensáis que podría mejorar algo en este post, ¡dejádmelo saber por favor! :)

<br>

- - - -

<br>

## Jailbreak del Dispositivo iOS ##
Todos los dispositivos móviles tienen restricciones puestas por los fabricantes que evitan que los usuarios finales instalen ciertos componentes o aplicaciones, accedan a recursos de bajo nivel, modifiquen configuraciones avanzadas... El Jailbreaking es una forma de elevar privilegios para eliminar estas restricciones y poder ser capaces de realizar cualquier operación deseada en un dispositivo iOS. Su equivalente en Android se llama ‘Rooting’. 

**¡Aviso!** Apple avisa a sus usuarios que realizar un Jailbreak a sus dispositivos provoca que la garantía del producto ya no sea válida, por lo que, continuad bajo vuestra propia responsabilidad y no realicéis estos pasos en vuestro dispositivo iOS principal.

Para realizar el Jailbreak del dispositivo desde un host Linux, descubrí que se necesitaban algunas librerías para que el dispositivo fuera detectado correctamente. Dependiendo de vuestra instalación de Linux las necesitareis o no, pero estas son las que funcionaron para mí:

```
sudo apt install ifuse usbmuxd libimobiledevice libimobiledevice-utils libplist-utils ideviceinstaller python3-imobiledevice python3-plist python3-libplist (y/o libplist3)
```


### Checkra1n ###
Uno de los programas que permiten realizar el Jailbreak de dispositivos iOS es ‘checkra1n’. Este método de jailbreak necesita un ordenador host donde se conectará el dispositivo iOS. En este caso, el ordenador host será un host Linux, donde la distro recomendada es cualquiera basada en Debian. Los pasos para descargar ‘checkra1n’ están en su página web, donde hay diferentes opciones como usando el repo oficial o descargando el binario. Yo seguí el método del repo oficial:

```
echo 'deb https://assets.checkra.in/debian /' | sudo tee /etc/apt/sources.list.d/checkra1n.list
sudo apt-key adv --fetch-keys https://assets.checkra.in/debian/archive.key
sudo apt-get update
sudo apt-get install checkra1n
```

Una vez descargado, comprobad que lo estéis ejecutando con **privilegios de administrador**, ya que, si no se lanza con estos privilegios, no detectará el dispositivo una vez que esté en modo *‘recovery’* [No seáis como yo y os paseís un día entero revisando qué está mal para ver que simplemente era esta tontería].  

Para lanzar la versión gráfica:
```
sudo checkra1n --gui & 
```

Si la versión de vuestro dispositivo aparece como **unsupported/untested**, por ejemplo, está actualizado a la **última versión de iOS 14.x** como el mío, se puede hacer el jailbreak (menos en iOS 15 por ahora), ya que este método explota una vulnerabilidad en la BootROM (‘checkm8’), por lo que, en general, no se puede parchear con actualizaciones de software comunes y el jailbreak muy probablemente siga funcionando. Para que checkra1n pueda realizar el jailbreak de estas versiones *’unsupported/untested’* simplemente dirigíos a ‘Options’ y seleccionad la opción ‘Allow untested iOS/iPAD/tvOS versions’. Si vuestro dispositivo es un dispositivo A11 con iOS 14.0 - iOS 14.8, es necesario eliminar la contraseña de acceso *(‘passcode’)* y seleccionar la opción ‘Skip A11 BPR check’. También, cabe destacar que en Linux, checkra1n no funciona para los dispositivos con A7 por ahora. Para más información y más actualizada sobre la compatibilidad con versiones y dispositivos específicos, os recomiendo visitar la [página web oficial de Checkra1n](https://checkra.in).

Ahora, el jailbreak se puede realizar simplemente siguiendo los pasos que proporciona la herramienta. Tened en cuenta que checkra1n realiza un jailbreak *semi-tethered*, que significa que, si el dispositivo se reinicia, el dispositivo puede ser utilizado para las funciones y apps originales de iOS, pero el proceso de jailbreak se deberá realizar de nuevo desde el ordenador para acceder a sus funciones y aplicaciones elevadas. Eso sí, mantendrá las configuraciones y aplicaciones instaladas desde el usuario elevado y podrán ser accedidas de nuevo una vez el proceso de jailbreak se vuelva a realizar.

**Nota**: Vuestro ordenador host debe de ser Linux para poder usar este método correctamente. Si intentáis hacer el jailbreak desde una máquina virtual de Linux en un host Windows, podrían aparecer algunos problemas de conexión con el dispositivo en medio del proceso de jailbreak y son relativamente complicados de solucionar. Si no disponéis de un host Linux, recomiendo usar un Live-USB de Kali para realizar el jailbreak.

- - - -

<br>

## Setup Inicial Recomendado para Hacking (Usando Linux como OS del Ordenador Host) ##
Una vez el dispositivo esté correctamente *jailbreakeado* (no sé como traducirlo y que no suene feo), la app de 'checkra1n' debería aparecer en el dispositivo. Si vuestro dispositivo no está conectado al wifi aún, conectadlo y descargad 'Cydia' desde la app de 'checkra1n'.

### Cydia Impactor ###
Cydia es una AppStore para dispositivos con Jailbreak que contiene aplicaciones avanzadas para el dispositivo. Desde Cydia se pueden descargar una gran cantidad de apps interesantes. De las que hablaré son aquellas que he necesitado hasta ahora, y tened en cuenta que pueden existir alternativas en caso de que alguna no os funcione.


### OpenSSH ###
OpenSSH activará SSH para el dispositivo con Jailbreak. La contraseña por defecto es ‘alpine’ (¿... tendrá que ver con el plan de Fernando Alonso?) para todos los dispositivos iOS, así que el primer paso recomendado es cambiar la contraseña. Un paso adicional que recomiendo para más seguridad es desactivar la autenticación por contraseña, permitiendo únicamente el acceso por *SSH Key*.

Este post de Reddit de u/4z0k habla sobre como securizar las conexiones SSH de los dispositivos iOS (aunque no lo he probado a implementar aún): [Secure and customize your SSH installation and port! - Reddit](https://www.reddit.com/r/jailbreak/comments/f7logb/tutorial_secure_and_customize_your_ssh/)


### Filza ###
Filza permite navegar por todos los archivos del dispositivo. Esto nos permitirá, entre otras cosas, navegar a los directorios donde nos descarguemos IPAs no oficiales y se podrán instalar gracias a la siguiente app de la lista.


### AppSync Unified ###
AppSync Unified se necesita para poder instalar correctamente IPAs firmadas *ad-hoc*, falsamente o sin firmar directamente. Para descargarla desde Cydia, se deben seguir los siguientes pasos:

1. Id a la tab 'Sources' > 'Edit' > 'Add'
2. Añadid el repo de AppSync Unified más reciente: 'https://cydia.akemi.ai' (a fecha de Marzo de 2022)
3. Una vez el repo esté añadido, aparecerá como 'Karen Repo' y la app debería aparecer en la tab 'Search'


#### **Ejemplo Práctico: Descarga e Instalación de una App No Firmada en el Dispositivo** ####
1. Copiad el archivo al dispositivo a partir de SSH con 'scp' a la carpeta deseada, por ejemplo: `scp <file> root@<iDevice-IP>:/User/Downloads` 
2. Abrid Filza y navegad al archivo
3. Clickad en el archivo IPA y seleccionad 'Install'

Perfecto! Ahora ya tenéis vuestra IPA no oficial instalada correctamente en el dispositivo! :)


### Cycript ###
Para poder realizar análisis dinámico a través del *hooking* de apps y la inyección de código JavaScript. Se accede desde SSH. Más información en detalle se puede encontrar en la sección [Análisis Dinámico](#análisis-dinámico-101).


### Frida ###
Frida es otra app que permite realizar análisis dinámico a través del *hooking* de las apps y la inyección de código JavaSript. Puede ser accedida desde SSH o conectando el dispositivo al ordenador host directamente. Es mi opción preferida para el análisis dinámico de apps debido a sus capacidades extendidas cuando se usa su extensión: **'Objection'**. Más información en detalle se puede encontrar en la sección [Análisis Dinámico](#análisis-dinámico-101).

Para descargarla desde Cydia, se deben seguir los siguientes pasos:
1. Id a la tab 'Sources' > 'Edit' > 'Add'
2. Añadid el repo de Frida más reciente: 'https://build.frida.re' (a fecha de Marzo de 2022)
3. Una vez el repo esté añadido, la app debería aparecer en la tab 'Search'


### SSL Kill Switch 2 ###
Una protección frecuente ante el *hijacking* de las comunicaciones de las apps es el Certificate Pinning. Hay varias maneras de *bypassear* este control, de las cuales veremos algunas en la sección [Análisis Dinámico](#análisis-dinámico-101), pero la manera más fácil es descargando y activando esta aplicación.

Para descargarla, visitad la [página de GitHub de SSL Kill Switch 2](https://github.com/nabla-c0d3/ssl-kill-switch2), descargad el archivo .IPA e instaladlo desde Filza.


### Liberty Lite ###
Otra protección común frente al *hijacking* de la app es a partir de la implementación de controles de Jailbreak, para detectar si el dispositivo está *jailbreakeado*. Dependiendo del tipo y la calidad de los controles usados hay diferentes maneras de bypassearlos, aunque una de las maneras más fáciles es descargando y activando esta app desde Cydia. Otras maneras de bypassear este control se pueden encontrar en la sección [Análisis Dinámico](#análisis-dinámico-101).

Para descargarla desde Cydia, se deben seguir los siguientes pasos:
1. Id a la tab 'Sources' > 'Edit' > 'Add'
2. Añadid el repo de Liberty Lite más reciente: 'https://ryleyangus.com/repo/' (a fecha de Marzo de 2022)
3. Una vez el repo esté añadido, la app debería aparecer en la tab 'Search'


### Darwin CC Tools ###
Este paquete instalará una serie de herramientas de línea de comandos útiles como 'otool' or 'nm'.


### BigBoss Recommended Tools ###
Este paquete instalará una serie de herramientas de línea de comandos útiles como 'top', 'whois' or 'cURL'. Si existe un conflicto con la instalación debido a su dependencia con 'gdb', simplemente añadid 'http://cydia.radare.org' a las fuentes de repos e instalad 'gdb' desde ahí.


- - - -

<br>

## Análisis Estático 101 ##
El análisis estático de las apps se centra principalmente en la evaluación del archivo ejecutable y sus configuraciones antes de ejecutarlo para encontrar configuraciones sensibles o posibles funciones vulnerables. En iOS, las apps se compilan para ejecutarse de forma nativa en el dispositivo, por lo que el decompilado/reversing en lenguaje Objective-C en alto nivel no es posible. Pero sí que es posible hacer reversing a código asemblador, aunque requiere tener skills y experiencia en reversing y asemblador para poder llegar a entender el código.

<br>

### Reconocimiento Inicial del Binario de la App ###

#### **Binario de la App iOS** ####
El binario de una app iOS debería tener medidas de seguridad propias. El binario se encuentra dentro del directorio de la app, que normalmente se encuentra en '/private/var/containers/Bundle/Application/'.

* **PIE (Position Independent Executable)**: Cuando está activado, la aplicación se carga en una dirección aleatoria en memoria cada vez que se ejecuta, haciendo más complicado predecir su dirección en memoria inicial.
  ```
  otool -hv <app-binary> | grep PIE   # Debería incluir la flag PIE 
  ```

* **Stack Canaries**: Para validar la integridad de la pila, un valor 'canary' (canario) se introduce en la pila antes de llamar a la función, y es validado de nuevo una vez la función termina.
  ```
  otool -I -v <app-binary> | grep stack_chk   # Debería incluir los símbolos: stack_chk_guard y stack_chk_fail
  ```

* **ARC (Automatic Reference Counting)**: Para evitar vulnerabilidades comunes de corrupción de memoria
  ```
  otool -I -v <app-binary> | grep objc_release   # Debería incluir el símbolo _objc_release 
  ```

* **Binario Encriptado/Cifrado**: El binario debería estar encriptado/cifrado
  ```
  otool -arch all -Vl <app-binary> | grep -A5 LC_ENCRYPT   # El cryptid debería ser 1
  ```


### Desencriptar/Descifrar la App ###
Para poder realizar ingeniería inversa sobre la app, se necesita acceso a una copia no encriptada/cifrada del binario. Si el binario está encriptado/cifrado, existen diferentes maneras de desencriptarlo/descifrarlo, pero personalmente he usado '[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)'. Los pasos para usar este método, y otras 2 maneras de desencriptar/descifrar el binario, se pueden encontrar aquí: [Decrypt iOS Applications: 3 Methods](https://fadeevab.com/decrypt-ios-applications-3-methods/). Si Frida es el método escogido, comprobad que habéis instalado Frida en el dispositivo.

Una vez se obtiene el IPA desencriptado/descifrado, se puede cambiar su extensión a 'zip' y se puede descomprimir para analizar sus archivos internos.


#### **Información Sensible en el Sistema de Archivos de la App** ####
Una buena manera de identificar información sensible *hardcodeada* es buscar campos sensibles comunes en los archivos del sistema de archivos de la app, como IPs, direcciones de e-mail... Esto se puede hacer fácilmente con 'strings' y/o 'grep', con un poco de Regex.

```
strings <nombre-del-archivo>

grep -Eo '<expresion-regex>' -r *
```

Ejemplos de Expresiones Regex:
```
IPv4: [0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3} or ^\d{1,3}[.]\d{1,3}[.]\d{1,3}[.]\d{1,3}$

Direcciones de Correo Electrónico: [a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$ or ^[\w\.=-]+@[\w\.-]+\.[\w]{2,3}$

Números de Tarjeta Visa: \b([4]\d{3}[\s]\d{4}[\s]\d{4}[\s]\d{4}|[4]\d{3}[-]\d{4}[-]\d{4}[-]\d{4}|[4]\d{3}[.]\d{4}[.]\d{4}[.]\d{4}|[4]\d{3}\d{4}\d{4}\d{4})\b

IBAN: [a-zA-Z]{2}[0-9]{2}[a-zA-Z0-9]{4}[0-9]{7}([a-zA-Z0-9]?){0,16}

Base64: (?:[A-Za-z0-9+\/]{4})*(?:[A-Za-z0-9+\/]{2}==|[A-Za-z0-9+\/]{3}=)?$

URLs: (?i)\b((?:[a-z][\w-]+:(?:\/{1,3}|[a-z0-9%])|www\d{0,3}[.]|[a-z0-9.\-]+[.][a-z]{2,4}\/)(?:[^\s()]+|\(([^\s()]+|(\([^\s()]+\)))*\))+(?:\(([^\s()]+|(\([^\s()]+\)))*\)|[^\s`!()\[\]{};:'".,?«»“”‘’]))
```

Además, archivos con información sensible potencial pueden ser encontradas en el sistema de archivos, especialmente aquellos con extensiones como: plist, sql, xml, localstorage, db... Estos archivos se pueden encontrar fácilmente con el comando 'find':

```
find . -iname "*<extension>"
```

#### **Archivos Plist** ####
iOS almacena los datos de la configuración de la app en archivos 'plist'. Estos archivos pueden estar en binario o en formato XML, pero para que podamos leerlos fácilmente deberán estar en formato XML. Por lo tanto, si los archivos 'plist' encontrados están en formato binario, duplicad el archivo y ejecutad `plutil -convert xml1 <duplicated-file>.plist`. Ahora, se pueden analizar configuraciones inseguras en el archivo 'plist':

* **Identificar 'URL Handlers'**: Si la aplicación usa URLs personalizadas, el servidor puede que no parsee correctamente su contenido y, por lo tanto, podría derivar en ataques más sofisticados.
* **Identificar Claves API Keys o Credenciales Hardcodeadas**
* **Identificar Paths Internos**: Los developers puede que se hayan dejado algunos paths internos que podrían proporcionar información sobre los ordenadores usados para el desarrollo.
* **Identificar Ajustes de 'App Transport Security' Configurados de Forma Sensible**: Si la aplicación permite la carga de URLs arbitrarias (ajustes 'App Transport Security' establecidos en 'YES'), podría potencialmente causar comportamientos peligrosos en la app.


#### **Checksums del Sistema de Archivos** ####
Una app iOS debería validar los cambios en los contenidos del sistema de archivos. Intentad cambiar algunos valores de archivos que encontréis, por ejemplo, un archivo de base de datos 'sqlite':
```
sqlite3 <file>.sqlite
> (realizad el reconocimiento SQL necesario)
> update <tabla> set <campo>='<valor>' WHERE <condicion>
```

Guardad el archivo y reiniciad la app. Si el nuevo valor se muestra correctamente, la app no valida el checksum del sistema de archivos.


#### **Análisis Avanzado** ####
* **Examinar los Mensajes de Logging (ASL)**: Los contenidos del Apple System Log (ASL) pueden ser obtenidos por cualquier aplicación sin necesidad de permisos especiales, por lo tanto, no debe haber ninguna información sensible sobre la aplicación o sus usuarios en los logs.
  ```
  idevice_id --list   # Para encontrar el device ID (ID del dispositivo)
  idevicesyslog -u <id> (| grep <app>)   # Para obtener los logs del dispositivo
  ```

* **Almacenamiento de Contraseñas**: A continuación se encuentra un resumen de peor a mejor maneras de manejar las contraseñas de la aplicación:
  * Almacenar las contraseñas en texto en claro en un objeto 'NSUserDefaults' o cualquier otro archivo
  * Almacenar las contraseñas en texto en claro en el Keychain
  * Almacenar las contraseñas cifradas en el Keychain
  * Utilizar contraseñas para la autenticación inicial y después proporcionar claves específicas del dispositivo en vez de almacenar la contraseña
  * Utilizar tokens de autenticación 'short-lived' que controlan y limitan el acceso para evitar la necesidad de almacenar contraseñas

  <br/>

  El iOS Keychain proporciona una manera segura de almacenar datos sensibles, que se protege siguiendo esta estructura de clases:
  * **kSecAttrAccessibleWhenUnlocked**: El archivo se almacena encriptado/cifrado y no puede ser leido mientras el dispositivo se está encenciendo o está bloqueado.
  * **kSecAttrAccessibleAfterFirstUnlock**: El archivo permanece accesible hasta el siguiente reinicio del dispositivo. Se recomienda para items que se acceden por apps en segundo plano.
  * **kSecAttrAccessibleAlways**: Los datos son accesibles en cualquier momento, incluso si el dispositivo está bloqueado. No se recomienda bajo ninguna circunstancia.
  * **kSecAttrAccesibleWhenPasscodeSetThisDeviceOnly**: Los datos son accesibles únicamente cuando el dispositivo está desbloqueado.

<br>

### Ingeniería Inversa ###
La Ingeniería Inversa es un mundo en sí mismo. No tengo demasiada experiencia aún con ello así que simplemente comentaré las pocas cosas que sé, por lo que recomiendo que hagáis más investigación por vuestra cuenta. La Ingeniería Inversa de apps iOS es una tarea compleja que requiere bastante tiempo debido a la forma en que las apps se compilan, lo que por consecuencia las hace un poquito más seguras frente a ataques como 'app cloning' (clonación de apps).

Primero, se recomienda encontrar si la app está escrita en Swift o en Objective-C. Las apps que estén escritas en Swift tendrán los siguientes binarios:
```
nm <app> | grep '_OBJC_CLASS_$__' 
otool -L <app> | grep libswift   # No os confundáis pensando que si dice OBJC debe de ser un binario en Objective-C, no lo es
```

Si estas queries no proporcionan resultados, la app está escrita en Objective-C.

El objetivo de realizar ingeniería inversa sobre el código es entender el comportamiento de la app sin ejecutarla. La herramienta que principalmente utilizo es **Ghidra**, ya que es una herramienta de ingeniería inversa gratuita disponible para Linux y relativamente fácil de entender si eres nuevo en el mundo de la ingeniería inversa. Para importar el archivo selecciono la opción ‘Batch’ y el archivo IPA. A partir de aquí, intento encontrar credenciales *hardcodeadas*, intento entender como funcionan las funciones, o intento encontrar, por ejemplo, funciones como la de detección de jailbreak para intentar entender cómo se puede evitar la detección.

#### **Identificación de Funciones Sensibles/Inseguras** ####

* **Algoritmos de Cifrado Débiles**
  ```
  # En el dispositivo iOS
  otool -Iv <app> | grep -w "_CC_MD5"
  otool -Iv <app> | grep -w "_CC_SHA1"

  # En linux
  grep -iER "_CC_MD5"
  grep -iER "_CC_SHA1"
  ```

* **Funciones Aleatorias Inseguras**
  ```
  # En el dispositivo iOS
  otool -Iv <app> | grep -w "_random"
  otool -Iv <app> | grep -w "_srand"
  otool -Iv <app> | grep -w "_rand"

  # En linux
  grep -iER "_random"
  grep -iER "_srand"
  grep -iER "_rand"
  ```

* **Función 'Malloc' Insegura**
  ```
  # En el dispositivo iOS
  otool -Iv <app> | grep -w "_malloc"

  # En linux
  grep -iER "_malloc"
  ```

* **Funciones Inseguras y Vulnerables**
  ```
  # En el dispositivo iOS
  otool -Iv <app> | grep -w "_gets"
  otool -Iv <app> | grep -w "_memcpy"
  otool -Iv <app> | grep -w "_strncpy"
  otool -Iv <app> | grep -w "_strlen"
  otool -Iv <app> | grep -w "_vsnprintf"
  otool -Iv <app> | grep -w "_sscanf"
  otool -Iv <app> | grep -w "_strtok"
  otool -Iv <app> | grep -w "_alloca"
  otool -Iv <app> | grep -w "_sprintf"
  otool -Iv <app> | grep -w "_printf"
  otool -Iv <app> | grep -w "_vsprintf"

  # En linux
  grep -R "_gets"
  grep -iER "_memcpy"
  grep -iER "_strncpy"
  grep -iER "_strlen"
  grep -iER "_vsnprintf"
  grep -iER "_sscanf"
  grep -iER "_strtok"
  grep -iER "_alloca"
  grep -iER "_sprintf"
  grep -iER "_printf"
  grep -iER "_vsprintf"
  ```

- - - -

<br>

## Análisis Dinámico 101 ##
El análisis estático es importante, pero es un poco más complejo y, en mi opinión, requiere de un poco más de experiencia tanto en codificación como en ingeniería inversa que la que suele tener un penetration tester principiante. En el análisis estático, la aplicación puede que esté ofuscada o sea complicada de seguir, y puede que requiera de la interacción con el servidor o la carga de código dinámico, por lo que es aquí donde el análisis dinámico de la app entra en juego. El análisis dinámico consiste en monitorizar el comportamiento de la aplicación cuando se ejecuta a tiempo real. Esto incluye inspeccionar el almacenaje local y la interacción con la plataforma, e incluso modificar el comportamiento de la aplicación en tiempo de ejecución para, por ejemplo, forzar que la app use HTTP en vez de HTTPS para sus comunicaciones, o para evitar la detección de Jailbreak.

Como la app no puede ser decompilada, la manipulación del código fuente y su recompilación no es una opción, pero es posible manipular la app accediendo a las propiedades reflectivas en tiempo de ejecución de Obj-C. Esto permitirá que podamos acceder a variables internas y objetos en tiempo de ejecución, para poder cambiar dinámicamente el comportamiento de la app.

<br>

### Introducción a Objective-C ###
Objective-C solía ser el lenguaje principal para el desarrollo en iOS, aunque ahora Swift está siendo cada vez más popular debido a su simplicidad y mejoras respecto a Obj-C. Personalmente no he entrado demasiado en el mundo de Swift aún, así que en este post me centraré en Obj-C, pero quizá lo actualizaré una vez aprenda más sobre el análisis dinámico de aplicaciones en Swift.  

#### **Conceptos Principales de Obj-C** ####
* **Object**: Elemento principal que puede combinar grupos de datos (variables) y funcionalidades en la forma de métodos que pueden tomar acciones sobre los datos.
* **Class**: 'Feature' central que es un objeto instanciado que almacena variables y métodos.
* **Method**: Sección de código que se llama desde la clase objeto (es decir, la función).
* **Message**: Se usa para instruir a un método a que ejecute una acción.
* **Singleton**: Una clase singleton es una clase especial donde únicamente una instancia de la clase existe. Normalmente se usa para compartir datos entre múltiples clases en los procesos en ejecución.
* **Delegate**: Interfaz de objeto usada por dos objetos independientes para la comunicación e interacción.

Para más información, visitad esta guía rápida de Objective-C de Tutorials Point: [Objective-C Tutorial - Quick Guide](https://www.tutorialspoint.com/objective_c/objective_c_quick_guide.htm)

<br>

### Cycript ###
Cycript es una herramienta de manipulación en tiempo de ejecución que usa sintáxis JavaScript para acceder a los datos de objetos Obj-C en una app iOS. Usa una librería de Cydia, Cydia Substrate, que permite escribir un programa que se enganche/vincule a un programa en ejecución y pueda manipular su comportamiento en memoria, aunque, por lo tanto, dichos cambios se perderán una vez la app termine. Si la aplicación está escrita en Swift, Cycript en general puede modificar únicamente aquellos métodos que estén marcados con el header '@objc'.

Cycript se lanzará desde el dispositivo iOS a partir de una sesión SSH. Para lanzar Cycript, se debe identificar el nombre o PID de la app: `ps aux | grep <nombre-App>`. Después, se deberá lanzar Cycript vinculándolo a dicho proceso: `cycript -p <PID o Nombre de la App>`.

#### **Enumeración de Clases** ####
Cycript tiene un par de funciones que enumeran todas las clases. Estos métodos proporcionan mucho output, por lo que se recomienda dumpear el output en un archivo y parsearlo desde ahí.

```
class-dump

ObjectiveC.classes
[ObjectiveC.classes allKeys]

# Para escribirlo a un archivo
var classes = [[ObjectiveC.classes allKeys] componentsJoinedByString:@"\n"]
[classes writeToFile:"<output-path>/classesoutput.txt" atomically:NO encoding:4 error:NULL]
```

#### **Explorando la Aplicación** ####
* `UIApp.keyWindow`: Interfaz de la ventana abierta actual
* `UIApp.keyWindow.rootViewController`: Proporciona acceso a la ventana actual
* `printMethods(<object>)`: Para explorar el objeto

#### **Ejemplo de Manipulación/Modificación de Métodos** ####
```
<object>.prototype.<function> = function() { return <true/false/...>; } 
```

<br>

### Frida / Objection ###
Frida es un toolkit de ingeniería inversa e inyección de código dinámico que permite inyectar snippets de código o incluso librerías completas en aplicaciones.

Para instalar Frida (y Objection) en el ordenador host: `pip3 install frida-tools` y/o `pip3 install objection`. Frida deberá ser instalado también en el dispositivo iOS desde Cydia, como mencionado en la sección [Setup Inicial Recomendado](#setup-inicial-recomendado-para-hacking-usando-linux-como-os-del-ordenador-host). Por defecto, tanto Frida como Objection se conectarán al dispositivo iOS via USB, pero puede ser configurado para conectar sobre una conexión de red.

El Frida Server se ejecutará desde el dispositivo iOS e inyectará el código en la aplicación.

```
frida-ls-devices   # Para listar todas las conexiones disponibles a frida-servers
frida-ps -U   # Para listar todos los procesos en ejecución del dispositivo conectado por USB

frida -U <process-name>   # Para vincularnos a un proceso
frida -U -l <script>.js -n <process-name> --no-pause   # Para inyectar un script en el proceso y vincularnos

frida-ps -Uai   # Para listar todas las aplicaciones instaladas en el dispositivo conectado por USB

frida-discover -U <process-name>   # Para descubrir funciones internas
frida-trace -m "<function>" -U -f <process-name>   # Para rastrear ('trace') llamadas a funciones

frida-kill -U <PID>   # Para matar un proceso
```

Objection es un toolkit construido encima de Frida, que extiende las capacidades de Frida y facilita algunas acciones, como el bypass del SSL Certificate Pinning o la detección de Jailbreak.

```
objection -g <app-name> explore   # Para vincularnos a un proceso en ejecución - Objection reinicia la app y se inyecta en el proceso

[usb] # env   # Para localizar todos los directorios relacionados con la app
[usb] # pwd print / ls / !cat <file>   # Para imprimir el path del directorio actual, listar el directorio actual, y ejecutar el comando 'cat' en el sistema del dispositivo iOS
[usb] # ios plist cat <info-plist>.plist   # Para imprimir los contenidos del archivo Info.plist 
```

Nota: Si aparece un error donde Objection o Frida no pueden acceder al Frida Server, abrid una sesión de SSH en el dispositivo y ejecutad: ` frida-server & `, para abrir el Frida Server como un proceso en segundo plano.

#### **Explorar la Aplicación** ####
```
[usb] # ios hooking list classes   # Para listar todas las clases de la app
[usb] # ios hooking search classes <keyword>   # Para buscar todas las clases que contengan una palabra específica
[usb] # ios hooking list class_methods <class>   # Para listar todos los métodos de una clase
[usb] # ios hooking watch class/method <class/method> (--dump-args --dump-return --dump-backtrace)   # Para obtener notificaciones cada vez que un cierto método se ejecute por la aplicación

[usb] # memory dump all <output-file>   # Para dumpear toda la memoria de la app en un archivo

[usb] # import <frida-script>.js   # Importará un script de Frida
```

#### **Modificar la Aplicación** ####
```
[usb] # ios hooking set return_value <method> <true/false>   # Para establecer el valor de retorno de un método a un cierto valor Booleano

# Si aparece un error de 'invalid query', intentad escribir el método con el formato recomendado, por ejemplo: "*[<class> <method>]"
```


#### **Deshabilitar Certificate Pinning** ####
Another way to disable Certificate Pinning on the device is using Objection:

```
[usb] # ios sslpinning disable --quiet   # La opción 'quiet' es porque este hook puede generar mucho ruido
```


#### **Bypass de la Detección de Jailbreak** ####
```
[usb] # ios jailbreak disable   # Para hacer el bypass de la detección de jailbreak
[usb] # ios jailbreak simulate   # Para simular un entorno con jailbreak
```


#### **Información sobre el Binario** ####
La información del binario de la app también se puede ver desde Objection y revisar que estén los valores recomendados en la sección [Binario de la App iOS](#binario-de-la-app-ios):

```
[usb] # ios info binary   # Mostrará información sobre el binario de la app
```

#### **Keychain Dump** ####
Los contenidos del Keychain del dispositivo se pueden analizar desde Objection para buscar información sensible.

```
[usb] # ios keychain dump   # Dumpeará los contenidos de la keychain
```

#### **Cookies** ####
Las cookies de la app deberían revisarse para comprobar que tengan las flags y configuraciones correctas ('Secure', 'HTTPOnly', 'Path' hacia el path correcto...):

```
[usb] # ios cookies get   # Dumpeará las cookies de la aplicación
```

<br>

### Más Análisis del Sistema de Archivos y Funcionalidades de la App ###

#### **Capturas de Pantalla (Screenshots)** ####
Revisad si la app permite capturas de pantalla. Puede que sea una característica popular, pero podría conllevar la revelación de datos sensibles.


#### **Cortapapeles (Pasteboard)** ####
Si la app puede almacenar valores sensibles en el cortapapeles compartido entre las apps, es vulnerable a ataques de 'Side Channel Data Leakage'. Se puede comprobar de diferentes maneras: desde Objection, mirando manualmente los contenidos del cortapapeles del sistema... La app debería implementar su propio cortapapeles privado.

Desde Objection:
```
[usb] # ios pasteboard monitor   # Notificará cada vez que un nuevo valor se asigne al cortapapeles
```

Manualmente:
* Mirad en el contenido del cortapapeles compartido '/private/var/mobile/Library/Caches/com.apple.Pasteboard/' (o 'com.apple.UIKit.pboard') y mirad si cambia con valores sensibles copiados de la app 


#### **Snapshots** ####
Los 'Snapshots' de las aplicaiones son capturas de pantalla que la app realiza cuando se mueve a segundo plano, y se usan, por ejemplo, cuando se hace un vistazo rápido a las apps que el dispositivo tiene abiertas. Como en las capturas de pantalla, estas pueden contener información sensible que podría conllevar a la revelación de datos sensibles. Estos snapshots se pueden encontrar en: '< app-directory >/Library/SplashBoard/Snapshots/'.


#### **Caché de Teclado** ####
iOS almacena cualquier cosa que los usuarios tecleen para proporcionar funciones como la corrección automática *(auto-correct)* o el completado automático de texto *(text completion)*, por lo que podría almacenar información potencialmente sensible en texto en claro en el sistema. Los campos sensibles deben ser marcados como seguros para que no sean almacenados en caché(UITextAutocorrectionType). El texto en caché se puede encontrar en: '/var/mobile/Library/Keyboard/dynamic-text.dat'.

<br>

### Analizando las Comunicaciones de la App ###
Para analizar las comunicaciones entre la app y su servidor, se debe configurar un proxy para poder observarlas e incluso manipularlas. El proxy recomendado es Burp Suite, que es una plataforma integrada para realizar pruebas de seguridad de aplicaciones web creado por PortSwigger.

Para configurar Burp como proxy del dispositivo, seguid los siguientes pasos:
1. Abrid 'Burp Suite' en el ordenador > En la tab 'Proxy' > Id a la tab 'Options'
2. En 'Proxy Listeners' > Editad el listener actual y seleccionad 'All Interfaces' en 'Bind to Address' (aquí también se puede seleccionar el puerto deseado, yo lo dejaré por defecto en 8080)
4. En el dispositivo iOS, id a 'Ajustes' ('Settings') > 'Wi-Fi' > Seleccionad la conexión actual
5. En la sección 'Proxy HTTP' > 'Configurar Proxy' > 'Manual' > Escribid la IP de vuestro ordenador, el puerto del proxy Burp (en este ejemplo, 8080) y dejad la opción de Autenticación desactivada
6. Para que el dispositivo confíe en Burp para navegar por las páginas HTTPS, os deberéis descargar el certificado CA de Burp desde: 'http://burp' (o 'http://< IP-ordenador >:8080') y se deberá instalar: Id a 'Ajustes' ('Settings') > 'General' > 'Perfil' ('Profile') > Seleccionad 'PortSwigger CA' y seguid las instrucciones
7. Ahora deberíais ver las comunicaciones del dispositivo a través de Burp Suite


#### **Certificate Pinning** ####
Certificate Pinning debería ser usado por las aplicaciones móviles para validar un certificado específico del servidor y no aceptar ningún otro certificado que intente acceder al servidor de la app. Esto se consigue generando certificados auto-firmados *(self-signed)* o firmados de forma privada, y validando ciertos parámetros cuando el certificado se envíe al servidor. El Certificate Pinning evita que se puedan añadir confianzas de certificados ajenos en el dispositivo para poder interceptar las conexiones TLS de la app, como por ejemplo el certificado de Burp, aunque se puede *bypassear* si el dispositivo tiene jailbreak.

Uno de los problemas con el Certificate Pinning es que, cuando el certificado expira, debe haber una cierta coordinación entre el servidor y la app para poder actualizarlo, ya que habitualmente existe un periodo de superposición donde más de un certificado es válido. Otro problema es el almacenaje local del certificado: el peor método de almacenaje es en el directorio ‘Documents’ de la app. Un mejor método sería añadirlo como un recurso de la app en el directorio del ejecutable, y, un método incluso mejor sería integrarlo como un objeto ‘NSString’ dentro de la app, que sería la manera más complicada que tendría un atacante para manipularlo.

Si la app a analizar no tiene Certificate Pinning, sus comunicaciones serán visibles a través del proxy de forma automática. Si después de seguir los pasos de configuración del proxy no se pueden ver las comunicaciones, es muy probable que la app implemente Certificate Pinning de sus comunicaciones.

Una de las formas con las que se puede *bypassear* es activando desde los ‘Ajustes’ del dispositivo la aplicación ‘SSL Kill Switch 2’ previamente instalada. Esta app manipula las rutinas que manejan el certificado a bajo nivel, sobrescribiendo cualquier método delegado, incluyendo las rutinas de Certificate Pinning. Otra manera sería sobrescribir de forma automática los métodos relacionados con el Certificate Pinning con la opción 'ios sslpinning disable' de 'Objection'.


#### **TcpDump** ####
En algunos casos, no todas las comunicaciones entre el cliente y el servidor serán por los puertos 80 y 443, por lo que puede ser interesante analizar el tráfico completo del dispositivo para identificar posibles canales alternativos de comunicación. Una herramienta que permite capturar todo el tráfico entrante y saliente del dispositivo es *'tcpdump'*, que puede ser instalada desde Cydia.

Algunos ejemplos básicos de uso de la herramienta (que se pueden combinar) son:
```
tcpdump -w <archivo-output, p.e output.pcap>   # Para almacenar todo el tráfico en un archivo para poder analizarlo después con herramientas como Wireshark o Tshark
tcpdump host <ip>   # Para únicamente capturar el tráfico de un cierto host
tcpdump src/dst <ip>   # Para únicamente capturar el tráfico con origen/destino de/a un cierto host
tcpdump port <puerto>    # Para únicamente capturar el tráfico basado en un puerto de servicio
tcpdump <servicio, p.e. tcp, udp>    # Para únicamente capturar el tráfico basado en un servicio
tcpdump greater <N>   # Para únicamente capturar el tráfico que tenga un tamaño mayor a N
```

Más información sobre la herramienta *'tcpdump'* se puede encontrar fácilmente por internet, como por ejemplo en: [Wikipedia - TCPDump](https://es.wikipedia.org/wiki/Tcpdump), [TCPDump Cheat Sheet](https://packetlife.net/media/library/12/tcpdump.pdf), [Digital Forensics with TCPDump](https://www.securitynewspaper.com/2021/11/06/how-to-do-digital-forensics-of-a-hacked-network-with-tcpdump/), [Capturar el Tráfico de Red con Tcpdump y Wireshark](https://puerto53.com/linux/capturar-el-trafico-de-red-con-tcpdump-y-wireshark/) o [An introduction to using tcpdump at the Linux command line](https://opensource.com/article/18/10/introduction-tcpdump). (Agradecimientos a @Miguel_Arroyo76 por sugerir añadir esta herramienta al post)


#### **Análisis del Tráfico** ####
A partir de este punto, el análisis de las comunicaciones es el mismo que en las aplicaciones web habituales, siempre teniendo en cuenta las limitaciones que puedan estar presentes en el entorno de móviles.

<br>

Espero que esta guía os haya ayudado un poco en vuestra introducción al análisis de vulnerabilidades en aplicaciones iOS, y, de nuevo, cualquier feedback es bienvenido si encontráis errores o puntos de mejora.

***@martabyte***
