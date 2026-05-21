# Base de Datos — InnovateTech

---

# 1. Contexto

InnovateTech necesita una base de datos centralizada para gestionar:

- empleados,
- departamentos,
- videollamadas,
- streaming multimedia,
- auditoría,
- backups,
- medidas de ancho de banda.

La solución implementada utiliza MariaDB sobre una instancia EC2 privada.

---

# 2. Información general

| Elemento | Valor |
|---|---|
| SGBD | MariaDB |
| Base de datos | innovatetech |
| Servidor | EC2 privada |
| IP privada | 10.0.142.205 |

---

# 3. Instalación de MariaDB

Instalación automatizada mediante Ansible.

Playbook utilizado:

```bash
playbooks/mariadb.yml
```

📸 Captura:
- `capturas/install_mariadb.png`

---

# 4. Creación de la base de datos

Comandos utilizados:

```sql
SHOW DATABASES;
USE innovatetech;
```

📸 Captura:
- `capturas/database_created.png`

---

# 5. Diseño Entidad-Relación

El modelo E/R representa las entidades principales del sistema:

- empleats,
- departaments,
- clients,
- trucades,
- videos,
- auditoría,
- backups.

📸 Captura:
- `capturas/diagrama_er.png`

---

# 6. Modelo relacional

| Tabla | PK | FK |
|---|---|---|
| departaments | codi | - |
| empleats | dni | departament |
| usuaris_comunicacio | id | - |
| trucades | id | origen, destinatari |
| videos | id | - |
| mesures_amplada_banda | id | - |
| avisos | id | - |
| backups_control | id | - |

📸 Captura:
- `capturas/modelo_relacional.png`

---

# 7. Creación de tablas

Archivo SQL utilizado:

```bash
files/sql/schema.sql
```

Comandos de validación:

```sql
SHOW TABLES;
```

📸 Captura:
- `capturas/show_tables.png`

---

# 8. Estructura de tablas

Comandos utilizados:

```sql
DESCRIBE empleats;
DESCRIBE trucades;
SHOW CREATE TABLE trucades;
```

📸 Captura:
- `capturas/describe_tables.png`
- `capturas/foreign_keys.png`

---

# 9. Datos de prueba

Archivo SQL utilizado:

```bash
files/sql/seed.sql
```

Consultas realizadas:

```sql
SELECT * FROM empleats;
SELECT * FROM videos;
SELECT * FROM trucades;
SELECT * FROM clients;
```

📸 Captura:
- `capturas/datos_prueba.png`

---

# 10. Roles y permisos

Roles implementados:

| Rol | Función |
|---|---|
| admin | Acceso total |
| vendes | Gestión comercial |
| administracio | Gestión administrativa |
| treballador | Acceso limitado |

Archivo SQL utilizado:

```bash
files/sql/roles.sql
```

📸 Captura:
- `capturas/roles.png`
- `capturas/grants.png`

---

# 11. Usuario web con permisos mínimos

Usuario creado:

```text
web_innovate
```

Permisos:
- SELECT
- INSERT

Restricción:
- acceso únicamente desde `10.0.1.0/24`

Consulta utilizada:

```sql
SHOW GRANTS FOR 'web_innovate'@'10.0.1.%';
```

📸 Captura:
- `capturas/web_user_grants.png`

---

# 12. Script automatizado de usuarios

Script Python utilizado:

```bash
files/scripts/crear_usuaris.py
```

Este script:
- crea usuarios,
- asigna roles,
- genera automáticamente un fichero `.sql`.

Archivo generado:

```bash
usuarios_generados.sql
```

📸 Captura:
- `capturas/script_python.png`
- `capturas/usuarios_sql.png`

---

# 13. Triggers de auditoría

Triggers implementados:

- control de usuarios bloqueados,
- límite diario de llamadas,
- cuota mensual,
- auditoría automática.

Consultas utilizadas:

```sql
SHOW TRIGGERS;
SELECT * FROM avisos;
```

📸 Captura:
- `capturas/triggers.png`
- `capturas/avisos.png`

---

# 14. Events y backups automáticos

Event implementado:

```text
ev_backup_diari
```

Consultas utilizadas:

```sql
SHOW EVENTS;
SELECT * FROM backups_control;
```

📸 Captura:
- `capturas/events.png`
- `capturas/backups_control.png`

---

# 15. Seguridad aplicada

Medidas implementadas:

- MariaDB escuchando únicamente en IP privada,
- acceso restringido a `10.0.1.0/24`,
- usuario web con mínimos privilegios,
- triggers de auditoría,
- backups automáticos,
- automatización mediante Ansible.

Archivo modificado:

```bash
/etc/mysql/mariadb.conf.d/50-server.cnf
```

📸 Captura:
- `capturas/bind_address.png`

---

# 16. Validación final

Consultas utilizadas:

```sql
SHOW TABLES;
SHOW TRIGGERS;
SHOW EVENTS;
SELECT * FROM backups_control;
```

📸 Captura:
- `capturas/validacion_final.png`

---

# 17. Conclusión

La base de datos de InnovateTech ha sido implementada utilizando MariaDB y automatizada mediante Ansible.

La solución cumple los requisitos de:
- gestión de usuarios,
- roles,
- auditoría,
- seguridad,
- automatización,
- backups,
- monitorización básica.
