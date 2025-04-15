# Ejercicio CiberKillChain - Ataque
# Alumno
Pablo Horacio Salas
# Sistema victima 
### Sistema de monitoreo de variables ambientales y alerta temprana
El “Sistema de monitoreo de variables ambientales y alerta temprana” tiene como objetivo proteger ecosistemas mediante la medición de factores ambientales
relacionados a la salud y el bienestar de las personas. Este sistema emplea diversos sensores para capturar las variables en el medio ambiente
y desplegar una red de comunicaciones de largo alcance (LoRaWAN). Además, permitirá
visualizar estos valores en una interfaz web y generar una alerta en caso de que se excedan
ciertos umbrales preestablecidos. Los componentes del proyecto incluyen sensores asequibles,
redes IoT eficientes, una plataforma de procesamiento de datos en tiempo real y sistemas de
alerta temprana.

Los datos recolectados son enviados a un sistema de procesamiento basado en la nube, con una base de datos alojada en Google Cloud, lo que permite escalabilidad y alta disponibilidad.

Además, el sistema permite visualizar estos valores a través de una interfaz web, que está alojada en un servidor interno. Los usuarios acceden a la plataforma a través de un navegador, donde pueden ver los datos en tiempo real y recibir alertas si se exceden ciertos umbrales preestablecidos, lo que les permite tomar medidas preventivas para proteger el ecosistema y la salud pública.

[Link del trabajo práctico](https://drive.google.com/file/d/18wzF2KUW19Co_6zkKrUzV4OzF9rQ69Qz/view?usp=sharing)

# Objetivo
Falsificar datos ambientales para evitar sanciones regulatorias.

# Resolución

## Reconnaissance
[Reconnaissance - TA0043](https://attack.mitre.org/tactics/TA0043/)

[T1598 - Gather Victim Network Information](https://attack.mitre.org/techniques/T1598/):
Identificar la infraestructura del sistema incluyendo servidores, APIs y bases de datos en la nube.
Se identifica que el sistema posee una base de datos en Google Cloud y una API REST para la recolección y visualización de datos ambientales.
Herramienta útil: dirsearch para enumerar rutas del servidor.

[T1595 - Active Scanning](https://attack.mitre.org/techniques/T1595/): Buscar puntos debiles en la interfaz web (falta de cifrado, APIs inseguras).
Se analiza el dominio de la interfaz web probando endpoints conocidos de la API pública. Se descubre un endpoint vulnerable que no requiere autenticación y es susceptible a inyeccion SQL. 
Herramienta útil: sqlmap para automatizar la detección de vulnerabilidades de inyección SQL.

La combinación de ambas tecnicas permiten detectar una API vulnerable a inyección SQL que permite acceso no autorizado a la base de datos.  


## Weaponization
[Resource Development - TA0042](https://attack.mitre.org/tactics/TA0042/)

[T1587.001 - Develop Malware](https://attack.mitre.org/techniques/T1587/001/): crear un malware para modificar para interceptar y modificar los datos que se envian de los sensores a la base de datos.

[T1608.001 - Deploy Compromised Software](https://attack.mitre.org/techniques/T1608/001/): Diseñar un script malicioso para editar los registros de las variables ambientales en tiempo real.

## Delivery
[Initial access - TA0001](https://attack.mitre.org/tactics/TA0001/)

[T1190 - Exploit Public-Facing Application](https://attack.mitre.org/techniques/T1190/): acceder a la base de datos (si hay inyección SQL vulnerable o autenticacion debil).

[T1200 - Hardware Additions](https://attack.mitre.org/techniques/T1200/): Intentar inyectar dispositivos maliciosos en la red IoT para alterar lecturas.

[T1566 - Phishing](https://attack.mitre.org/techniques/T1566/): Enviar correos de phishing a administradores del sistema para obtener credenciales de acceso.

## Exploit
[Execution - TA0002](https://attack.mitre.org/tactics/TA0002/)

[T1059.004 - Command and Scripting Interpreter: Unix Shell](https://attack.mitre.org/techniques/T1059/004/): Ejecutar comandos remotos para alterar las lecturas en los sensores.

[T1565.002 - Data Manipulation: Stored Data Manipulation:](https://attack.mitre.org/techniques/T1565/002/): Modificar registros históricos en la base de datos para encubrir anomalías.

    
## Installation 
[Persistence - TA0003](https://attack.mitre.org/tactics/TA0003/)

[T1505.003 - Server Software Component: Web Shell](https://attack.mitre.org/techniques/T1505/003/): Instalar web shells en el servidor para mantener acceso persistente.

[T1546 - Event Triggered Execution](https://attack.mitre.org/techniques/T1546/):Crear procesos automáticos para modificar datos ambientales periódicamente.

    
## Command & control
[Command and control - TA0011](https://attack.mitre.org/tactics/TA0011/)

[T1071.001 - Application Layer Protocol: Web Protocols](https://attack.mitre.org/techniques/T1071/): Conectar con un servidor C2 (command & control) para continuar con la modificación de los valores reportados.

## Actions on Objectives
[Impact - TA0040](https://attack.mitre.org/tactics/TA0040/)

[T1565.001 - Data Manipulation: Runtime Data Manipulation](https://attack.mitre.org/techniques/T1565/001/): Manipular datos de manera que las autoridades no detecten ninguna anomalia en las mediciones.

[T1485 - Data Destruction](https://attack.mitre.org/techniques/T1485/): Eliminar registros críticos que puedan generar sospechas.
    
