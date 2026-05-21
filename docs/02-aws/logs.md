# Logs Centralitzats — Rsyslog

**Responsable:** Bilal El Younossi
**Màquina:** logs-server (`100.49.230.2` / `10.0.10.91`)

---

## 1. Descripció

Servidor centralitzat de logs que recull els registres de totes les màquines del projecte. Permet monitoratge centralitzat i diagnosi de problemes.

---

## 2. Instal·lació

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install rsyslog -y
sudo systemctl enable rsyslog
sudo systemctl start rsyslog
```

---

## 3. Configuració del servidor receptor

Editar `/etc/rsyslog.conf` i descomentar:

```bash
module(load="imudp")
input(type="imudp" port="514")

module(load="imtcp")
input(type="imtcp" port="514")
```

Afegir al final per organitzar logs per màquina:

```bash
$template RemoteLogs,"/var/log/remote/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
```

Crear directori i reiniciar:

```bash
sudo mkdir -p /var/log/remote
sudo chown syslog:adm /var/log/remote
sudo systemctl restart rsyslog
```

---

## 4. Configuració dels clients

A cada màquina afegir al final de `/etc/rsyslog.conf`:

```bash
echo "*.* @10.0.10.91:514" | sudo tee -a /etc/rsyslog.conf
sudo systemctl restart rsyslog
```

---

## 5. Verificació

Verificar ports escoltant:
```bash
sudo ss -tulnp | grep 514
```

Verificar logs rebuts:
```bash
ls /var/log/remote/
```

---

## 6. Resultat

Logs rebuts de totes les màquines:

| Màquina | Estat |
|---------|-------|
| ansible-controller | ✅ |
| web-sftp | ✅ |
| multimedia | ✅ |
| jitsi-meet | ✅ |
| samba-ad | ✅ |
| mariadb | ✅ |
| logs-server | ✅ |
