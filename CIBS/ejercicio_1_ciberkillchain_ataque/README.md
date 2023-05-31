# Ejercicio CiberKillChain - Ataque

## Alumno

Simón Rodríguez

## Enunciado

Armar una cyberkillchain usando técnicas de la matriz de Att&ck para un escenario relacionado al trabajo práctico de la carrera.

## Datos trabajo práctico

*Nota: El trabajo práctico objetivo es un supuesto ficticio y no el trabajo práctico de la carrera.*

El sistema está compuesto por un módulo IOT que sensa la humedad y temperatura de un recinto. Los datos son enviados a través del protocolo MQTT a un proveedor en la nube donde se reciben y almacenan los datos, para luego ser accedidos en el front-end por el usuario (bien sea por una aplicación o un sitio web). El usuario puede descargar reportes y realizar configuraciones al sistema, además de crear alertas y reportes personalizados. El servicio es usado por varios clientes diferentes.

Referencia de la Arquitectura (*pendiente actualización con AWS*):
![Arquitectura Cloud del TP de DAIoT](/ceiot_base/img/arquitectura-cloud_daiot.jpg)

## Resolución

### Objetivo del Ataque
El objetivo final del ataque es transmitir un ransomware enmascarado como un reporte a todos los clientes del servicio. Para lo cual, se realiza un primer ataque a la organización proveedora del servicio para acceder a la información de los clientes y luego enviar el reporte con el malware.

### 1. Reconnaisssance
Técnicas utilizadas: [T1589](https://attack.mitre.org/techniques/T1589), [T1591](https://attack.mitre.org/techniques/T1591), [T1593](https://attack.mitre.org/techniques/T1593).
* Obtener información de los empleados de la empresa proveedora del servicio ([T1589.003](https://attack.mitre.org/techniques/T1589/003/)) y sus roles ([T1591.004](https://attack.mitre.org/techniques/T1591/004/)). 
* Específicamente se busca en redes sociales ([T1593.001](https://attack.mitre.org/techniques/T1593/001/)) esta información para encontrar empleados que puedan tener acceso a la base de datos de clientes.

### 2. Weaponization
Técnicas utilizadas:

* 
* 

### 3. Delivery

* 
* 
### 4. Exploitation

* 
* 
### 5. Installation

* 
* 

### 6. Command & Control

* 
* 
### 7. Actions on Objectives

* Lanzamiento de un mail a la lista de clientes con un archivo adjunto que se hace pasar por un reporte, pero en realidad está cargado con malware/ransomware.
## Enlaces de Referencia

