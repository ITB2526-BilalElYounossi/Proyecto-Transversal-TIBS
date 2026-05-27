# Web-SFTP — Portal interno, NGINX y transferencia segura

## 1. Objetivo del servidor web-sftp
En esta parte del proyecto se configuró el servidor `web-sftp`, encargado de ofrecer el portal web interno de InnovateTech y el servicio de transferencia segura de ficheros mediante SFTP.

Este servidor cumple dos funciones principales dentro de la infraestructura:

- Publicar el portal web interno mediante NGINX y PHP.
- Permitir acceso SFTP utilizando usuarios del dominio Samba AD.

Además, el portal web se conecta con la base de datos MariaDB del proyecto para mostrar información real de la empresa, como empleados, productos, vídeos, llamadas, mediciones de ancho de banda y otros datos gestionados por el sistema.

El objetivo de esta máquina es actuar como punto central de acceso para los usuarios internos. Desde el portal, los usuarios pueden iniciar sesión con sus credenciales corporativas, consultar información según su perfil y descargar datos en formato CSV o SQL.

El servidor también integra accesos hacia los servicios multimedia del proyecto:

- Radio / Icecast.
- Jitsi Meet.
- Jellyfin.

De esta forma, el servidor `web-sftp` conecta la parte web, la autenticación centralizada, la base de datos y los servicios multimedia en un único portal interno.

## 2. Datos del servidor
La máquina utilizada para esta parte del proyecto fue `web-sftp`.

| Elemento | Valor |
|---|---|
| Nombre del servidor | `web-sftp` |
| FQDN | `web-sftp.btis.inovate.cat` |
| IP pública | `52.1.67.249` |
| IP privada | `10.0.5.140` |
| Usuario de administración | `adminitb` |
| Servicios principales | NGINX, PHP-FPM, SFTP, integración con Samba AD |
| Dominio | `BTIS.INOVATE.CAT` |
| Servidor Samba AD | `samba-ad.btis.inovate.cat` |
| Base de datos | `innovatetech` en MariaDB |

Para comprobar el nombre del servidor, el usuario activo y la configuración de red se utilizaron los siguientes comandos:

```bash
hostname -f
whoami
ip -4 a
```
<img width="852" height="222" alt="{6C5918A6-AF23-4FCE-98E5-CD6FD851F5CB}" src="https://github.com/user-attachments/assets/7c9f786d-df3b-42c7-82f5-de43c99674d4" />

## 3. Usuario de administración y acceso SSH
Para administrar el servidor `web-sftp` se utilizó el usuario específico `adminitb`, evitando trabajar directamente con el usuario por defecto de la instancia.

Esto cumple con el requisito del proyecto de administrar las máquinas con un usuario propio y acceder mediante clave pública/privada.

El usuario `adminitb` se configuró con permisos administrativos mediante `sudo`, permitiendo realizar tareas de mantenimiento, instalación y configuración del servidor.

La conexión al servidor se realiza mediante SSH:

```bash
ssh -i Taylor.pem adminitb@52.1.67.249
```

Una vez dentro del servidor, se comprobó el usuario activo y el nombre de la máquina:

```bash
whoami
hostname -f
```

También se verificaron los permisos administrativos del usuario:

```bash
sudo -l
```

Para comprobar la configuración de permisos de `sudo`, se revisó el archivo correspondiente:

```bash
sudo cat /etc/sudoers.d/adminitb
```

Además, se comprobó la carpeta `.ssh` del usuario, donde se almacenan las claves autorizadas para el acceso remoto:

```bash
ls -la ~/.ssh
```

Con estas comprobaciones se valida que el servidor se administra mediante el usuario `adminitb`, con acceso SSH por clave y permisos administrativos.

**Evidencia:** captura donde se ve el usuario activo `adminitb`.
<img width="583" height="146" alt="{7771A40B-3E19-4D59-9310-EC58FCFAA163}" src="https://github.com/user-attachments/assets/ce003a10-afd4-41ea-bc23-884c4416d45e" />

## 4. Preparación del servidor
Antes de instalar y configurar los servicios principales, se preparó la máquina `web-sftp` para asegurar que tenía conectividad, nombre correcto, sistema actualizado y resolución de red funcional.

Esta preparación es importante porque el servidor `web-sftp` debe comunicarse con otros servicios internos del proyecto:

- `samba-ad`, para autenticar usuarios del dominio.
- `mariadb`, para consultar y sincronizar datos.
- Servidor multimedia, para redirigir accesos a radio, Jitsi y Jellyfin.

Primero se comprobó el nombre del servidor y la configuración de red:

```bash
hostname -f
ip -4 a
```

Después se verificó la conectividad con los servidores internos principales:

```bash
ping -c 3 10.0.141.9
ping -c 3 10.0.142.205
```

Donde:

| IP | Servicio |
|---|---|
| `10.0.141.9` | Servidor Samba AD |
| `10.0.142.205` | Servidor MariaDB |

También se actualizó la lista de paquetes del sistema:

```bash
sudo apt update
```

Esta preparación permitió dejar el servidor listo para instalar NGINX, PHP, herramientas de integración con Samba AD y el servicio SFTP.

**Evidencia:** captura donde se ve la conectividad hacia Samba AD y MariaDB.
<img width="578" height="321" alt="{0044128D-548C-44D9-AB7E-13D01DEDFF04}" src="https://github.com/user-attachments/assets/cf2ff7cf-af5c-4517-b8e4-cbf9f2d5756f" />

## 5. Instalación de paquetes necesarios
Para que el servidor `web-sftp` pudiera ofrecer el portal web interno, integrarse con Samba AD y permitir acceso SFTP con usuarios del dominio, se instalaron varios paquetes del sistema.

Los paquetes principales utilizados fueron:

| Paquete | Función |
|---|---|
| `nginx` | Servidor web utilizado para publicar el portal interno |
| `php8.3-fpm` | Procesamiento de páginas PHP desde NGINX |
| `php8.3-mysql` | Conexión del portal PHP con MariaDB mediante PDO |
| `php8.3-ldap` | Autenticación del portal contra Samba AD mediante LDAP/TLS |
| `mariadb-client` | Cliente para realizar pruebas de conexión con MariaDB |
| `samba-common-bin` | Herramientas para integración con dominio Samba |
| `winbind` | Resolución de usuarios y grupos del dominio en Linux |
| `libnss-winbind` | Integración de usuarios del dominio con NSS |
| `libpam-winbind` | Integración de autenticación del dominio con PAM |
| `krb5-user` | Herramientas Kerberos para autenticación del dominio |
| `openssh-server` | Servicio SSH/SFTP |
| `curl` | Pruebas HTTP/HTTPS y comprobaciones de servicios |
| `openssl` | Generación y comprobación de certificados SSL |

El comando utilizado para instalar los paquetes principales fue:

```bash
sudo apt update
sudo apt install -y nginx php8.3-fpm php8.3-mysql php8.3-ldap mariadb-client samba-common-bin winbind libnss-winbind libpam-winbind krb5-user openssh-server curl openssl
```

Después de la instalación, se comprobaron las versiones y servicios principales:

```bash
nginx -v
php -v
php -m | grep -E "PDO|pdo_mysql|ldap"
which wbinfo
which net
which sshd
```

También se comprobó el estado de los servicios principales:

```bash
sudo systemctl status nginx --no-pager
sudo systemctl status php8.3-fpm --no-pager
sudo systemctl status ssh --no-pager
sudo systemctl status winbind --no-pager
```

Con estos paquetes instalados, el servidor quedó preparado para:

- Publicar el portal web con NGINX.
- Ejecutar código PHP mediante PHP-FPM.
- Conectar con MariaDB desde PHP.
- Autenticar usuarios contra Samba AD mediante LDAP/TLS.
- Reconocer usuarios del dominio mediante Winbind.
- Permitir acceso SSH/SFTP.

**Evidencia:** captura donde se comprueben las versiones de NGINX/PHP, módulos PHP necesarios y servicios principales activos.

<img width="614" height="317" alt="{78874DD4-A036-4943-BA63-582D0A387167}" src="https://github.com/user-attachments/assets/86ec0ba9-4255-4af5-af54-9c40c8153b9a" />
<img width="777" height="73" alt="{2152506F-3186-4B49-BD89-F167E1C11FE3}" src="https://github.com/user-attachments/assets/d7d3ca1f-a664-4e87-8caf-d88fd2554bfc" />
<img width="826" height="74" alt="{647EA3C0-F688-4F48-8786-547621794E0C}" src="https://github.com/user-attachments/assets/3f834752-846c-46bf-bb0a-ee936703f294" />
<img width="797" height="74" alt="{4EEB6E92-AE48-4C7B-9F72-64DABEF0C293}" src="https://github.com/user-attachments/assets/878db0c5-1796-4bbc-825b-0ce0f9504f72" />

## 6. Configuración de DNS y unión al dominio Samba AD
Para que el servidor `web-sftp` pudiera autenticarse contra Samba AD y reconocer usuarios del dominio, fue necesario integrarlo dentro del dominio `BTIS.INOVATE.CAT`.

Esta integración permite que el servidor `web-sftp` pueda utilizar usuarios centralizados del dominio para servicios como:

- Inicio de sesión en el portal web.
- Acceso SFTP con usuarios corporativos.
- Resolución de usuarios y grupos mediante Winbind.

Antes de unir el servidor al dominio, se comprobó que `web-sftp` pudiera comunicarse con el servidor Samba AD:

```bash
ping -c 3 10.0.141.9
```

También se comprobó la resolución del servidor Samba AD por nombre:

```bash
getent hosts samba-ad.btis.inovate.cat
```

El servidor se integró con el dominio utilizando herramientas de Samba y Kerberos. Una vez unido al dominio, se comprobó el estado de la unión con:

```bash
sudo net ads testjoin
```

El resultado esperado fue:

```text
Join is OK
```

Después se comprobó que `web-sftp` podía listar usuarios y grupos del dominio:

```bash
wbinfo -u | grep -E "joan.garcia|maria.lopez|pere.martinez|anna.puig|admin.itb"
wbinfo -g | grep -E "vendes|administracio|suport|logistica|portal_admins"
```

También se validó que Linux reconocía usuarios concretos del dominio:

```bash
id 'BTIS\joan.garcia'
id 'BTIS\admin.itb'
```

Estas comprobaciones demuestran que el servidor `web-sftp` está correctamente unido al dominio Samba AD y puede utilizar usuarios corporativos en el portal web y en el servicio SFTP.

**Evidencia:** captura donde se comprueba la unión al dominio y la resolución de usuarios del dominio desde `web-sftp`.

<img width="920" height="349" alt="{3F38E3DB-1861-41CD-BC98-7A75BF581184}" src="https://github.com/user-attachments/assets/5842d308-ec3b-4997-b5c6-1fe1f6530c01" />

## 7. Instalación y configuración de NGINX
Para publicar el portal interno de InnovateTech se utilizó NGINX como servidor web.

NGINX se encarga de recibir las peticiones HTTP/HTTPS de los usuarios y servir la aplicación PHP ubicada en el directorio:

```text
/var/www/innovatetech
```

La configuración principal del sitio se guardó en:

```text
/etc/nginx/sites-available/innovatetech
```

Y se habilitó mediante un enlace simbólico en:

```text
/etc/nginx/sites-enabled/
```

El paquete principal instalado fue:

```bash
sudo apt install -y nginx
```

Después se creó el directorio del portal:

```bash
sudo mkdir -p /var/www/innovatetech
```

La configuración del sitio de NGINX incluye:

- Escucha en el puerto `80`.
- Escucha en el puerto `443` con SSL.
- Directorio raíz `/var/www/innovatetech`.
- Uso de `index.php` como página principal.
- Procesamiento PHP mediante PHP-FPM.
- Cabeceras básicas de seguridad.
- Reverse proxy o redirección hacia servicios multimedia.

El bloque principal del sitio se revisó con:

```bash
sudo cat /etc/nginx/sites-available/innovatetech
```

Para comprobar que la sintaxis de NGINX era correcta, se utilizó:

```bash
sudo nginx -t
```

Después de aplicar cambios, se reinició el servicio:

```bash
sudo systemctl restart nginx
```

También se comprobó el estado del servicio:

```bash
sudo systemctl status nginx --no-pager
```

Y los puertos abiertos:

```bash
sudo ss -tulpn | grep -E ':80|:443'
```

Con esta configuración, el servidor `web-sftp` quedó preparado para publicar el portal interno de InnovateTech mediante NGINX.

**Evidencia:** captura donde se compruebe la configuración de NGINX, la validación con `nginx -t`, el servicio activo y los puertos `80` y `443`.

## 8. Configuración HTTPS y redirección HTTP a HTTPS

## 9. Certificado SSL con Subject Alternative Name

## 10. Portal interno InnovateTech

## 11. Autenticación web con Samba AD mediante LDAP/TLS

## 12. Conexión del portal con MariaDB

## 13. Control de acceso por usuario en el portal

## 14. Descarga CSV/SQL desde el portal

## 15. Configuración del servicio SFTP

## 16. Estructura de carpetas SFTP

## 17. Integración SFTP con MariaDB

## 18. Automatización con cron

## 19. Integración con servicios multimedia

## 20. Comprobaciones finales

## 21. Incidencias y soluciones

## 22. Conclusión
