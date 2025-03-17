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
  - Buscar puntos debiles en la interfaz web (falta de cifrado, APIs inseguras)
  - Investigar cuáles son los sensores que miden las variables críticas como niveles de gases o radiación.
  - Investigar fallas de seguridad en la base de datos de Google Cloud o en la infraestructura de la plataforma
## Weaponization
[Resource Development - TA0042](https://attack.mitre.org/tactics/TA0042/)
  - crear un malware para modificar para interceptar y modificar los datos que se envian de los sensores a la base de datos.
  - Crear un script para editar los registros de las variables ambientales en tiempo real.
## Delivery
[Initial access - TA0001](https://attack.mitre.org/tactics/TA0001/)
  - Acceso a la interfaz: acceder a la base de datos (si hay inyección SQL vulnerable o autenticacion debil).
  - Acceso al sistema IoT: enviar comandos a los dispositivos IoT para alterar las lecturas.
  - Enviar correos pishing a los administradores del sistema.
## Exploit 
[Execution - TA0002](https://attack.mitre.org/tactics/TA0002/)
  - Modificar la lectura de los sensores críticos una vez que se ha logrado entregar el malware o acceder a la base de datos
  - Manipular los datos en tiempo real mediante la alteracion de las lecturas de los sensores.
  - Desactivar alarmas configuradas.
## Installation 
[Persistence - TA0003](https://attack.mitre.org/tactics/TA0003/)
  -  Instalar backdoors en el servidor web para mantener acceso continuo.
  -  Crear procesos automaticos asegurando que los datos falsificados perduren.
## Command & control
[Command and control - TA0011](https://attack.mitre.org/tactics/TA0011/)
  - Conectar con un servidor C2 (command & control) para continuar con la modificación de los valores reportados.
## Actions on Objectives
[Impact - TA0040](https://attack.mitre.org/tactics/TA0040/)
  - Manipular datos de manera que las autoridades no detecten ninguna anomalia en las mediciones.
