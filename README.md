# Infraestructura 2 - FortiGate, NAT, HTTPS y Seguridad Web

Laboratorio de seguridad en GNS3 utilizando FortiGate para segmentar la red de usuarios y servidores, controlar el acceso mediante políticas de firewall y publicar un servidor web mediante NAT/VIP.

## 🎥 Video demostrativo

[Ver video demostrativo](ENLACE_DEL_VIDEO)

---

## 📌 Propósito del laboratorio

El objetivo de esta infraestructura es implementar diferentes controles de seguridad utilizando FortiGate.

La red se encuentra dividida entre usuarios y servidores, permitiendo controlar qué tipo de comunicación puede existir entre ambos segmentos.

También se configuró la publicación del servidor web mediante HTTPS y se realizaron pruebas de comunicación con el servidor de base de datos.

---

## 🖥️ Topología

La infraestructura está formada por:

- 1 FortiGate
- 1 switch Cisco
- 1 PC de usuario
- 1 WEB-SERVER
- 1 DB-SERVER
- 1 nodo NAT
- 1 Webterm para realizar pruebas externas

### Diagrama de la topología

![Topología de Infraestructura 2](imagenes/topologia.png)

---

## 🌐 Segmentación de la red

Para separar los dispositivos se utilizaron diferentes VLAN.

### VLAN 10 - Usuarios

Red:

`10.12.48.0/25`

Gateway:

`10.12.48.1`

PC1:

`10.12.48.2/25`

---

### VLAN 20 - Servidores

Red:

`10.12.48.128/28`

Gateway:

`10.12.48.129`

WEB-SERVER:

`10.12.48.130/28`

DB-SERVER:

`10.12.48.131/28`

---

## 🔀 Configuración del switch

El switch Cisco se utiliza para transportar las diferentes VLAN de la infraestructura.

El enlace hacia FortiGate fue configurado como trunk para permitir el tráfico de las VLAN utilizadas.

Los equipos finales se conectan mediante puertos de acceso dependiendo de la red a la que pertenecen.

- PC1 → VLAN 10
- WEB-SERVER → VLAN 20
- DB-SERVER → VLAN 20

---

## 🌍 Acceso WAN

FortiGate obtiene conectividad hacia la red externa mediante la interfaz WAN.

Durante las pruebas se utilizó la dirección:

`192.168.42.23`

Esta dirección también fue utilizada para publicar el servicio HTTPS del servidor web.

---

## 🔐 Servidor WEB

El WEB-SERVER utiliza la dirección:

`10.12.48.130/28`

En el servidor se configuró Nginx con soporte HTTPS en el puerto:

`443`

Durante las pruebas fue posible acceder correctamente al sitio web utilizando HTTPS.

---

## 🗄️ Servidor de base de datos

El DB-SERVER utiliza:

`10.12.48.131/28`

En este servidor se configuró MariaDB utilizando el puerto:

`3306`

También se comprobó la comunicación entre el WEB-SERVER y la base de datos.

De esta manera, el servidor web puede utilizar el servicio de base de datos sin necesidad de exponer directamente MariaDB hacia redes externas.

---

## 🔄 NAT y publicación del servidor WEB

Para permitir el acceso externo al servidor web se creó un Virtual IP en FortiGate.

La publicación utilizada fue:

`192.168.42.23:8443`

hacia:

`10.12.48.130:443`

De esta manera, una conexión realizada hacia el puerto externo `8443` es redirigida al servicio HTTPS del WEB-SERVER.

---

## 🛡️ Políticas de Firewall

Se configuraron políticas de firewall para controlar el tráfico entre las diferentes redes.

Entre las comunicaciones controladas se encuentran:

- Tráfico desde la red de usuarios
- Tráfico hacia los servidores
- Acceso al WEB-SERVER mediante HTTPS
- Acceso al DB-SERVER mediante el puerto 3306
- Publicación del servidor web hacia la red externa

Las políticas permiten aplicar el principio de permitir únicamente las comunicaciones necesarias para cada servicio.

---

## ✅ Prueba de acceso HTTPS

Para comprobar el funcionamiento de la publicación del servidor se realizó una conexión desde el equipo externo hacia:

`https://192.168.42.23:8443`

La conexión fue redirigida por FortiGate hacia:

`10.12.48.130:443`

El navegador mostró correctamente la página configurada en el servidor web.

Esto confirmó el funcionamiento del VIP, la política de firewall y el servicio HTTPS.

---

## 🔍 DPI y protección contra SQL Injection

Como parte de los objetivos de la infraestructura se contempla utilizar las funciones de inspección de FortiGate para analizar el tráfico dirigido al servidor web.

También se contempla una regla de protección para detectar y bloquear intentos de SQL Injection antes de que alcancen el servidor.

Las evidencias correspondientes a estas pruebas se documentan en el repositorio una vez realizadas.

---

## 📸 Evidencias

En la carpeta `imagenes/` se almacenan las capturas utilizadas para demostrar la configuración y funcionamiento de la infraestructura.

Entre las evidencias se incluirán:

- Topología
- VLAN y trunk del switch
- Interfaces de FortiGate
- Políticas de firewall
- Configuración del VIP
- Acceso HTTPS al WEB-SERVER
- Funcionamiento del DB-SERVER
- Inspección DPI
- Prueba de SQL Injection
- Bloqueo o cuarentena del atacante

---

## ⚙️ Running Configuration

El backup de configuración de FortiGate se incluye en el repositorio para permitir una revisión más detallada de la configuración realizada.

Archivo:

`fortigate.conf`

También se incluirá la configuración utilizada en el switch Cisco.

---

## 📂 Scripts y comandos

En la carpeta `scripts/` se almacenan los comandos utilizados durante la configuración, verificación y pruebas del laboratorio.

---

## 📝 Conclusión

En esta infraestructura se implementó una red segmentada para separar los usuarios de los servidores y controlar la comunicación mediante FortiGate.

Se configuraron los servicios del WEB-SERVER y DB-SERVER y se publicó el servidor web utilizando HTTPS mediante un Virtual IP.

Las políticas de firewall permiten controlar qué servicios pueden comunicarse entre las diferentes redes y reducir la exposición innecesaria de los servidores.

Finalmente, la infraestructura permite incorporar mecanismos adicionales de inspección y protección del tráfico web mediante DPI y detección de ataques como SQL Injection.
