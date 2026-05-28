# Logs Centralitzats — Rsyslog + EFS

**Responsable:** Bilal El Younossi  
**Màquina:** logs-server-private (`10.0.133.107`) — Subnet privada  
**Data:** Maig 2026

---

## 1. Descripció

Sistema de logs centralitzat basat en **rsyslog** amb emmagatzematge compartit a **AWS EFS**. Totes les instàncies EC2 envien els seus logs al servidor `logs-server-private` per TCP/UDP port 514. El servidor emmagatzema els logs classificats per hostname i programa al sistema de fitxers EFS (`efs-innovatetech-logs`), accessible simultàniament des de `web-sftp` per a la seva visualització al portal web.

### Arquitectura del sistema

```
EC2 (totes) ──rsyslog:514──► logs-server-private ──escriu──► EFS /mnt/efs-logs
                                                                      │
web-sftp ──llegeix──────────────────────────────────────────────────►┘
Portal web mostra logs en temps real (admin.itb)
```

---

## 2. Servidor de logs — logs-server-private

### Instal·lació

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install rsyslog -y
sudo systemctl enable rsyslog
sudo systemctl start rsyslog
```

### Muntatge EFS

```bash
sudo apt install nfs-common -y
sudo mkdir -p /mnt/efs-logs
sudo mount -t nfs4 -o nfsvers=4.1 10.0.142.148:/ /mnt/efs-logs

# Muntatge permanent al fstab
echo "10.0.142.148:/ /mnt/efs-logs nfs4 nfsvers=4.1,_netdev 0 0" | sudo tee -a /etc/fstab
```

### Configuració rsyslog receptor

Editar `/etc/rsyslog.conf` i habilitar recepció TCP/UDP:

```bash
module(load="imudp")
input(type="imudp" port="514")
module(load="imtcp")
input(type="imtcp" port="514")
```

Afegir template per organitzar logs per màquina al EFS:

```bash
$template RemoteLogs,"/mnt/efs-logs/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
```

### Permisos i AppArmor

```bash
# Permisos al directori EFS
sudo chmod -R 777 /mnt/efs-logs/

# Desactivar AppArmor per rsyslog (necessari per escriure al EFS)
sudo apt install apparmor-utils -y
sudo aa-disable /usr/sbin/rsyslogd

# Reiniciar rsyslog
sudo systemctl restart rsyslog
```

### Logrotate — rotació automàtica de logs

Configuració a `/etc/logrotate.d/efs-logs`:

```
/mnt/efs-logs/*/*.log {
    su root root
    daily
    rotate 7
    compress
    missingok
    notifempty
    size 50M
    copytruncate
}
```

---

## 3. Configuració dels clients (totes les EC2)

A cada màquina crear `/etc/rsyslog.d/99-remote.conf`:

```bash
echo "*.* @@10.0.133.107:514" | sudo tee /etc/rsyslog.d/99-remote.conf
sudo systemctl restart rsyslog
```

> `@@` indica TCP (més fiable que UDP `@`)

---

## 4. web-sftp — Accés al EFS per al portal web

```bash
sudo apt install nfs-common -y
sudo mkdir -p /mnt/efs-logs
sudo mount -t nfs4 -o nfsvers=4.1 10.0.50.145:/ /mnt/efs-logs

# Permisos per www-data
sudo chmod -R 755 /mnt/efs-logs/

# Muntatge permanent
echo "10.0.50.145:/ /mnt/efs-logs nfs4 nfsvers=4.1,_netdev 0 0" | sudo tee -a /etc/fstab
```

---

## 5. Verificació

### Ports escoltant al servidor de logs

```bash
$ sudo ss -tlnup | grep 514
udp   UNCONN  0.0.0.0:514   users:(("rsyslogd"))
tcp   LISTEN  0.0.0.0:514   users:(("rsyslogd"))
```

### EFS muntat correctament

```bash
$ df -h | grep mnt
10.0.142.148:/  8.0E  623M  8.0E  1%  /mnt/efs-logs   # logs-server-private
10.0.50.145:/   8.0E    0   8.0E  0%  /mnt/efs-logs   # web-sftp
```

### Logs rebuts de totes les màquines

```bash
$ ls /mnt/efs-logs/
ansible-controller  jitsi-meet  logs-server-private  mariadb  multimedia  samba-ad  web-sftp
```

---

## 6. Estat del sistema

| Màquina | IP | Logs al EFS | Estat |
|---------|----|-------------|-------|
| web-sftp | 10.0.5.140 | ✅ | Actiu |
| multimedia | 10.0.8.36 | ✅ | Actiu |
| jitsi-meet | 10.0.14.189 | ✅ | Actiu |
| ansible-controller | 10.0.7.201 | ✅ | Actiu |
| samba-ad | 10.0.141.9 | ✅ | Actiu |
| mariadb | 10.0.142.205 | ✅ | Actiu |
| logs-server-private | 10.0.133.107 | ✅ | Actiu |

---

## 7. Dashboard de logs al portal web

El portal web (`admin.itb`) mostra els logs en temps real amb les funcionalitats següents:

- Visualització dels últims 8 registres per fitxer i servidor
- **Filtres** per tipus: Tots / Errors / Accesos OK / Warnings / SSH / Sudo
- **Buscador** en temps real per tots els servidors
- Conversió automàtica de timestamps UTC → Europe/Madrid
- Color vermell per errors, verd per accesos correctes, groc per warnings

### Accés

1. Entrar al portal web amb `admin.itb`
2. Desplaçar-se fins a la secció **Logs del sistema** al final del dashboard

---

## 8. Evidències

### Logs rebuts al EFS
<img width="912" height="618" alt="imatge" src="https://github.com/user-attachments/assets/5a76e6b0-5c09-4b3c-84be-145179c9eb4e" />


### Dashboard de logs al portal web
<img width="1828" height="934" alt="imatge" src="https://github.com/user-attachments/assets/3396af65-9bfa-4efe-9b3c-40de95cab54d" />


### rsyslog escoltant al port 514
<img width="926" height="117" alt="imatge" src="https://github.com/user-attachments/assets/35746984-d894-4f71-8160-516ee0434324" />
