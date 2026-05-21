# Infraestructura AWS — InnovateTech CPD

**Responsable:** Bilal El Younossi
**Data:** Maig 2026

---

## 1. Visió general

Infraestructura desplegada a AWS regió `us-east-1` amb VPC dedicada, subnets pública i privada, 7 instàncies EC2 i serveis de xarxa.

---

## 2. VPC i Xarxa

| Paràmetre | Valor |
|-----------|-------|
| Nom | vpc-innovatetech-vpc |
| CIDR | 10.0.0.0/16 |
| Regió | us-east-1 |
| DNS Hostnames | Activat |
| DNS Resolution | Activat |

### Subnets

| Nom | CIDR | Tipus | AZ |
|-----|------|-------|----|
| innovatetech-subPublica | 10.0.1.0/24 | Pública | us-east-1a |
| innovatetech-subPrivada | 10.0.2.0/24 | Privada | us-east-1a |

### Gateways

| Recurs | Nom | Funció |
|--------|-----|--------|
| Internet Gateway | vpc-innovatetech-igw | Connexió subnet pública a internet |
| NAT Gateway | igw-innovatetech | Connexió subnet privada a internet |

---

## 3. Instàncies EC2

| Màquina | IP Pública | IP Privada | Tipus | Subnet | SG |
|---------|-----------|------------|-------|--------|----|
| web-sftp | 52.1.67.249 | 10.0.1.X | t3.small | Pública | SG-WEB |
| multimedia | 32.198.236.17 | 10.0.1.X | t3.small | Pública | SG-MULTIMEDIA |
| jitsi-meet | 3.219.249.6 | 10.0.1.X | t3.small | Pública | SG-JITSI |
| ansible-controller | 32.193.193.146 | 10.0.7.201 | t3.small | Pública | SG-ANSIBLE |
| logs-server | 100.49.230.2 | 10.0.10.91 | t3.small | Pública | SG-LOGS |
| samba-ad | — | 10.0.143.169 | t3.small | Privada | SG-AD |
| mariadb | — | 10.0.142.205 | t3.small | Privada | SG-DB |

### Configuració comuna

- SO: Ubuntu 24.04 LTS
- Disc: 8 GB gp3 (15 GB en multimedia)
- Usuari admin: `adminitb`
- Autenticació: clau pública/privada
- Sudo: sense contrasenya

---

## 4. Security Groups

### SG-WEB
| Direcció | Protocol | Port | Origen |
|----------|----------|------|--------|
| Entrada | TCP | 80 | 0.0.0.0/0 |
| Entrada | TCP | 443 | 0.0.0.0/0 |
| Entrada | TCP | 22 | 0.0.0.0/0 |
| Sortida | TCP | 3306 | 10.0.0.0/16 |
| Sortida | TCP | 389 | 10.0.0.0/16 |
| Sortida | TCP/UDP | 514 | 10.0.0.0/16 |

### SG-MULTIMEDIA
| Direcció | Protocol | Port | Origen |
|----------|----------|------|--------|
| Entrada | TCP | 8096 | 0.0.0.0/0 |
| Entrada | TCP | 8000 | 0.0.0.0/0 |
| Entrada | TCP | 22 | 0.0.0.0/0 |
| Sortida | TCP/UDP | 514 | 10.0.0.0/16 |

### SG-JITSI
| Direcció | Protocol | Port | Origen |
|----------|----------|------|--------|
| Entrada | TCP | 80 | 0.0.0.0/0 |
| Entrada | TCP | 443 | 0.0.0.0/0 |
| Entrada | UDP | 10000 | 0.0.0.0/0 |
| Entrada | TCP | 4443 | 0.0.0.0/0 |
| Entrada | TCP | 22 | 0.0.0.0/0 |
| Sortida | TCP/UDP | 514 | 10.0.0.0/16 |

### SG-AD
| Direcció | Protocol | Port | Origen |
|----------|----------|------|--------|
| Entrada | TCP/UDP | 389 | 10.0.0.0/16 |
| Entrada | TCP | 636 | 10.0.0.0/16 |
| Entrada | TCP | 445 | 10.0.0.0/16 |
| Entrada | TCP/UDP | 88 | 10.0.0.0/16 |
| Entrada | TCP | 22 | 10.0.0.0/16 |
| Sortida | TCP | 80/443 | 0.0.0.0/0 |

### SG-DB
| Direcció | Protocol | Port | Origen |
|----------|----------|------|--------|
| Entrada | TCP | 3306 | 10.0.0.0/16 |
| Entrada | TCP | 22 | 10.0.0.0/16 |
| Sortida | TCP | 80/443 | 0.0.0.0/0 |

### SG-ANSIBLE
| Direcció | Protocol | Port | Origen |
|----------|----------|------|--------|
| Entrada | TCP | 22 | 0.0.0.0/0 |
| Sortida | TCP | 22 | 10.0.0.0/16 |
| Sortida | TCP | 80/443 | 0.0.0.0/0 |

### SG-LOGS
| Direcció | Protocol | Port | Origen |
|----------|----------|------|--------|
| Entrada | TCP | 514 | 10.0.0.0/16 |
| Entrada | UDP | 514 | 10.0.0.0/16 |
| Entrada | ICMP | All | 10.0.0.0/16 |
| Entrada | TCP | 22 | 0.0.0.0/0 |
| Sortida | TCP | 80/443 | 0.0.0.0/0 |

---

## 5. Logs centralitzats

Totes les màquines envien logs a **logs-server** (`10.0.10.91:514`) via rsyslog.

Configuració aplicada a cada màquina:
```
*.* @10.0.10.91:514
```

Logs rebuts verificats a `/var/log/remote/`:
- ansible-controller ✅
- web-sftp ✅
- multimedia ✅
- jitsi-meet ✅
- samba-ad ✅
- mariadb ✅
- logs-server ✅

---

## 6. Procediment de desplegament

```
1.  Crear VPC + subnets + IGW + NAT Gateway + Route Tables
2.  Crear 7 Security Groups
3.  Importar Key Pairs (I.pem, S.pem, T.pem)
4.  Llançar 7 instàncies EC2
5.  Assignar Elastic IPs a màquines públiques
6.  Crear usuari adminitb a totes les màquines
7.  Configurar hostnames
8.  Instal·lar i configurar rsyslog a logs-server
9.  Configurar totes les màquines per enviar logs
10. Verificar connectivitat i logs
```

---

## 7. Captures d'evidència

> Afegir captures de pantalla a `/media/aws/`

- [ ] VPC creada
- [ ] Subnets creades
- [ ] Instàncies en execució
- [ ] Elastic IPs assignades
- [ ] Security Groups configurats
- [ ] Logs rebuts a logs-server
