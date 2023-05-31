# Ejercicio CiberKillChain - Ataque

## Alumno

Simón Rodríguez

## Enunciado

Armar una cyberkillchain usando técnicas de la matriz de Att&ck para un escenario relacionado al trabajo práctico de la carrera.

## Datos trabajo práctico

*Nota: El trabajo práctico objetivo es un supuesto ficticio y no el trabajo práctico de la carrera.*

El sistema está compuesto por un módulo IOT que sensa la humedad y temperatura de un recinto. Los datos son enviados a través del protocolo MQTT a un proveedor en la nube (AWS en este caso) donde se reciben y almacenan los datos, para luego ser accedidos en el front-end por el usuario (bien sea por una aplicación o un sitio web). El usuario puede descargar reportes y realizar configuraciones al sistema, además de crear alertas y reportes personalizados. El servicio es usado por varios clientes diferentes.

Referencia de la Arquitectura (*pendiente actualización con AWS*):
![Arquitectura Cloud del TP de DAIoT](/ceiot_base/img/arquitectura-cloud_daiot.jpg)

## Resolución

### Objetivo del Ataque
El objetivo final del ataque es transmitir un ransomware enmascarado como un reporte a todos los clientes del servicio. Para lo cual, se realiza un primer ataque a la organización proveedora del servicio para acceder a la información de los clientes y luego enviar el reporte con el malware.

### 1. Reconnaisssance
> Búsqueda de vulnerabilidades y reconocimiento del objetivo para realizar el ataque.

Técnicas utilizadas: [T1589](https://attack.mitre.org/techniques/T1589), [T1591](https://attack.mitre.org/techniques/T1591), [T1593](https://attack.mitre.org/techniques/T1593).

* Obtener información de los empleados de la empresa proveedora del servicio [[T1589.003](https://attack.mitre.org/techniques/T1589/003/)] y sus roles [[T1591.004](https://attack.mitre.org/techniques/T1591/004/)]. 
* Específicamente se busca en redes sociales [[T1593.001](https://attack.mitre.org/techniques/T1593/001/)] esta información para encontrar empleados que puedan tener acceso a la base de datos de clientes.

### 2. Weaponization
> Preparación de los recursos necesarios para ejecutar el plan de ataque.

Técnicas utilizadas: [T1583](https://attack.mitre.org/techniques/T1583), [T1585](https://attack.mitre.org/techniques/T1585), [T1588](https://attack.mitre.org/techniques/T1588).

* Adquirir un Servidor Virtual (VPS) y un dominio para crear un sitio web que simule ser un sistema interno de la empresa.
* Se establece un correo electrónico en base al dominio creado, desde donde se enviará un ataque de phishing [[T1585.002](https://attack.mitre.org/techniques/T1585/002)].
* Adquirir malware que sirva para ejecutar el secuestro de los sistemas objetivo (ransomware) [[T1588.001](https://attack.mitre.org/techniques/T1588/001)].

### 3. Delivery
> Envío de los recursos establecidos anteriormente para lograr un acceso inicial al sistema.

Técnicas utilizadas: [T1566](https://attack.mitre.org/techniques/T1566), [T1204](https://attack.mitre.org/techniques/T1204)

* Se envía un email de phishing que simule ser una alerta por parte del proveedor cloud, avisando que el sistema tiene un problema en la base de datos y requiere su atención [[T1566.002](https://attack.mitre.org/techniques/T1566/002/)].
* El enlace envía al sitio web previamente preparado que simula ser el servicio de login del proveedor cloud y pide al usuario ingresar con sus datos [[T1204.001](https://attack.mitre.org/techniques/T1204/001/)].
* El usuario ingresa sus datos, los cuales son enviados a la base de datos del servidor privado y luego es inmediatamente redirigido al sitio real de login, donde recibe un error de clave errónea.
* El usuario ingresa al sistema y no percibe que fue víctima del ataque.
### 4. Exploitation
> Explotación de la vulnerabilidad y acceso inicial al sistema.

Técnicas utilizadas: [T1078](https://attack.mitre.org/techniques/T1078)

* Se usan las credenciales obtenidas en el paso anterior para acceder al proveedor cloud [[T1078.004](https://attack.mitre.org/techniques/T1078/004/)].
* Como la cuenta es de administrador, se tienen todos los privilegios.

### 5. Installation
> Ejecución del ataque dentro del sistema.

Técnicas utilizadas: [[T1651](https://attack.mitre.org/techniques/T1651/)]

* Una vez dentro del sistema, se procede a tomar control administrativo del mismo.
* Suponiendo que el acceso a la base de datos está configurada con IAM, entonces se genera un token de autentificación para poder realizar la conexión a la base de datos.
* Se accede también al servicio de envío de emails (p.ej. Amazon SES).
### 6. Command & Control
> Se establece un canal de comunicación para controlar remotamente a la víctima.

Técnicas utilizadas: [T1090](https://attack.mitre.org/techniques/T1090/)

* Para evitar la detección del origen del ataque se utilizan múltiples proxies para redirigir las comunicaciones a través de diferentes puntos [[T1090.003](https://attack.mitre.org/techniques/T1090/003/)].

### 7. Actions on Objectives

Técnicas utilizadas: [T1119](https://attack.mitre.org/techniques/T1119/), [T1538](https://attack.mitre.org/techniques/T1538/)

* Se identifican los datos de la base de datos con la información de los clientes utilizando la conexión a la base de datos y una aplicación de ETL (Extract, Transform, Load). 
* Se prepara un email simulando ser un reporte y se adjunta un archivo malicioso.
* Se envía un email a la lista de clientes a través del servicio de envío de emails, utilizando el Dashboard del proveedor cloud.

