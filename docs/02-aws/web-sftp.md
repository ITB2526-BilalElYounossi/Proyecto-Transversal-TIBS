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

**Evidencia:** captura donde se comprueba la configuración de NGINX, , el servicio activo y los puertos `80` y `443`.

<img width="1206" height="440" alt="{CCDBB08A-85F3-4458-A3B4-540BB655503B}" src="https://github.com/user-attachments/assets/172d167a-cf35-448e-b973-5a838dc03146" />

## 8. Configuración HTTPS y redirección HTTP a HTTPS
Para mejorar la seguridad del portal interno, se configuró NGINX para servir la web mediante HTTPS.

La configuración utiliza dos bloques principales:

- Un bloque en el puerto `80`, encargado de redirigir todo el tráfico HTTP hacia HTTPS.
- Un bloque en el puerto `443`, encargado de servir el portal web con certificado SSL.

La redirección HTTP a HTTPS se configuró en NGINX con la siguiente directiva:

```nginx
server {
    listen 80;
    server_name _;

    return 301 https://$host$request_uri;
}
```

Con esta configuración, cualquier acceso por HTTP se redirige automáticamente a la misma URL utilizando HTTPS.

El bloque HTTPS escucha en el puerto `443`:

```nginx
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate /etc/ssl/certs/innovatetech.crt;
    ssl_certificate_key /etc/ssl/private/innovatetech.key;

    root /var/www/innovatetech;
    index index.php;
}
```

Después de modificar la configuración, se validó la sintaxis de NGINX:

```bash
sudo nginx -t
```

También se comprobó la redirección HTTP a HTTPS con:

```bash
curl -I http://localhost/login.php
```

El resultado esperado fue una respuesta `301 Moved Permanently` hacia HTTPS:

```text
HTTP/1.1 301 Moved Permanently
Location: https://localhost/login.php
```

Finalmente, se comprobó que el portal respondía correctamente por HTTPS:

```bash
curl -k -I https://localhost/login.php
curl -k -I https://52.1.67.249/login.php
```

El resultado esperado fue:

```text
HTTP/1.1 200 OK
```

Con estas pruebas se valida que el servidor redirige correctamente HTTP hacia HTTPS y que el portal interno funciona mediante conexión cifrada.

**Evidencia:** captura donde se ve la redirección HTTP a HTTPS y la respuesta correcta del portal mediante HTTPS.

<img width="571" height="659" alt="{AE578978-675B-4B0C-9F41-522C877A2D03}" src="https://github.com/user-attachments/assets/e4b03814-3410-4975-ba77-c5bd1b9d9b53" />
<img width="551" height="87" alt="{D6E3C6BC-5DFD-4F5A-B6B5-82C0163184A6}" src="https://github.com/user-attachments/assets/47c5d2f3-ce4a-44bd-a46c-ec7bf3b5d143" />
Además, se comprobó visualmente desde el navegador que el portal carga mediante `https://52.1.67.249/login.php`. El navegador muestra el aviso de “No es seguro” porque el certificado utilizado es autofirmado, pero la conexión se realiza mediante HTTPS.

## 9. Certificado SSL con Subject Alternative Name
Para servir el portal mediante HTTPS se generó un certificado SSL autofirmado para el servidor `web-sftp`.

Inicialmente, el certificado funcionaba para cifrar la conexión, pero el navegador mostraba un aviso de seguridad porque el certificado no incluía la extensión `Subject Alternative Name` y no estaba firmado por una autoridad certificadora pública.

Para corregir esta parte, se generó un nuevo certificado incluyendo los nombres alternativos necesarios:

| Tipo | Valor |
|---|---|
| IP Address | `52.1.67.249` |
| DNS | `web-sftp.btis.inovate.cat` |
| DNS | `localhost` |

Primero se creó un archivo de configuración para OpenSSL:

```bash
sudo nano /tmp/innovatetech-openssl.cnf
```

En este archivo se definieron los datos del certificado y los valores de `Subject Alternative Name`.

Después se generó el certificado y la clave privada:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/innovatetech.key \
  -out /etc/ssl/certs/innovatetech.crt \
  -config /tmp/innovatetech-openssl.cnf
```

Posteriormente se ajustaron los permisos de los ficheros SSL:

```bash
sudo chmod 600 /etc/ssl/private/innovatetech.key
sudo chmod 644 /etc/ssl/certs/innovatetech.crt
```

Para comprobar el contenido del certificado se ejecutó:

```bash
openssl x509 -in /etc/ssl/certs/innovatetech.crt -noout -subject -issuer -dates -ext subjectAltName
```

También se comprobó el certificado servido realmente por NGINX:

```bash
echo | openssl s_client -connect 52.1.67.249:443 -servername 52.1.67.249 2>/dev/null | openssl x509 -noout -subject -issuer -dates -ext subjectAltName
```

En la salida se comprobó que el certificado incluía:

```text
IP Address:52.1.67.249
DNS:web-sftp.btis.inovate.cat
DNS:localhost
```

Aunque el navegador sigue mostrando un aviso porque el certificado es autofirmado, la conexión HTTPS funciona correctamente y el certificado contiene los valores SAN necesarios. Para eliminar completamente el aviso del navegador sería necesario utilizar un dominio público y un certificado emitido por una autoridad certificadora reconocida, como Let's Encrypt.

**Evidencia:** captura donde se comprueba que el certificado SSL contiene el `Subject Alternative Name` con la IP pública `52.1.67.249`, el DNS `web-sftp.btis.inovate.cat` y `localhost`.

<img width="1105" height="134" alt="{8875B6AE-47DA-4F22-945A-DE0E62B0130B}" src="https://github.com/user-attachments/assets/0c9f682a-e9f7-4522-a488-4699fb74405c" />


## 10. Portal interno InnovateTech
En el servidor `web-sftp` se desarrolló y desplegó el portal interno de InnovateTech.

Este portal no es una página estática, sino una aplicación web en PHP que permite a los usuarios internos iniciar sesión con sus credenciales del dominio, consultar información almacenada en MariaDB y acceder a los servicios integrados del proyecto.

El portal se encuentra en el directorio:

```text
/var/www/innovatetech
```

Los archivos principales de la aplicación son:

| Archivo | Función |
|---|---|
| `login.php` | Formulario de inicio de sesión y autenticación contra Samba AD |
| `index.php` | Dashboard principal del portal interno |
| `logout.php` | Cierre de sesión del usuario |
| `download.php` | Descarga de tablas permitidas en formato CSV o SQL |

El portal ofrece las siguientes funcionalidades:

- Inicio de sesión con usuarios del dominio Samba AD.
- Visualización de datos reales procedentes de MariaDB.
- Control de acceso por usuario.
- Descarga de información en formato CSV o SQL.
- Acceso a servicios multimedia integrados.
- Interfaz tipo dashboard corporativo.
- Diseño responsive para adaptarse a diferentes tamaños de pantalla.

La estructura principal de la web se comprobó con:

```bash
ls -la /var/www/innovatetech
```

También se validó que los archivos PHP no tuvieran errores de sintaxis:

```bash
php -l /var/www/innovatetech/login.php
php -l /var/www/innovatetech/index.php
php -l /var/www/innovatetech/download.php
php -l /var/www/innovatetech/logout.php
```

El portal se publica mediante NGINX y es accesible desde el navegador mediante la URL:

```text
https://52.1.67.249/login.php
```

Aunque el navegador muestra un aviso porque el certificado es autofirmado, la web se sirve mediante HTTPS y el acceso está cifrado.

**Evidencia:** captura del directorio del portal y validación de sintaxis de los archivos PHP.
<img width="696" height="278" alt="{941F3023-5257-4504-8882-7D3364CF77A0}" src="https://github.com/user-attachments/assets/24771400-80c0-4bce-88d0-e204b2757d18" />

**Evidencia visual:** captura del portal interno cargando correctamente desde el navegador.
<img width="927" height="1176" alt="{578BC624-AD7A-45F5-ABDF-033498426BF9}" src="https://github.com/user-attachments/assets/62ac54df-043c-41fe-912c-c4e00c6f18dd" />



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
