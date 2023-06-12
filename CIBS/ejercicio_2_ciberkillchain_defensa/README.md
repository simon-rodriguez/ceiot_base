# Ejercicio CiberKillChain - Defensa

## Alumno

Simón Rodríguez

## Enunciado

Desarrollar la defensa en función del ataque planteado en orden inverso.

Para cada etapa elegir una sola defensa, la más importante, considerar recursos limitados.

## Resolución

### Objetivo del Ataque
El objetivo final del ataque principal se logró y por consiguiente también el objetivo del ataque auxiliar, que posibilitaba al ataque principal. Como consecuencia, se logró acceder a la base de datos de clientes de la empresa y posteriormente transmitir el ransomware a los clientes y encriptar sus datos.

### Ataque Principal
### 7. Actions on Objectives
> El usuario se da cuenta que está infectado por un ransomware cuando todos sus archivos pasan a tener una extensión extraña, cambia el fondo de pantalla por un mensaje de extorsión y aparece un archivo de texto con información sobre el procedimiento a seguir.

Técnicas de mitigación: [M1040 - Behaviour Prevention on Endpoint](https://attack.mitre.org/mitigations/M1040/), [M1053 - Data Backup](https://attack.mitre.org/mitigations/M1053/)

* En Windows 10 se puede activar la protección desde la nube y reglas de reducción de superficie de ataque para bloquear la ejecución de archivos que parecen ser ramsonware.
* Realizar copias de seguridad frecuentemente en dispositivos externos (que no estén siempre conectados al sistema) puede permitir la recuperación de los datos de un equipo afectado por un ransomware.

### 6. Command & Control

Técnicas de mitigación: [M1031 - Network Intrusion Prevention](https://attack.mitre.org/mitigations/M1031/)

* Existen sistemas de prevención y detección de intrusión en la red que utilizan ciertas señales en la red para identificar tráfico de malware conocido, lo que puede mitigar la actividad a nivel de redes.
* Es importante mantener las aplicaciones de defensa contra malware y antivirus, así como el firewall actualizados, de modo que sus bases de datos puedan identificar el tráfico de red asociado a malwares particulares.

### 5. Installation

Técnicas de mitigación: [M1028 - Operating System Configuration](https://attack.mitre.org/mitigations/M1028/)

* Revisar y configurar el sistema operativo a través de políticas que impidan que las tareas programadas se ejecuten como Administrador o SYSTEM, y en cambio se deban ejecutar con un usuario autenticado.
* Para evitar que abuse el mecanismo de elevación de privilegios, se debe limitar la cantidad de aplicaciones que tienen la capacidad de realizar esta acción y además configurar el sistema de control de cuentas de usuario en Windows (UAC) a un nivel elevado.  

### 4. Exploitation

Técnicas de mitigación: [M1049 - Antivirus/Antimalware](https://attack.mitre.org/mitigations/M1049/)

* Los usuarios deben estar atentos a los archivos que descargan de internet, incluso si provienen de fuentes que aparentan ser confiables. Es prudente escanear los archivos descargados con un antivirus/antimalware automáticamente.
* Si no se necesita usar PowerShell, éste se puede deshabilitar o limitar su uso solo a usuarios de tipo administrador (¡y no usar esta cuenta para tareas diarias!). [[M1042](https://attack.mitre.org/mitigations/M1042/)] / [[M1026](https://attack.mitre.org/mitigations/M1026/)]

### 3. Delivery

Técnicas de mitigación: [M1017 - User Training](https://attack.mitre.org/mitigations/M1017/)

* Los ataques de suplantación de identidad vía email (phishing) son muy comunes y cada vez más ingeniosos y realistas. Es importante prestar atención a la dirección de correo del remitente. En caso de que sea un email que se recibe con frecuencia, buscar detectar diferencias notables en el contenido (p.ej. un reporte que originalmente se descarga desde el sitio web, ahora *sospechosamente* se envía como un archivo adjunto pero nunca se informó del cambio).
* Las técnicas para enmascarar un enlace son más inteligentes y es fácil confundir un enlace legítimo con uno falso. De ser posible, el usuario debería buscar acceder a la misma información por otra vía (p.ej. dirigiéndose al sitio web tipeando la dirección URL y allí buscar la información que indica el correo, en vez de hacer clic en el enlace que aparece en el correo).

### Ataque Auxiliar (primer ciclo de ataque)
### 7. Actions on Objectives (primer ciclo de ataque)
> La empresa empieza a recibir mensajes de sus clientes informando del ataque de ransomware. El equipo de atención al cliente recibe información sobre el correo de phishing que fue enviado a los clientes, por lo que el equipo de IT supone que la base de datos fue comprometida. El administrador de la cuenta comprometiva revisa los registros de ingreso y encuentra inicios de sesión extraños.

Técnicas de mitigación: [M1018 - User Account Management](https://attack.mitre.org/mitigations/M1018/)

* Para evitar el acceso a todos los sistemas, se debería utilizar el principio de menos privilegios y limitar lo que puede hacer cada cuenta.

### 6. Command & Control (primer ciclo de ataque)

Técnicas de mitigación: [M1037 - Filter Traffic Network](https://attack.mitre.org/mitigations/M1037/)

* Para evitar que cualquier IP se pueda conectar a los servicios creados en la nube, se pueden crear filtros que limiten el acceso a un número limitado de IPs autorizadas, en vez de permiter el acceso irrestricto.

### 5. Installation (primer ciclo de ataque)

Técnicas de mitigación: [M1026 - Privileged Account Management](https://attack.mitre.org/mitigations/M1026/)

* Para evitar que se puedan crear nuevos roles o tokens de autentificación en caso de que alguna cuenta quede comprometida, se debe limitar el número de usuarios con privilegios para realizar estas acciones.

### 4. Exploitation (primer ciclo de ataque)

Técnicas de mitigación: [M1036 - Account Use Policies](https://attack.mitre.org/mitigations/M1036/)

* Para evitar el ingreso por parte de terceros con credenciales comprometidas, se puede agregar una capa de seguridad adicional activando otro factor de autentificación. Por ejemplo, utilizando un generador de claves/tokens o la solicitud de confirmación de acceso vía push al dispositivo móvil del usuario.


### 3. Delivery (primer ciclo de ataque)

Técnicas de mitigación: [M1017 - User Training](https://attack.mitre.org/mitigations/M1017/), [M1054 - Software Condiguration](https://attack.mitre.org/mitigations/M1054/)

* Los ataques de suplantación de identidad vía email (phishing) son muy comunes y cada vez más ingeniosos y realistas, por lo que se debe entrenar al personal para detectarlos, en especial aquellos que tienen roles importantes.
    * Las personas deben poder identificar si el remitente es confiable y si es quien dice ser.
    * También en caso de seguir un enlace, verificar que se encuentran en el lugar correcto y por las razones correctas.
    * En este caso en particular, la persona podía percatarse de que algo andaba mal incluso después de haber entregado las credenciales, ya que al entrar al sistema no había ninguna alerta de seguridad que requiriera su atención (tal vez incluso no había configurado este tipo de alertas).

* Se puede agregar también una alerta al principio del contenido de todos los mensajes que provienen fuera del dominio de la empresa o fuera de la lista de remitentes confiables/autorizados. Del mismo modo, se pueden agregar mecanismos de autentificación y anti-spoofing basados en chequeos de validez del dominio del remitente. [[M1054](https://attack.mitre.org/mitigations/M1054/)]

### 2. Weaponization
> Difícil de mitigar, ya que está fuera del alcance de la víctima.

### 1. Reconnaisssance

Técnicas de mitigación: [[M1056 - Pre-compromise](https://attack.mitre.org/mitigations/M1056/)]

* Si bien es díficil de mitigar, los empleados pueden ser precavidos con la información que comparten en redes sociales de forma pública. Por ejemplo, no mencionar detalles específicos sobre los accesos que se tiene.
