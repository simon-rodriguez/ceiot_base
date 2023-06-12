# Ejercicio CiberKillChain - Ataque

## Alumno

Simón Rodríguez

## Enunciado

Armar una cyberkillchain usando técnicas de la matriz de Att&ck para un escenario relacionado al trabajo práctico de la carrera.

## Datos trabajo práctico

*Nota: El trabajo práctico objetivo es un supuesto ficticio y no el trabajo práctico de la carrera.*

El sistema está compuesto por un módulo IOT que sensa la humedad y temperatura de un recinto. Los datos son enviados a través del protocolo MQTT a un proveedor en la nube (AWS en este caso) donde se reciben y almacenan los datos, para luego ser accedidos en el front-end por el usuario (bien sea por una aplicación o un sitio web). El usuario puede descargar reportes y realizar configuraciones al sistema, además de crear alertas y reportes personalizados. El servicio es usado por varios clientes diferentes.

## Resolución

### Objetivo del Ataque
El objetivo final del ataque es transmitir un ransomware enmascarado como un reporte a todos los clientes del servicio. Para lo cual, se realiza un primer ataque a la organización proveedora del servicio para acceder a la información de los clientes y luego enviar el reporte con el malware.

### 1. Reconnaisssance
> Búsqueda de vulnerabilidades y reconocimiento del objetivo para realizar el ataque.

Técnicas utilizadas: [T1589 - Gather Victim Identity Information](https://attack.mitre.org/techniques/T1589), [T1591 - Gather Victim Org Information](https://attack.mitre.org/techniques/T1591), [T1593 - Search Open Websites/Domains](https://attack.mitre.org/techniques/T1593).

* Obtener información de los empleados de la empresa proveedora del servicio [[T1589.003](https://attack.mitre.org/techniques/T1589/003/)] y sus roles [[T1591.004](https://attack.mitre.org/techniques/T1591/004/)]. 
* Específicamente se busca en redes sociales esta información para encontrar empleados que puedan tener acceso a la base de datos de clientes [[T1593.001](https://attack.mitre.org/techniques/T1593/001/)]. Se crea una cuenta con una persona falsa, simulando ser miembro de un equipo de recursos humanos/reclutamiento, para no levantar sospechas y visualizar todo el perfil de la víctima. [[T1585.001](https://attack.mitre.org/techniques/T1585/001/)]
* Se analiza el funcionamiento de la empresa en cuanto al sistema de envío de reportes, haciéndose pasar por un cliente. Con esto se sabe que la empresa envía a todos sus clientes un reporte mensual con un resumen de los datos obtenidos en el mes y un análisis. [[T1591.002/.003](https://attack.mitre.org/techniques/T1591/)]

### 2. Weaponization
> Preparación de los recursos necesarios para ejecutar el plan de ataque.

Técnicas utilizadas: [T1583 - Acquire Infrastructure](https://attack.mitre.org/techniques/T1583), [T1585 - Establish Accounts](https://attack.mitre.org/techniques/T1585), [T1588 - Obtain Capabilities](https://attack.mitre.org/techniques/T1588).

* Adquirir un Servidor Virtual (VPS) y un dominio para crear un sitio web que simule ser un sistema interno de la empresa. Además se adquiere también un dominio ".zip" que servirá para engañar a las vícitmas de que están descargando un archivo desde la web "real" en vez de seguir un link a una web falsa.
    * La dirección .zip sería del estilo "ReporteMensual.zip", utilizando una técnica para enmascarar la dirección como real [[Descripción de la Técnica]](https://www.malwarebytes.com/blog/news/2023/05/zip-domains).
* Se establece un correo electrónico en base al dominio creado, desde donde se enviará un ataque de phishing [[T1585.002](https://attack.mitre.org/techniques/T1585/002)].
* Adquirir malware que sirva para ejecutar el secuestro de los sistemas objetivo (ransomware) [[T1588.001](https://attack.mitre.org/techniques/T1588/001)]. Se obtiene un ransomware del estilo "Medusa" que es Ransomware-as-a-Service (RaaS), de modo que la infraestructura sea gestionada por el grupo.

### Primer ciclo de Ataque
#### Delivery
> Envío de los recursos establecidos anteriormente para lograr un acceso inicial al sistema.

Técnicas utilizadas: [T1566 - Phishing](https://attack.mitre.org/techniques/T1566), [T1204 - User Execution](https://attack.mitre.org/techniques/T1204)

* Se envía un email de phishing que simule ser una alerta por parte del proveedor cloud, avisando que el sistema tiene un problema en la base de datos y requiere su atención [[T1566.002](https://attack.mitre.org/techniques/T1566/002/)].
* El enlace envía al sitio web previamente preparado que simula ser el servicio de login del proveedor cloud y pide al usuario ingresar con sus datos [[T1204.001](https://attack.mitre.org/techniques/T1204/001/)].
* El usuario ingresa sus datos, los cuales son enviados a la base de datos del servidor privado y luego es inmediatamente redirigido al sitio real de login, donde recibe un error de clave errónea.
* El usuario ingresa al sistema y no percibe que fue víctima del ataque.
#### Exploitation
> Explotación de la vulnerabilidad y acceso inicial al sistema.

Técnicas utilizadas: [T1078 - Valid Accounts](https://attack.mitre.org/techniques/T1078)

* Se usan las credenciales obtenidas en el paso anterior para acceder al proveedor cloud [[T1078.004](https://attack.mitre.org/techniques/T1078/004/)].
* Como la cuenta es de administrador, se tienen todos los privilegios.

#### Installation
> Ejecución del ataque dentro del sistema.

Técnicas utilizadas: [[T1651 - Cloud Administration Command](https://attack.mitre.org/techniques/T1651/)]

* Una vez dentro del sistema, se procede a tomar control administrativo del mismo.
* Suponiendo que el acceso a la base de datos está configurada con IAM, entonces se genera un token de autentificación para poder realizar la conexión a la base de datos de clientes.
* Se abren los puertos y las IPs para poder realizar conexiones desde otras IPs.

#### Command & Control
> Se establece un canal de comunicación para acceder a la base de datos.

Técnicas utilizadas: [T1090 - Proxy](https://attack.mitre.org/techniques/T1090/)

* Se conecta a la base de datos utilizando el token creado en el dashboard del proveedor Cloud.
* Para evitar la detección del origen del ataque se utilizan múltiples proxies para redirigir las comunicaciones a través de diferentes puntos [[T1090.003](https://attack.mitre.org/techniques/T1090/003/)].

#### (Partial) Actions on Objectives

Técnicas utilizadas: [T1119 - Automated Collection](https://attack.mitre.org/techniques/T1119/).

* Se identifican los datos de la base de datos con la información de los clientes utilizando la conexión a la base de datos y una aplicación de ETL (Extract, Transform, Load). 

### Continuación Ataque Principal

### 3. Delivery
> Envío de un link para descargar el Malware utilizando los recursos establecidos anteriormente.

Técnicas utilizadas: [T1566.002 - Phishing / Spearphising Link](https://attack.mitre.org/techniques/T1566/002/), [T1204 - User Execution](https://attack.mitre.org/techniques/T1204)

* Se prepara un email simulando ser un reporte mensual para ser enviado a todas las direcciones obtenidas previamente.
* Este email contiene un enlace del tipo `https://nombreempresa.com∕reportes∕downloads∕@ReporteMensual.zip`, similar al que envía la empresa legítimamente.
    * El arroba y el uso de caracteres que parecen "/" pero no lo son, engañan al explorador y terminan dirigiéndose al dominio ReporteMensual.zip. Aunque en la barra aparece la dirección completa. [[Más información sobre esta técnica]](https://www.malwarebytes.com/blog/news/2023/05/zip-domains)
    * Las personas no se dan cuenta de esto y descargan el archivo con el reporte y el malware.
### 4. Exploitation
> Explotación de la vulnerabilidad y acceso inicial al sistema.

Técnicas utilizadas: [T1204 - User Execution](https://attack.mitre.org/techniques/T1204), [T1059 - Command and Scripting Interpreter: PowerShell](https://attack.mitre.org/techniques/T1059/001/)

* El usuario descomprime el archivo zip que descargó e intenta abrir el archivo del reporte, pero en realidad es un archivo BAT que ejecuta un script de PowerShell. [[T1059.001]](https://attack.mitre.org/techniques/T1059/001/)


### 5. Installation
> El malware se instala en el sistema de la víctima y evita ser detectado o detenido.

Técnicas utilizadas: [T1547 - Boot Autostart Execution](https://attack.mitre.org/techniques/T1547/), [T1053 - Schedule Task](https://attack.mitre.org/techniques/T1053/), [T1548 - Abuse Elevation Control Mechanism](https://attack.mitre.org/techniques/T1548/)

* El script establece una nueva entrada en el registro de modo que se ejecute el ransomware siempre que se reinicie el equipo. [[T1547.001](https://attack.mitre.org/techniques/T1547/001/)]
* Se agrega una tarea en el sistema para que el ransomware se ejecute cada 10 minutos. [[T1053.005](https://attack.mitre.org/techniques/T1053/005/)]
* Se aumentan los privilegios utilizando las funciones propias del equipo infectado para poder ejecutar scripts y cualquier otra función sin problemas. [[T1548](https://attack.mitre.org/techniques/T1548/)]

### 6. Command & Control
> Se establece un canal de comunicación para controlar remotamente a la víctima y ejecutar comandos.

Técnicas utilizadas: [T1071 - Application Layer Protocol](https://attack.mitre.org/techniques/T1071/), [T1105 - Ingress Tool Transfer](https://attack.mitre.org/techniques/T1105/)

* La conexión se realiza a través del protocolo web HTTPS. [[T1071.001](https://attack.mitre.org/techniques/T1071/001/)]
* El ransomware Medusa utiliza el servicio de certificados de Windows para transferir archivos desde el servidor (certutil.exe). [[T1105](https://attack.mitre.org/techniques/T1105/)]

### 7. Actions on Objectives

Técnicas utilizadas: [T1486 - Data Encrypted for Imapct](https://attack.mitre.org/techniques/T1486/), [T1490 - Inhibit System Recovery](https://attack.mitre.org/techniques/T1490/)

* Se encriptan los datos del equipo de la víctima utilizando un algoritmo de encriptación. [[T1486](https://attack.mitre.org/techniques/T1486/)]
* Para evitar que la víctima intente recuperar el sistema, eliminando copias fantasma (shadow copies), backups, eliminando la recuperación del sistema en Windows. [[T1490](https://attack.mitre.org/techniques/T1490/)]
* Se solicita a la víctima un rescate para poder desencriptar los archivos.

