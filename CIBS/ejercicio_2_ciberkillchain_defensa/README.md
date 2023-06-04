# Ejercicio CiberKillChain - Defensa

## Alumno

Simón Rodríguez

## Enunciado

Desarrollar la defensa en función del ataque planteado en orden inverso.

Para cada etapa elegir una sola defensa, la más importante, considerar recursos limitados.

## Resolución

### Objetivo del Ataque
El objetivo final del ataque se cumplió, por lo que se transmitió un ransomware enmascarado como un reporte a todos los clientes del servicio vía la lista de distribución de emails.

### 7. Actions on Objectives

Técnicas de mitigación: [M1018 - User Account Management](https://attack.mitre.org/mitigations/M1018/)

* Para evitar el acceso a todos los sistemas, se debería utilizar el principio de menos privilegios y limitar lo que puede hacer cada cuenta.

### 6. Command & Control

Técnicas de mitigación: [M1037 - Filter Traffic Network](https://attack.mitre.org/mitigations/M1037/)

* Para evitar que cualquier IP se pueda conectar a los servicios creados en la nube, se pueden crear filtros que limiten el acceso a un número limitado de IPs autorizadas, en vez de permiter el acceso irrestricto.

### 5. Installation

Técnicas de mitigación: [M1026 - Privileged Account Management](https://attack.mitre.org/mitigations/M1026/)

* Para evitar que se puedan crear nuevos roles o tokens de autentificación en caso de que alguna cuenta quede comprometida, se debe limitar el número de usuarios con privilegios para realizar estas acciones.

### 4. Exploitation

Técnicas de mitigación: [M1036 - Account Use Policies](https://attack.mitre.org/mitigations/M1036/)

* Para evitar el ingreso por parte de terceros con credenciales comprometidas, se puede agregar una capa de seguridad adicional activando otro factor de autentificación. Por ejemplo, utilizando un generador de claves/tokens o la solicitud de confirmación de acceso vía push al dispositivo móvil del usuario.


### 3. Delivery

Técnicas de mitigación: [M1017 - User Training](https://attack.mitre.org/mitigations/M1017/)

* Los ataques de suplantación de identidad vía email (phishing) son muy comunes y cada vez más ingeniosos y realistas, por lo que se debe entrenar al personal para detectarlos, en especial aquellos que tienen roles importantes.
    * Las personas deben poder identificar si el remitente es confiable y si es quien dice ser.
    * También en caso de seguir un enlace, verificar que se encuentran en el lugar correcto y por las razones correctas.
    * En este caso en particular, la persona podía percatarse de que algo andaba mal incluso después de haber entregado las credenciales, ya que al entrar al sistema no había ninguna alerta de seguridad que requiriera su atención (tal vez incluso no había configurado este tipo de alertas).

* Se puede agregar también una alerta al principio del contenido de todos los mensajes que provienen fuera del dominio de la empresa o fuera de la lista de remitentes confiables/autorizados.

### 2. Weaponization
> Difícil de mitigar, ya que está fuera del alcance de la víctima.

### 1. Reconnaisssance

Técnicas de mitigación: -

* Si bien es díficil de mitigar, los empleados pueden ser precavidos con la información que se comparten en redes sociales de forma pública. Por ejemplo, no mencionar detalles específicos sobre los accesos que tiene.
