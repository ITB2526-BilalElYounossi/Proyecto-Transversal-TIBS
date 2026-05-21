# Automatización con Ansible

---

# 1. Objetivo

El objetivo de este apartado es automatizar la configuración de la infraestructura de InnovateTech mediante Ansible.

Se utiliza una máquina dedicada como controlador Ansible desde la cual se administran el servidor MariaDB y el servidor de logs.

---

# 2. Arquitectura Ansible

| Máquina | IP | Función |
|---|---|---|
| ansible-controller | 32.193.193.146 | Controlador Ansible |
| mariadb | 10.0.142.205 | Servidor de base de datos |
| logs-server | 100.49.230.2 | Servidor de logs |

---

# 3. Instalación de Ansible

Comandos utilizados:

```bash
sudo apt update
sudo apt install ansible git -y
ansible --version
```

📸 Captura:
- `capturas/ansible_version.png`

---

# 4. Configuración del inventario

Archivo utilizado:

```bash
inventory/inventory.ini
```

Contenido:

```ini

```

📸 Captura:
- `capturas/inventory_ini.png`

---

# 5. Prueba de conectividad

Comando utilizado:

```bash
ansible all -i inventory/inventory.ini -m ping
```

Resultado esperado:
- Todas las máquinas deben responder con `SUCCESS`.

📸 Captura:
- `capturas/ansible_ping.png`

---

# 6. Automatización del servidor MariaDB

Playbook utilizado:

```bash
playbooks/mariadb.yml
```

Este playbook automatiza:

- instalación de MariaDB,
- configuración del servicio,
- creación de la base de datos,
- creación de tablas,
- inserción de datos,
- creación de roles,
- creación de triggers,
- creación de eventos automáticos.

---

# 7. Ejecución del playbook MariaDB

Comando ejecutado:

```bash
ansible-playbook -i inventory/inventory.ini playbooks/mariadb.yml
```

📸 Captura:
- `capturas/playbook_mariadb.png`
- `capturas/play_recap_mariadb.png`

---

# 8. Automatización del servidor de logs

Playbook utilizado:

```bash
playbooks/logs_baseline.yml
```

Este playbook automatiza:

- instalación de herramientas de logs,
- creación de carpetas,
- configuración inicial del servidor.

---

# 9. Ejecución del playbook Logs

Comando ejecutado:

```bash
ansible-playbook -i inventory/inventory.ini playbooks/logs_baseline.yml
```

📸 Captura:
- `capturas/playbook_logs.png`
- `capturas/play_recap_logs.png`

---

# 10. Validación final

Comandos utilizados:

```bash
ansible all -i inventory/inventory.ini -m ping
```

📸 Captura:
- `capturas/validacion_ansible.png`

---

# 11. Conclusión

La infraestructura de InnovateTech ha sido automatizada utilizando Ansible, permitiendo desplegar configuraciones de forma centralizada, reproducible y segura.

La automatización facilita la administración de servidores y reduce errores manuales en la configuración.
