# Icecast2 - Streaming de Audio

**Máquina:** EC2-3 - Ubuntu 22.04 LTS  
**IP pública:** 32.198.236.17  
**Puerto:** 8000 TCP

---

## 1. Descripción del servicio

Icecast2 es un servidor de streaming de audio de código abierto que permite transmitir audio en tiempo real a múltiples clientes simultáneamente. Actúa como punto central de distribución: recibe el audio de una fuente emisora y lo redistribuye a todos los oyentes conectados.

Soporta los formatos MP3, OGG Vorbis y AAC, y dispone de un panel web integrado accesible desde el navegador para monitorizar el estado del servicio, los canales activos y el número de oyentes en tiempo real.

En este proyecto se utiliza para retransmitir una radio en directo dentro de la infraestructura de InnovateTech mediante la función de relay, accesible tanto desde navegadores web como desde clientes multimedia como VLC.

---

## 2. Requerimientos

- Descripción de la funcionalidad del servicio de audio.
- Instalación y configuración de un servidor de streaming de audio (Icecast2).
- Uso del formato de audio digital MP3.
- Configuración del relay para retransmitir una radio en directo.
- Acceso al servicio via navegador web.
- Documentación de la instalación, configuración y pruebas del servicio.

---

## 3. Instalación

*Servidor: EC2-3 - Ubuntu 22.04 LTS - IP pública: 32.198.236.17 - Puerto 8000 TCP abierto en el Security Group de AWS*

**Paso 1 - Corregir dependencias rotas (necesario en este servidor):**

```bash
sudo dpkg --configure -a
sudo apt --fix-broken install -y
```

**Paso 2 - Actualizar e instalar Icecast2:**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install icecast2 -y
```

Durante la instalación el asistente pregunta:
- Hostname → `32.198.236.17`
- Source password → `pirineus`
- Relay password → `pirineus`
- Admin password → `pirineus`

**Paso 3 - Activar e iniciar el servicio:**

```bash
sudo systemctl enable icecast2
sudo systemctl start icecast2
```

**Paso 4 - Verificar que funciona:**

```bash
sudo systemctl status icecast2
```
<img width="730" height="345" alt="unnamed" src="https://github.com/user-attachments/assets/1414b292-e400-498c-aa8e-32a72929282f" />

---

## 4. Configuración del servicio

El archivo de configuración principal es `/etc/icecast2/icecast.xml`:

```bash
sudo nano /etc/icecast2/icecast.xml
```

Parámetros modificados:

| Parámetro | Valor configurado | Descripción |
|---|---|---|
| `<hostname>` | 32.198.236.17 | IP pública del servidor EC2-3 en AWS |
| `<port>` | 8000 | Puerto de escucha. Abierto en el Security Group de AWS |
| `<source-password>` | pirineus | Contraseña para clientes source |
| `<relay-password>` | pirineus | Contraseña para servidores relay |
| `<admin-user>` | admin | Usuario del panel web de administración |
| `<admin-password>` | pirineus | Contraseña del panel web de administración |

Después de modificar el archivo, reiniciar el servicio:

```bash
sudo systemctl restart icecast2
```

<img width="723" height="200" alt="unnamed" src="https://github.com/user-attachments/assets/cfd360ea-93a3-4aec-81fc-5fdac719ede9" />
<img width="735" height="306" alt="unnamed" src="https://github.com/user-attachments/assets/4c676f75-c7c2-434c-a325-33ae28d91605" />

---

## 5. Configuración del relay de radio

En lugar de emitir audio local, Icecast2 se configura como relay para retransmitir una radio en directo. El relay descarga el stream de Radio France Inter y lo redistribuye a los oyentes conectados a nuestro servidor.

Se añade la siguiente configuración al final del archivo `/etc/icecast2/icecast.xml`, justo antes de `</icecast>`:

```xml
<relay>
    <server>icecast.radiofrance.fr</server>
    <port>80</port>
    <mount>/franceinter/franceinter-midfi.mp3</mount>
    <local-mount>/radio-spain</local-mount>
    <on-demand>0</on-demand>
    <relay-shoutcast-metadata>1</relay-shoutcast-metadata>
</relay>
```

| Parámetro | Valor | Descripción |
|---|---|---|
| `<server>` | icecast.radiofrance.fr | Servidor de origen del stream |
| `<port>` | 80 | Puerto del servidor de origen |
| `<mount>` | /franceinter/franceinter-midfi.mp3 | Mountpoint en el servidor de origen |
| `<local-mount>` | /radio-spain | Mountpoint local en nuestro servidor |
| `<on-demand>` | 0 | El relay arranca siempre, sin esperar oyentes |
| `<relay-shoutcast-metadata>` | 1 | Retransmite los metadatos del stream original |

Después de añadir el relay, reiniciar el servicio:

```bash
sudo systemctl restart icecast2
```

El relay se conecta automáticamente al servidor de origen y empieza a redistribuir el audio sin necesidad de ningún proceso adicional.

---

## 6. Acceso via navegador web

Una vez configurado el relay, el servicio es accesible desde cualquier navegador:

- **Panel de administración:** http://32.198.236.17:8000
- **URL directa del stream:** http://32.198.236.17:8000/radio-spain
- **Estado del servicio:** http://32.198.236.17:8000/status.xsl

<img width="1852" height="973" alt="unnamed" src="https://github.com/user-attachments/assets/400b420b-7aca-488b-9778-f136ed7c467b" />

---

## 7. Formatos de audio utilizados

| Formato | Descripción técnica | Uso en el proyecto |
|---|---|---|
| **MP3** | MPEG-1 Audio Layer III. Compresión con pérdida. Bitrate: 128 kbps. Compatible con todos los navegadores sin plugins. | Formato del stream relay. Radio France Inter emite en MP3 y Icecast2 lo redistribuye en el mismo formato. |
| **OGG Vorbis** | Formato libre y abierto. Mejor calidad que MP3 al mismo bitrate. | Soportado por Icecast2. Alternativa libre al MP3. |
| **AAC** | Advanced Audio Coding. Sucesor del MP3 con mejor calidad a menor bitrate. | Soportado por Icecast2. Usado en streaming móvil. |

---

## 8. Incidencias y soluciones

| Incidencia | Solución aplicada |
|---|---|
| `E: Unable to locate package icecast2` | Ejecutar `sudo apt update` antes de instalar |
| `E: dpkg was interrupted` | Ejecutar `sudo dpkg --configure -a` y `sudo apt --fix-broken install -y` |
| `ERR_CONNECTION_TIMED_OUT` al acceder al puerto 8000 | Puerto 8000 no estaba abierto en el Security Group de AWS. Se abrió en Inbound Rules. |
| El relay de Los 40 no conecta | El servidor de Los 40 no permite relay externo. Se cambió a Radio France Inter que sí lo permite. |
| Mountpoint `/stream` en uso al configurar relay | ffmpeg y el relay competían por el mismo mountpoint. Se asignó `/radio-spain` al relay para evitar conflictos. |
| `Mountpoint /stream in use` en los logs | El servicio systemd de ffmpeg seguía reiniciándose. Se eliminó el servicio y se asignó un mountpoint diferente al relay. |

---

## 9. Security Group - SG-MULTIMEDIA

| Dirección | Protocolo | Puerto | Origen |
|---|---|---|---|
| Entrada | TCP | 8000 | 0.0.0.0/0 |
| Entrada | TCP | 22 | 0.0.0.0/0 |
| Salida | TCP/UDP | 514 | 10.0.0.0/16 |
