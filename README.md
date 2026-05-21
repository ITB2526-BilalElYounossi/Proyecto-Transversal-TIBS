# Projecte Transversal ASIXc1 — InnovateTech CPD

> Disseny i implementació d'un Centre de Processament de Dades (CPD) al núvol AWS per a l'empresa **InnovateTech**, incloent serveis multimèdia, base de dades, directori actiu i gestió centralitzada de logs.

**Grup:** TIBS (Taylor · Izan · Bilal · Serghei)
**Cicle:** CFGS Administració de Sistemes Informàtics en Xarxa
**Centre:** Institut Tecnològic de Barcelona
**Curs:** 2025–2026
**Lliurament:** 28 de maig de 2026
**Defensa:** 29 de maig de 2026

---

## Descripció del projecte

InnovateTech és una empresa dedicada a la provisió de serveis tecnològics que necessita modernitzar la seva capacitat operativa. Aquest projecte dissenya i implementa un CPD eficient al núvol AWS que integra:

- Distribució de continguts d'àudio i vídeo en streaming
- Sistema de videoconferències per a comunicació interna
- Base de dades per gestionar personal, comunicacions i activitat
- Directori Actiu per centralitzar la gestió d'usuaris
- Servidor web (intranet) autenticat contra el Directori Actiu
- Gestió centralitzada de logs de tots els servidors
- Configuració automatitzada amb Ansible

---

## Arquitectura AWS

```
Internet
    │
    ▼
[Internet Gateway — igw-innovatetech]
    │
    ▼
[Application Load Balancer]
    │
┌───────────────────────────────────────────────────────┐
│  VPC: vpc-innovatetech-vpc  (10.0.0.0/16)             │
│                                                       │
│  ┌─── Subnet Pública (10.0.1.0/24) ───────────────┐  │
│  │  web-sftp          52.1.67.249   10.0.1.X       │  │
│  │  multimedia        32.198.236.17 10.0.1.X       │  │
│  │  jitsi-meet        3.219.249.6   10.0.1.X       │  │
│  │  ansible-ctrl      32.193.193.146 10.0.7.201    │  │
│  │  logs-server       100.49.230.2  10.0.10.91     │  │
│  └─────────────────────────────────────────────────┘  │
│                                                       │
│  ┌─── Subnet Privada (10.0.2.0/24) ───────────────┐  │
│  │  samba-ad          —             10.0.143.169   │  │
│  │  mariadb           —             10.0.142.205   │  │
│  └─────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────┘
```

---

## Equip i responsabilitats

| Membre | Rol | Màquines |
|--------|-----|----------|
| **Bilal** | Infraestructura AWS + Git + Logs | logs-server |
| **Izan** | Base de dades + Ansible | mariadb + ansible-controller |
| **Taylor** | Web + SFTP + Directori Actiu | web-sftp + samba-ad |
| **Serghei** | Multimèdia + Videoconferències | multimedia + jitsi-meet |

---

## Màquines i serveis

| Màquina | IP Pública | IP Privada | Serveis | Subnet |
|---------|-----------|------------|---------|--------|
| web-sftp | 52.1.67.249 | 10.0.1.X | NGINX + SFTP | Pública |
| multimedia | 32.198.236.17 | 10.0.1.X | Jellyfin + Icecast2 | Pública |
| jitsi-meet | 3.219.249.6 | 10.0.1.X | Jitsi Meet | Pública |
| ansible-controller | 32.193.193.146 | 10.0.7.201 | Ansible | Pública |
| logs-server | 100.49.230.2 | 10.0.10.91 | Rsyslog | Pública |
| samba-ad | — | 10.0.143.169 | Samba AD | Privada |
| mariadb | — | 10.0.142.205 | MariaDB | Privada |

---

## Stack tecnològic

| Component | Tecnologia | Funció |
|-----------|-----------|--------|
| Cloud | AWS (VPC, EC2, ALB, S3, EFS) | Infraestructura |
| SO | Ubuntu 24.04 LTS | Totes les instàncies |
| Directori Actiu | Samba AD | Gestió d'usuaris |
| Servidor web | NGINX + PHP | Intranet InnovateTech |
| Transferència fitxers | SFTP + AD auth | Accés fitxers per usuaris |
| Vídeo streaming | Jellyfin | Distribució vídeo |
| Àudio streaming | Icecast2 | Distribució àudio |
| Videoconferències | Jitsi Meet | Comunicació interna |
| Base de dades | MariaDB | Gestió dades empresa |
| Automatització | Ansible | Configuració servidors |
| Logs centralitzats | Rsyslog | Monitoratge |
| Control de versions | Git + GitHub | Documentació |

---

## Seguretat

| Capa | Mesures aplicades |
|------|------------------|
| Xarxa | Subnet privada per BD i AD, Security Groups restrictius |
| Accés | Clau pública/privada, usuari adminitb dedicat, NOPASSWD sudo |
| Web | HTTPS, cabeceres seguretat (X-Frame-Options, X-XSS-Protection) |
| BD | Rols i permisos diferenciats, prepared statements, triggers auditoria |
| Logs | Centralització rsyslog, retenció per servei |

---

## Estructura del repositori

```
Proyecto-Transversal-TIBS/
├── README.md
├── architecture/              → Diagrames de xarxa
├── docs/
│   ├── 01-cpd/                → Proposta CPD físic
│   ├── 02-aws/                → Infraestructura AWS
│   ├── 03-audio-video/        → Serveis multimèdia
│   └── 04-base-dades/         → Base de dades
├── infrastructure/
│   ├── aws/                   → Configuració VPC, EC2, SG
│   ├── ansible/               → Playbooks
│   └── logs/                  → Configuració rsyslog
├── media/                     → Captures de pantalla evidències
├── scripts/                   → Scripts de configuració
└── tests/                     → Proves de funcionament
```

---

## Índex de documentació

| Document | Descripció |
|----------|-----------|
| [Proposta CPD](docs/01-cpd/cpd.md) | Ubicació física, racks, SAI, seguretat |
| [Infraestructura AWS](docs/02-aws/arquitectura.md) | VPC, subnets, instàncies, SG |
| [Samba AD](docs/02-aws/samba-ad.md) | Directori Actiu, usuaris, grups |
| [Web + SFTP](docs/02-aws/web-sftp.md) | NGINX, PHP, SFTP integrat AD |
| [Ansible](docs/02-aws/ansible.md) | Playbooks, inventari |
| [Logs](docs/02-aws/logs.md) | Rsyslog centralitzat |
| [Jellyfin + Icecast2](docs/03-audio-video/multimedia.md) | Streaming àudio i vídeo |
| [Jitsi Meet](docs/03-audio-video/jitsi.md) | Videoconferències |
| [Base de dades](docs/04-base-dades/disseny-er.md) | Disseny E/R, model relacional |
| [Usuaris i rols](docs/04-base-dades/usuaris-rols.md) | Rols, permisos, scripts |
| [Triggers i events](docs/04-base-dades/triggers-events.md) | Auditoria, backups automàtics |

---

## Proves de funcionament

| Prova | Resultat |
|-------|---------|
| Logs centralitzats (7 màquines) | ✅ |
| SSH amb clau pública a totes les màquines | ✅ |
| Connectivitat entre subnets | ✅ |
| NAT Gateway subnet privada | ✅ |
| Samba AD domini BTIS.INOVATE.CAT | 🔄 En curs |
| SFTP autenticat contra AD | 🔄 En curs |
| NGINX HTTPS + cabeceres seguretat | 🔄 En curs |
| Jellyfin streaming vídeo | 🔄 En curs |
| Icecast2 streaming àudio | 🔄 En curs |
| Jitsi Meet videoconferència | 🔄 En curs |
| MariaDB rols i permisos | 🔄 En curs |
| Ansible playbooks | 🔄 En curs |
