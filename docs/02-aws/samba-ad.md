# Samba AD — Directorio Activo de InnovateTech

## 1. Objetivo del servicio
En esta parte del proyecto se configuró un servidor Samba AD para actuar como servicio de Directorio Activo de la empresa ficticia InnovateTech.

El objetivo principal de este servidor es centralizar la gestión de usuarios y grupos del dominio, de forma que otros servicios del proyecto puedan autenticarse contra el mismo sistema de identidad. En nuestro caso, este servicio se utiliza principalmente para:

- Autenticar usuarios en el portal web interno.
- Permitir acceso SFTP con usuarios del dominio.
- Gestionar grupos corporativos como `vendes`, `administracio`, `suport` y `logistica`.
- Centralizar usuarios internos de InnovateTech.

Este servicio es importante porque evita tener usuarios locales independientes en cada máquina y permite una gestión más parecida a un entorno empresarial real.

## 2. Datos del dominio
La configuración del dominio utilizada en el proyecto es la siguiente:

| Elemento | Valor |
|---|---|
| Dominio DNS | `btis.inovate.cat` |
| Realm Kerberos | `BTIS.INOVATE.CAT` |
| NetBIOS | `BTIS` |
| Servidor | `samba-ad.btis.inovate.cat` |
| IP privada | `10.0.141.9` |
| Servicio principal | Samba AD / Directorio Activo |

El dominio se utiliza como base para autenticar usuarios desde otros servicios del proyecto, especialmente desde el servidor `web-sftp`.

## 3. Preparación del servidor Samba AD
Antes de configurar Samba AD, se preparó la máquina que actuaría como controlador de dominio. Esta máquina forma parte de la infraestructura AWS del proyecto y se encuentra en la red privada.
Se configuró el nombre del servidor para identificarlo correctamente dentro del dominio:
<img width="866" height="228" alt="{DE21759E-89AA-4049-A844-64E038C1EB92}" src="https://github.com/user-attachments/assets/0ce62a2f-c3be-40f1-87f7-3c873f415e9b" />

## 4. Instalación de paquetes necesarios
Para poder utilizar la máquina como controlador de dominio se instalaron los paquetes necesarios relacionados con Samba AD, Kerberos, Winbind y herramientas de comprobación de red.

Esta instalación se realizó en la máquina `samba-ad`, ya que es el servidor encargado de centralizar los usuarios y grupos del dominio.
 Paquete | Función |
|---|---|
| `samba` | Servicio principal para implementar Samba AD como controlador de dominio |
| `krb5-user` | Herramientas de Kerberos para autenticación dentro del dominio |
| `winbind` | Integración de usuarios y grupos del dominio en Linux |
| `smbclient` | Cliente SMB para pruebas y comprobaciones |
| `dnsutils` | Herramientas como `host` para comprobar registros DNS |
| `acl` / `attr` | Gestión de permisos y atributos necesarios en entornos Samba |

Los paquetes principales utilizados fueron:
```bash
sudo apt update
sudo apt install -y samba krb5-user winbind smbclient dnsutils acl attr
```
> **Evidencia:** captura donde se comprueba que Samba, Kerberos y las herramientas de administración del dominio están instaladas correctamente en el servidor `samba-ad`.
<img width="273" height="95" alt="{B9B41FF2-121B-47F7-919E-1742B47670F6}" src="https://github.com/user-attachments/assets/2050b02e-8e3e-4ba3-aad6-d940fbfaa2e2" />

<img width="699" height="214" alt="{776F52C1-8083-4D1A-85AD-37ACF5300D57}" src="https://github.com/user-attachments/assets/2191067c-0811-477d-8ba4-d28f99169542" />


## 5. Provisionado del dominio
Una vez instalados los paquetes necesarios, se configuró el servidor `samba-ad` como controlador de dominio de InnovateTech.

El provisionado del dominio permite crear la estructura principal del Directorio Activo, incluyendo el dominio, el realm Kerberos, el nombre NetBIOS y la configuración inicial de usuarios y servicios internos.

Los datos principales utilizados en el dominio fueron:
| Parámetro | Valor |
|---|---|
| Realm Kerberos | `BTIS.INOVATE.CAT` |
| Dominio DNS | `btis.inovate.cat` |
| NetBIOS | `BTIS` |
| Servidor controlador de dominio | `samba-ad.btis.inovate.cat` |
El comando de provisionado utilizado fue:

```bash
sudo samba-tool domain provision \
  --use-rfc2307 \
  --interactive
```
Durante el proceso interactivo se indicaron los datos del dominio, como el realm, el dominio DNS, el nombre NetBIOS y la contraseña del usuario administrador del dominio.

Este proceso genera la configuración principal de Samba AD y crea la base interna del dominio.

Después del provisionado, se comprobó la configuración general del dominio con:

```bash
sudo samba-tool domain info 127.0.0.1
```

También se revisó el archivo principal de configuración de Samba:

```bash
sudo testparm
```
El comando `testparm` permite validar que la configuración de Samba no contiene errores de sintaxis.

> **Evidencia:** insertar captura donde se vea la información del dominio con `sudo samba-tool domain info 127.0.0.1` y la validación de configuración con `sudo testparm`.
> <img width="413" height="143" alt="{BDCB07D1-B5E5-47EB-ACF3-7C3ED9F75D8B}" src="https://github.com/user-attachments/assets/6904be5b-189d-4d60-aa74-7b48dc4fa36b" /><img width="518" height="663" alt="{ECC328F6-61ED-480A-8615-56BCA6EE820B}" src="https://github.com/user-attachments/assets/44aa2558-9e78-4817-839e-e9545a46ba4e" />

## 6. Configuración de DNS y Kerberos

## 7. Creación de usuarios del dominio

## 8. Creación de grupos del dominio

## 9. Usuario administrativo admin.itb

## 10. Comprobaciones realizadas

## 11. Integración con el servidor web-sftp

## 12. Incidencias y soluciones

## 13. Conclusión
