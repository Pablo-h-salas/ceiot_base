# Ejercicio CiberKillChain - Defensa


## Alumno

Pablo Horacio Salas

## Sistema

Sistema de monitoreo de variables ambientales y alerta temprana

## Resolución

## Descripción 
A continuación se presenta una defensa que permita detectar y mitigar el ataque mencionado en el archivo [ciberKillChain-ataque](https://github.com/Pablo-h-salas/ceiot_base/blob/master/CIBS/ejercicio_1_ciberkillchain_ataque/entrega.md).
Se procede a abordar la defensa en distintas etapas del ataque, comenzando desde la fase final y recorriendo las etapas en sentido inverso.

## 7 - Actions on Objectives (Acciones sobre los objetivos)
### Detección:
Monitoreo de cambios en la base de datos. Herramientas como Prometheus o ELK stack supervisan continuamente los registros en la base de datos, permitiendo detectar cambios inusuales o modificaciones en los datos.
Por otro lado el sistema puede configurarse para verificar si los datos que están siendo almacenados en la base de datos coinciden con lo que se espera.
### Mitigación:
Validación de datos. Implementar validación de datos en la API, asegurando que los valores recibidos sean consistentes con las mediciones físicas reales. Si los valores no coinciden, se puede bloquear el acceso a la base de datos o generar una alerta para la intervención de administradores.

## 6 - Command & control (Mando y control)
### Detección:
Análisis de tráfico de red. Herramientas como Wireshark ayudan a identificar patrones de tráfico inusuales o conexiones a servidores externos C2. 
### Mitigación:
Bloqueo de IPs no autorizadas. Asegurar que la comunicación solo se permita entre nodos conocidos y confiables. Para ello se deberia bloquear todas las IPs no autorizadas, redirigiendo el tráfico hacia una red de aislamiento si es necesario.

## 5 - Installation (Instalación)
### Detección:
Monitoreo de procesos/tareas. Herramientas como Auditd permiten identificar que proceso se ejecutó, cuando y que tarea de realizó (borrar, modificar, leer). Esto puede ser muy util para detectar la instalacion de software malicioso como web shells. 
### Mitigación:
Limitar administradores autorizados. Permitir la instalación de software en los servidores criticos solo a los administradores autorizados. Se deberá bloquear la ejecución de programas no autorizados y revisar los permisos regularmente para prevenir accesos no autorizados.

## 4 - Exploit (Explotación)
### Detección:
// Escanear las vulnerabilidades de la API. Usar herramientas (por ejemplo Burp Suite) para identificar vulnerabilidades como inyecciones SQL o falta de autenticacion adecuada. 
Para detectar la explotacion se podria revisar los logs del servidor web, para registrar la IP solicitante, que endpoint se llamó y parametros se pasaron. 
un sistema de detección de intrusos (IDS) tambien nos ayudaria ya que alerta anomalias en la red, como patrones repetitivos. 
### Mitigación:
Brindar seguridad en la API. Implementar autenticación robusta con herramientas como tokens JWT o certificados TLS. 

## 3 - Delivery (Entrega)
### Detección:
Monitoreo de solicitudes HTTP. Identificar solicitudes maliciosas hacia la API, un WAF (Web Application Firewall) nos permitiria identificar tales solicitudes y bloquear ataques de inyección SQL.
Además, monitorear el tráfico de red para detectar patrones inusuales que puedan indicar un ataque.
### Mitigación:
Filtrado de tráfico. Configurar un WAF para bloquear solicitudes maliciosas y limitar la cantidad de peticiones hacia la API en un intervalo de corta duración.

## 2- Weaponization (Armamento)
### Detección:
En esta fase el atacante aun no envia nada, no se puede ver ni detectar nada aún.

### Mitigación:
limitar las vulnerabilidades: Esto se podria lograr encaneando el sistema frecuentemente en busca de fallas (como nikto para detectar configuraciones debiles en servidores web).
listar que puertos y servicios estan expuestos. 
Actualizar y parchear dependencias. 
Ejecutar exclusivamente script autorizados. Herramientas de whitelisting de aplciaciones nos ayudan a prevenir la ejecución de cualquier script que no haya sido previamente autorizado. Esto evita que cualquier script malicioso sea ejecutado en los servidores.

## 1- Reconnaissance (Reconocimiento)
### Detección:
Emplear monitores de escaneos de red. Implementar herramientas (por ejemplo Fail2Ban o Snort) para detectar escaneos de red o intentos de enumeración de rutas en el servidor. 
Generar alertas cuando se detecten patrones de escaneo o exploración de recursos.
### Mitigación:
Restricciones de acceso. Implementar firewalls y VPNs para restringir el acceso a recursos internos solo a direcciones IP conocidas. Limitar los servicios expuestos a los mínimos necesarios.
