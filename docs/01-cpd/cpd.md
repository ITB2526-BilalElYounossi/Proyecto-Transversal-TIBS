# Proposta de CPD — InnovateTech

**Responsable:** Bilal El Younossi  
**Data:** Maig 2026

---

## 1. Ubicació física

### 1.1 Situació de la sala a l'edifici

El CPD d'InnovateTech es troba a la **primera planta interior** de l'edifici corporatiu, orientada cap al pati interior, sense façana exterior i allunyada de zones de pas públic.

#### Justificació de la ubicació

La primera planta interior ha estat escollida després d'avaluar les alternatives:

| Ubicació | Risc inundació | Risc accés | Temperatura | Decisió |
|----------|---------------|------------|-------------|---------|
| Soterrani | ❌ Alt | ✅ Baix | ✅ Estable | **Descartat** |
| Planta baixa | ⚠️ Mig | ❌ Alt | ⚠️ Variable | **Descartat** |
| **1a planta interior** | ✅ Nul | ✅ Baix | ✅ Estable | **✅ Seleccionat** |
| Última planta | ✅ Nul | ✅ Baix | ❌ Calor | **Descartat** |

**Avantatges de la primera planta interior:**

- **Risc d'inundació nul**: elevat sobre el nivell del carrer, protegit de filtracions d'aigua.
- **Seguretat**: difícil accés des de l'exterior, sense finestres cap al carrer.
- **Temperatura estable**: l'orientació interior evita la incidència solar directa.
- **Accés per manteniment**: accessible sense ascensor, facilita el transport d'equips pesats.
- **Estructura de l'edifici**: les plantes intermèdies suporten millor el pes dels racks (fins a 1.200 kg/m²).

La sala ocupa **50 m²** amb dimensions de 10m x 5m i alçada lliure de 3m (2,5m útils amb terra tècnic i sostre tècnic).

```
┌─────────────────────────────────────────────┐
│            PRIMERA PLANTA                   │
│                                             │
│  Oficines   ┌──────────────┐   Oficines     │
│             │  SALA CPD    │                │
│             │  (50m²)      │ ← Pati interior│
│             │  Rack1 Rack2 │                │
│             │  [A/C] [A/C] │                │
│             └──────────────┘                │
│                   ↑                         │
│            Accés controlat                  │
│            (porta blindada)                 │
└─────────────────────────────────────────────┘
```

> 📸 Afegir plànol de l'edifici amb la ubicació del CPD marcada.

---

### 1.2 Climatització

El sistema de climatització és crític per mantenir els equips en condicions òptimes de funcionament.

#### Paràmetres ambientals objectiu

| Paràmetre | Valor òptim | Rang acceptable |
|-----------|-------------|-----------------|
| Temperatura | 21°C | 18°C – 27°C |
| Humitat relativa | 45% | 40% – 60% |
| Partícules en suspensió | < 100.000 p/m³ | — |

#### Sistema instal·lat

- **2 unitats de climatització de precisió** marca APC InRow RC (9 kW cadascuna) en configuració N+1 (una de redundant).
- **Sistema de flux d'aire** de baix a dalt: aire fred entra pel terra tècnic i l'aire calent surt pel sostre tècnic cap als conductes d'extracció.
- **Configuració de corredor fred/calent**: els racks estan orientats cara a cara per separar els corredors d'aire fred (davant dels racks) i calent (darrere dels racks).
- **Sensors ambientals** en 3 punts de la sala: entrada, centre i sortida.
- **Sistema de filtratge HEPA** per eliminar partícules i pols.

```
        CORREDOR FRED          CORREDOR CALENT
        ↓ Aire fred ↓          ↑ Aire calent ↑
   ┌──────────────────────────────────────────┐
   │  [Rack 1] →→→ | ←←← [Rack 2]            │
   │  FRONT          FRONT                    │
   │  ↑ Aire fred ↑  ↑ Aire fred ↑            │
   │                                          │
   │  [A/C 1]              [A/C 2 redundant]  │
   └──────────────────────────────────────────┘
         ↑ Terra tècnic (aire fred)
```

> 📸 Afegir fotografies del sistema de climatització instal·lat.

---

### 1.3 Mesures per dificultar la identificació de la sala

- **Senyalització neutra**: la porta no indica "CPD" ni "Sala de servidors". Apareix identificada com "Manteniment Tècnic B-03".
- **Sense finestres** cap a l'exterior ni cap al passadís principal.
- **Porta blindada** sense maneta exterior, amb obertura únicament per targeta RFID.
- **Cable i alimentació soterrats**: no hi ha cablejat visible als passadissos que pugui indicar la ubicació.
- **Murs reforçats**: les parets de la sala son de formigó armat de 20cm, sense indicació visual de la seva funció.

---

### 1.4 Distribució i gestió del cablejat

#### Principis de gestió del cablejat

- **Separació física** de cablejat de xarxa (blau) i cablejat elèctric (vermell/negre) per evitar interferències.
- **Etiquetatge** de tots els cables als dos extrems amb codi alfanumèric.
- **Brides i canals** per mantenir el cablejat ordenat i accessible.
- **Color coding**:
  - Blau: xarxa de dades (LAN)
  - Verd: connexions entre switches
  - Groc: connexions de gestió (IPMI/iLO)
  - Vermell: alimentació elèctrica
  - Negre: alimentació SAI

#### Distribució

```
TERRA TÈCNIC
├── Canal 1 (costat esquerre): Cablejat de xarxa
│   ├── Patch cables Cat6A (blau)
│   └── Fibra òptica (groc)
├── Canal 2 (costat dret): Alimentació elèctrica
│   ├── Cables PDU (vermell)
│   └── Cables SAI (negre)
└── Canal central: Connexions entre racks
```

> 📸 Afegir fotografies del cablejat organitzat als racks.

---

### 1.5 Terra tècnic i sostre tècnic

#### Terra tècnic

- **Alçada**: 40 cm sobre el terra estructural.
- **Plaques**: 60x60 cm d'acer galvanitzat amb acabat antistàtic.
- **Funció**: distribució d'aire fred des dels climatitzadors cap a la part frontal dels racks, i canalització del cablejat.
- **Càrrega màxima**: 1.200 kg/m².

#### Sostre tècnic

- **Alçada**: 30 cm entre fals sostre i sostre estructural.
- **Funció**: retorn d'aire calent als climatitzadors i pas de les instal·lacions (elèctrica, detecció d'incendis, cablejat de gestió).
- **Plaques desmuntables** per facilitar el manteniment.

```
┌─────────────────────────────────┐ ← Sostre estructural (3m)
│    Conductes retorn aire calent  │
│    Cables gestió                 │
├─────────────────────────────────┤ ← Fals sostre (2.7m)
│                                 │
│          ZONA RACKS              │  2.5m útils
│                                 │
├─────────────────────────────────┤ ← Terra tècnic (0.4m)
│    Cables xarxa / elèctrics     │
│    Distribució aire fred        │
└─────────────────────────────────┘ ← Terra estructural (0m)
```

---

### 1.6 Plànol de la sala CPD

```
         10 metres
    ┌──────────────────────────────┐
    │  [A/C 1]         [A/C 2]    │ 5m
    │                              │
    │  ┌────────┐   ┌────────┐    │
    │  │ RACK 1 │   │ RACK 2 │    │
    │  │        │   │        │    │
    │  │ 42U    │   │ 42U    │    │
    │  └────────┘   └────────┘    │
    │                              │
    │  [SAI 1]         [SAI 2]    │
    │                              │
    │              ← PORTA →      │
    └──────────────────────────────┘
    [Panel elèctric]  [Extintors]
```

> 📸 Afegir plànol CAD o dibuix tècnic de la sala.

---

### 1.7 Estructuració dels racks

#### Rack 1 — Servidors i xarxa

| U | Equip |
|---|-------|
| 1-2 | Patch panel Cat6A (48 ports) |
| 3 | Switch core (Cisco Catalyst 9300) |
| 4 | Switch accés (Cisco Catalyst 2960) |
| 5 | Firewall (Fortinet FortiGate 100F) |
| 6-8 | Servidor web-sftp (Dell PowerEdge R350) |
| 9-11 | Servidor multimedia (Dell PowerEdge R350) |
| 12-14 | Servidor Jitsi (Dell PowerEdge R350) |
| 15-17 | Servidor Ansible (Dell PowerEdge R350) |
| 18-20 | Servidor Samba AD (Dell PowerEdge R350) |
| 40-42 | PDU vertical (APC AP8959) |

#### Rack 2 — Bases de dades, logs i emmagatzematge

| U | Equip |
|---|-------|
| 1-2 | Patch panel Cat6A (48 ports) |
| 3 | Switch gestió (Cisco Catalyst 2960) |
| 4-6 | Servidor MariaDB (Dell PowerEdge R350) |
| 7-9 | Servidor logs (Dell PowerEdge R350) |
| 10-14 | NAS (Synology RS3621xs+, 12 bahies) |
| 15-16 | Servidor de còpies de seguretat |
| 40-42 | PDU vertical (APC AP8959) |

```
RACK 1                    RACK 2
┌──────────┐              ┌──────────┐
│ Patch    │              │ Patch    │ 1U
│ Panel    │              │ Panel    │ 2U
├──────────┤              ├──────────┤
│ Switch   │              │ Switch   │ 3U
│ Core     │              │ Gestió   │ 4U
├──────────┤              ├──────────┤
│ Firewall │              │ MariaDB  │ 5-7U
├──────────┤              ├──────────┤
│ web-sftp │              │ Logs     │ 8-10U
├──────────┤              ├──────────┤
│Multimedia│              │ NAS      │ 11-15U
├──────────┤              ├──────────┤
│  Jitsi   │              │ Backup   │ 16-17U
├──────────┤              ├──────────┤
│ Ansible  │              │          │
├──────────┤              │  (lliure)│
│ Samba AD │              │          │
├──────────┤              ├──────────┤
│   PDU    │              │   PDU    │ 40-42U
└──────────┘              └──────────┘
```

> 📸 Afegir fotografies dels racks amb els equips instal·lats.

---

## 2. Infraestructura IT

### 2.1 Servidors

| Servidor | Model | CPU | RAM | Disc | Funció |
|----------|-------|-----|-----|------|--------|
| web-sftp | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Web + SFTP |
| multimedia | Dell PowerEdge R350 | Intel Xeon E-2334 | 32 GB | 2x960GB SSD RAID1 | Icecast + Jellyfin |
| jitsi-meet | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Videoconferència |
| ansible | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Automatització |
| samba-ad | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Directori actiu |
| mariadb | Dell PowerEdge R650 | Intel Xeon Silver 4310 | 64 GB | 4x960GB SSD RAID10 | Base de dades |
| logs-server | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Logs centralitzats |

### 2.2 Patch panels

- **2x Patch panel Cat6A de 48 ports** (un per rack)
- Connexió entre els ports del patch panel i els ports dels switches mitjançant patch cables curts (0,5m) per minimitzar el cablejat dins el rack.
- Etiquetatge de cada port amb la màquina o dispositiu connectat.

### 2.3 Switches

| Switch | Model | Ports | Funció |
|--------|-------|-------|--------|
| Switch core | Cisco Catalyst 9300-24T | 24x1G + 4x10G uplink | Interconnexió entre racks i uplink internet |
| Switch accés 1 | Cisco Catalyst 2960-24TC | 24x1G | Connexió servidors Rack 1 |
| Switch accés 2 | Cisco Catalyst 2960-24TC | 24x1G | Connexió servidors Rack 2 |
| Switch gestió | Cisco Catalyst 2960-8TC | 8x1G | Gestió IPMI/iLO dels servidors |

---

## 3. Infraestructura elèctrica

### 3.1 Alimentació redundant

- **2 línies elèctriques independents** des del quadre general de l'edifici.
- Cada rack disposa de **2 PDUs** (Power Distribution Units) connectades a línies elèctriques diferents.
- Tots els servidors tenen **2 fonts d'alimentació** redundants (PSU1 → Línia A, PSU2 → Línia B).

```
Xarxa elèctrica
      │
  ┌───┴───┐
  │  UPS  │  (APC Symmetra LX 16 kVA)
  └───┬───┘
      │
  ┌───┴─────────┐
  │             │
Línia A      Línia B
  │             │
PDU Rack1A  PDU Rack1B    PDU Rack2A  PDU Rack2B
  │             │             │           │
PSU1        PSU2          PSU1        PSU2
(tots els servidors)
```

### 3.2 SAI — Sistemes d'Alimentació Ininterrompuda

#### Model seleccionat

**APC Symmetra LX 16 kVA** amb bateries externes.

#### Càlcul de consum

| Equip | Consum estimat |
|-------|---------------|
| 7 servidors Dell PowerEdge R350 | 7 × 350W = 2.450W |
| 2 switches Cisco Catalyst | 2 × 370W = 740W |
| 1 switch core Cisco 9300 | 1 × 715W = 715W |
| 2 climatitzadors APC InRow | 2 × 200W (ventiladors) = 400W |
| 1 NAS Synology | 1 × 100W = 100W |
| **TOTAL** | **4.405W** |
| **Marge de seguretat (+20%)** | **5.286W ≈ 5,3 kW** |

#### Autonomia

- **SAI APC Symmetra LX 16 kVA** amb 2 mòduls de bateries externs (SYBATT).
- Cada mòdul proporciona ~15 minuts a plena càrrega.
- Amb 2 mòduls: **~30 minuts d'autonomia** a plena càrrega.
- Temps suficient per a un apagat ordenat de tots els servidors o per a que el grup electrogen entri en funcionament.

> **Justificació**: 30 minuts és el temps estàndard recomanat per a CPDs de mida mitjana, suficient per a engegar un generador de backup o fer un apagat controlat.

---

## 4. Seguretat física

### 4.1 Control d'accés

- **Porta blindada** amb tancament electrònic i obertura per doble factor:
  1. Targeta RFID (HID iClass)
  2. PIN de 6 dígits
- **Registre d'accessos**: tot accés queda registrat amb data, hora i identitat.
- **Llista blanca**: únicament personal autoritzat explícitament pot accedir.
- **Accés d'emergència**: clau física en caixa forta al despatx del responsable IT.
- **Alarma**: activació automàtica si la porta s'obre sense autenticació o queda oberta més de 60 segons.

### 4.2 Videovigilància

- **4 càmeres IP** de 4K (Axis P3245-V) distribuïdes:
  - 1 a l'entrada de la sala
  - 1 al corredor fred (davant dels racks)
  - 1 al corredor calent (darrere dels racks)
  - 1 a la zona del quadre elèctric i SAI
- **Enregistrament**: 24/7 amb retenció de 30 dies en NAS dedicat.
- **Alertes**: detecció de moviment amb notificació per correu electrònic.
- **Accés remot**: visualització en temps real des del sistema de gestió IT.

```
┌──────────────────────────────┐
│  📷 Càmera entrada           │
│                              │
│  ┌────────┐   ┌────────┐    │
│  │📷 Rack1│   │ Rack 2 │📷  │
│  └────────┘   └────────┘    │
│                              │
│         📷 SAI/Elèctric      │
└──────────────────────────────┘
```

> 📸 Afegir fotografies de les càmeres instal·lades.

### 4.3 Sistemes de prevenció i extinció d'incendis

- **Detectors de fum** iònics i òptics distribuïts cada 9m² (6 unitats).
- **Detectors de temperatura** amb alarma a 60°C.
- **Sistema d'extinció per gas inert** (Inergen IG-541, 52% N₂, 40% Ar, 8% CO₂):
  - No danya els equips electrònics.
  - Segur per a persones (reducció d'O₂ fins al 12,5%).
  - Activació automàtica als 30 segons de detecció confirmada.
  - Activació manual des del panell de control a l'entrada.
- **Extintors manuals** CO₂ (2 unitats) per a actuació manual.
- **Panell de control d'incendis** a l'entrada connectat al sistema central de l'edifici.

### 4.4 Vies d'evacuació

- **Porta principal** (única entrada/sortida): obertura en mode pànic des de l'interior.
- **Senyalització fotoluminescent** de la ruta d'evacuació.
- **Il·luminació d'emergència** autònoma amb bateria de 3 hores.
- **Punt de trobada** designat a l'aparcament exterior, a 50m de l'edifici.

```
CPD                     Passadís              Sortida
┌──────┐    ←←←←←←←←←    ┌──────┐   →→→→   ┌──────┐
│      │ Porta emergència  │      │           │SORTIDA│
│      │ ──────────────►   │      │           │ 🚪   │
└──────┘                   └──────┘           └──────┘
```

> 📸 Afegir plànol de les vies d'evacuació.

---

## 5. Seguretat lògica

### 5.1 Restricció d'accés per autorització

- **Autenticació centralitzada** via Samba AD (domini BTIS).
- **Rols diferenciats**: cada usuari té accés únicament a les dades del seu departament.
- **Accés SSH** únicament amb clau pública/privada, sense contrasenyes.
- **Usuari administrador**: `adminitb` (específic del projecte, no l'usuari per defecte).
- **Sudo sense contrasenya** per a l'usuari adminitb amb registre de totes les accions.

### 5.2 Firewalls — Security Groups AWS

Cada instància EC2 té el seu Security Group específic amb el **principi de mínim privilegi**:

- Les màquines de la subnet privada (samba-ad, mariadb, logs-server) no tenen IP pública i només accepten trànsit intern.
- Els ports oberts al públic es limiten al mínim necessari (80, 443, 22).
- El servidor de bases de dades (MariaDB) només accepta connexions des de la VPC interna (port 3306).

Vegeu la documentació completa dels Security Groups a `arquitectura.md`.

### 5.3 Monitoratge

- **Logs centralitzats** via rsyslog al servidor `logs-server-private`, emmagatzemats al EFS.
- **Dashboard de logs** al portal web (accessible únicament per `admin.itb`).
- **CloudWatch** d'AWS per a mètriques de CPU, memòria i xarxa de les EC2.
- **Alertes automàtiques** via SNS quan el disc supera el 80% d'ús.
- **Logrotate** configurat per rotar logs diàriament i mantenir 7 dies d'historial.

### 5.4 Còpies de seguretat

- **Backup diari automàtic** de MariaDB via event scheduler:
  ```sql
  CREATE EVENT ev_backup_diari
  ON SCHEDULE EVERY 1 DAY
  DO SELECT * INTO OUTFILE '/var/lib/mysql-files/backup_...'
  ```
- **Taula `backups_control`** a MariaDB per registrar l'estat de cada backup.
- **S3 bucket** `s3-innovatetech-media` per a còpies de seguretat externes.
- **Snapshots EBS** setmanals de totes les instàncies EC2 via AWS Backup.

### 5.5 RAIDs

Tots els servidors físics del CPD incorporen RAID per garantir la disponibilitat de dades:

| Servidor | RAID | Configuració | Justificació |
|----------|------|-------------|--------------|
| Servidors generals | RAID 1 | 2x SSD mirall | Redundància sense pèrdua de rendiment |
| MariaDB | RAID 10 | 4x SSD | Màxim rendiment + redundància per a BD |
| NAS backup | RAID 6 | 12x HDD | Tolerància a 2 fallades simultànies |

> **Justificació RAID 1 per servidors**: Còpia exacta en temps real. Si un disc falla, el servidor continua funcionant sense interrupció. El cost és assumible (doble disc) per a màquines de servei.

> **Justificació RAID 10 per MariaDB**: La base de dades és el component més crític. RAID 10 combina la velocitat de RAID 0 (striping) amb la redundància de RAID 1 (mirroring), ideal per a càrregues de lectura/escriptura intenses.

---

## 6. Prevenció de riscos laborals

### 6.1 Riscos identificats al CPD

| Risc | Mesura preventiva |
|------|-------------------|
| Caigudes per cablejat al terra | Terra tècnic elevat, cablejat canalitzat |
| Descàrregues elèctriques | Totes les instal·lacions per professional certificat, senyalització de perill elèctric |
| Soroll dels equips (>85 dB) | EPI obligatori (protectors auditius) per treballs de >30 min al CPD |
| Temperatura extrema | Sensors i alarmes ambientals, accés limitat a personal format |
| Incendi | Detectors, extinció automàtica, simulacres anuals |
| Atrapament a l'interior | Porta obre des de l'interior sense codi, sistema antipànic |
| Exposició a gasos d'extinció | Protocol d'evacuació previ a activació automàtica (retard 30s) |
| Sobrecàrrega de racks | Racks certificats per càrrega màxima, distribució de pes documentada |

### 6.2 Mesures generals

- **Formació obligatòria** per a tot el personal que accedeixi al CPD: riscos elèctrics, protocol d'incendis i evacuació.
- **Treball en solitari prohibit**: sempre mínim 2 persones durant intervencions als equips.
- **EPIs disponibles** a l'entrada: guants dielèctrics, ulleres de protecció, protectors auditius.
- **Senyalització** de riscos elèctrics, càrregues pesades i zones de pas restringit.
- **Simulacres d'evacuació** almenys 1 vegada a l'any.
- **Registre de manteniment** documentat per a totes les intervencions.

---

## 7. Implementació al núvol AWS

### 7.1 Serveis implementats

| Servei | Màquina | IP Privada | Tecnologia |
|--------|---------|------------|------------|
| Servei web | web-sftp | 10.0.5.140 | NGINX + PHP + HTTPS |
| Servei SFTP | web-sftp | 10.0.5.140 | OpenSSH SFTP + Samba AD |
| Logs centralitzats | logs-server-private | 10.0.133.107 | rsyslog + EFS |
| Directori actiu | samba-ad | 10.0.141.9 | Samba AD (domini BTIS) |
| Base de dades | mariadb | 10.0.142.205 | MariaDB 10.11 |
| Àudio/streaming | multimedia | 10.0.8.36 | Icecast2 + ffmpeg |
| Vídeo | multimedia | 10.0.8.36 | Jellyfin |
| Videoconferència | jitsi-meet | 10.0.14.189 | Jitsi Meet (Docker) |

### 7.2 Serveis per màquina separada

Cada servei principal està instal·lat en una instància EC2 diferent, complint el requisit del projecte:

- ✅ Servei web + SFTP → `web-sftp` (excepció permesa)
- ✅ Logs centralitzats → `logs-server-private`
- ✅ Directori actiu → `samba-ad`
- ✅ Base de dades → `mariadb`
- ✅ Àudio/Vídeo → `multimedia`
- ✅ Videoconferència → `jitsi-meet`
- ✅ Automatització → `ansible-controller`

### 7.3 Automatització amb Ansible

Més de 2 màquines configurades amb Ansible:

- ✅ `logs-server-private`: rsyslog receptor + EFS muntat
- ✅ `web-sftp`: rsyslog client
- ✅ `mariadb`: MariaDB + rsyslog client
- ✅ Totes les EC2: rsyslog client via playbook `logs_clients.yml`

### 7.4 Administració amb usuari específic

- Usuari: `adminitb` (no l'usuari per defecte `ubuntu`)
- Autenticació: clau pública/privada (I.pem, S.pem, T.pem)
- Accés SSH sense contrasenya
- Sudo sense contrasenya per a operacions d'administració# Proposta de CPD — InnovateTech

**Responsable:** Bilal El Younossi  
**Data:** Maig 2026

---

## 1. Ubicació física

### 1.1 Situació de la sala a l'edifici

El CPD d'InnovateTech es troba a la **planta soterrani** de l'edifici corporatiu, allunyat de façanes exteriors i zones de pas públic. Aquesta ubicació ofereix els avantatges següents:

- **Protecció tèrmica**: la planta soterrani manté temperatures més estables durant tot l'any.
- **Seguretat**: difícil accés des de l'exterior, sense finestres.
- **Reducció de vibracions**: allunyat de vies de circulació pesada.
- **Accés controlat**: un únic accés principal amb porta blindada.

La sala ocupa **50 m²** amb dimensions de 10m x 5m i alçada lliure de 3m (2,5m útils amb terra tècnic i sostre tècnic).

```
┌─────────────────────────────────────────────┐
│              PLANTA SOTERRANI               │
│                                             │
│  ┌─────────────────────┐                    │
│  │   SALA CPD (50m²)   │ ← Accés controlat  │
│  │                     │   porta blindada    │
│  │  Rack 1 │ Rack 2    │                    │
│  │         │           │                    │
│  │  [A/C]  │  [A/C]    │                    │
│  └─────────────────────┘                    │
│                                             │
│  Escales   Aparcament   Magatzem            │
└─────────────────────────────────────────────┘
```

> 📸 Afegir plànol de l'edifici amb la ubicació del CPD marcada.

---

### 1.2 Climatització

El sistema de climatització és crític per mantenir els equips en condicions òptimes de funcionament.

#### Paràmetres ambientals objectiu

| Paràmetre | Valor òptim | Rang acceptable |
|-----------|-------------|-----------------|
| Temperatura | 21°C | 18°C – 27°C |
| Humitat relativa | 45% | 40% – 60% |
| Partícules en suspensió | < 100.000 p/m³ | — |

#### Sistema instal·lat

- **2 unitats de climatització de precisió** marca APC InRow RC (9 kW cadascuna) en configuració N+1 (una de redundant).
- **Sistema de flux d'aire** de baix a dalt: aire fred entra pel terra tècnic i l'aire calent surt pel sostre tècnic cap als conductes d'extracció.
- **Configuració de corredor fred/calent**: els racks estan orientats cara a cara per separar els corredors d'aire fred (davant dels racks) i calent (darrere dels racks).
- **Sensors ambientals** en 3 punts de la sala: entrada, centre i sortida.
- **Sistema de filtratge HEPA** per eliminar partícules i pols.

```
        CORREDOR FRED          CORREDOR CALENT
        ↓ Aire fred ↓          ↑ Aire calent ↑
   ┌──────────────────────────────────────────┐
   │  [Rack 1] →→→ | ←←← [Rack 2]            │
   │  FRONT          FRONT                    │
   │  ↑ Aire fred ↑  ↑ Aire fred ↑            │
   │                                          │
   │  [A/C 1]              [A/C 2 redundant]  │
   └──────────────────────────────────────────┘
         ↑ Terra tècnic (aire fred)
```

> 📸 Afegir fotografies del sistema de climatització instal·lat.

---

### 1.3 Mesures per dificultar la identificació de la sala

- **Senyalització neutra**: la porta no indica "CPD" ni "Sala de servidors". Apareix identificada com "Manteniment Tècnic B-03".
- **Sense finestres** cap a l'exterior ni cap al passadís principal.
- **Porta blindada** sense maneta exterior, amb obertura únicament per targeta RFID.
- **Cable i alimentació soterrats**: no hi ha cablejat visible als passadissos que pugui indicar la ubicació.
- **Murs reforçats**: les parets de la sala son de formigó armat de 20cm, sense indicació visual de la seva funció.

---

### 1.4 Distribució i gestió del cablejat

#### Principis de gestió del cablejat

- **Separació física** de cablejat de xarxa (blau) i cablejat elèctric (vermell/negre) per evitar interferències.
- **Etiquetatge** de tots els cables als dos extrems amb codi alfanumèric.
- **Brides i canals** per mantenir el cablejat ordenat i accessible.
- **Color coding**:
  - Blau: xarxa de dades (LAN)
  - Verd: connexions entre switches
  - Groc: connexions de gestió (IPMI/iLO)
  - Vermell: alimentació elèctrica
  - Negre: alimentació SAI

#### Distribució

```
TERRA TÈCNIC
├── Canal 1 (costat esquerre): Cablejat de xarxa
│   ├── Patch cables Cat6A (blau)
│   └── Fibra òptica (groc)
├── Canal 2 (costat dret): Alimentació elèctrica
│   ├── Cables PDU (vermell)
│   └── Cables SAI (negre)
└── Canal central: Connexions entre racks
```

> 📸 Afegir fotografies del cablejat organitzat als racks.

---

### 1.5 Terra tècnic i sostre tècnic

#### Terra tècnic

- **Alçada**: 40 cm sobre el terra estructural.
- **Plaques**: 60x60 cm d'acer galvanitzat amb acabat antistàtic.
- **Funció**: distribució d'aire fred des dels climatitzadors cap a la part frontal dels racks, i canalització del cablejat.
- **Càrrega màxima**: 1.200 kg/m².

#### Sostre tècnic

- **Alçada**: 30 cm entre fals sostre i sostre estructural.
- **Funció**: retorn d'aire calent als climatitzadors i pas de les instal·lacions (elèctrica, detecció d'incendis, cablejat de gestió).
- **Plaques desmuntables** per facilitar el manteniment.

```
┌─────────────────────────────────┐ ← Sostre estructural (3m)
│    Conductes retorn aire calent  │
│    Cables gestió                 │
├─────────────────────────────────┤ ← Fals sostre (2.7m)
│                                 │
│          ZONA RACKS              │  2.5m útils
│                                 │
├─────────────────────────────────┤ ← Terra tècnic (0.4m)
│    Cables xarxa / elèctrics     │
│    Distribució aire fred        │
└─────────────────────────────────┘ ← Terra estructural (0m)
```

---

### 1.6 Plànol de la sala CPD

```
         10 metres
    ┌──────────────────────────────┐
    │  [A/C 1]         [A/C 2]    │ 5m
    │                              │
    │  ┌────────┐   ┌────────┐    │
    │  │ RACK 1 │   │ RACK 2 │    │
    │  │        │   │        │    │
    │  │ 42U    │   │ 42U    │    │
    │  └────────┘   └────────┘    │
    │                              │
    │  [SAI 1]         [SAI 2]    │
    │                              │
    │              ← PORTA →      │
    └──────────────────────────────┘
    [Panel elèctric]  [Extintors]
```

> 📸 Afegir plànol CAD o dibuix tècnic de la sala.

---

### 1.7 Estructuració dels racks

#### Rack 1 — Servidors i xarxa

| U | Equip |
|---|-------|
| 1-2 | Patch panel Cat6A (48 ports) |
| 3 | Switch core (Cisco Catalyst 9300) |
| 4 | Switch accés (Cisco Catalyst 2960) |
| 5 | Firewall (Fortinet FortiGate 100F) |
| 6-8 | Servidor web-sftp (Dell PowerEdge R350) |
| 9-11 | Servidor multimedia (Dell PowerEdge R350) |
| 12-14 | Servidor Jitsi (Dell PowerEdge R350) |
| 15-17 | Servidor Ansible (Dell PowerEdge R350) |
| 18-20 | Servidor Samba AD (Dell PowerEdge R350) |
| 40-42 | PDU vertical (APC AP8959) |

#### Rack 2 — Bases de dades, logs i emmagatzematge

| U | Equip |
|---|-------|
| 1-2 | Patch panel Cat6A (48 ports) |
| 3 | Switch gestió (Cisco Catalyst 2960) |
| 4-6 | Servidor MariaDB (Dell PowerEdge R350) |
| 7-9 | Servidor logs (Dell PowerEdge R350) |
| 10-14 | NAS (Synology RS3621xs+, 12 bahies) |
| 15-16 | Servidor de còpies de seguretat |
| 40-42 | PDU vertical (APC AP8959) |

```
RACK 1                    RACK 2
┌──────────┐              ┌──────────┐
│ Patch    │              │ Patch    │ 1U
│ Panel    │              │ Panel    │ 2U
├──────────┤              ├──────────┤
│ Switch   │              │ Switch   │ 3U
│ Core     │              │ Gestió   │ 4U
├──────────┤              ├──────────┤
│ Firewall │              │ MariaDB  │ 5-7U
├──────────┤              ├──────────┤
│ web-sftp │              │ Logs     │ 8-10U
├──────────┤              ├──────────┤
│Multimedia│              │ NAS      │ 11-15U
├──────────┤              ├──────────┤
│  Jitsi   │              │ Backup   │ 16-17U
├──────────┤              ├──────────┤
│ Ansible  │              │          │
├──────────┤              │  (lliure)│
│ Samba AD │              │          │
├──────────┤              ├──────────┤
│   PDU    │              │   PDU    │ 40-42U
└──────────┘              └──────────┘
```

> 📸 Afegir fotografies dels racks amb els equips instal·lats.

---

## 2. Infraestructura IT

### 2.1 Servidors

| Servidor | Model | CPU | RAM | Disc | Funció |
|----------|-------|-----|-----|------|--------|
| web-sftp | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Web + SFTP |
| multimedia | Dell PowerEdge R350 | Intel Xeon E-2334 | 32 GB | 2x960GB SSD RAID1 | Icecast + Jellyfin |
| jitsi-meet | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Videoconferència |
| ansible | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Automatització |
| samba-ad | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Directori actiu |
| mariadb | Dell PowerEdge R650 | Intel Xeon Silver 4310 | 64 GB | 4x960GB SSD RAID10 | Base de dades |
| logs-server | Dell PowerEdge R350 | Intel Xeon E-2334 | 16 GB | 2x480GB SSD RAID1 | Logs centralitzats |

### 2.2 Patch panels

- **2x Patch panel Cat6A de 48 ports** (un per rack)
- Connexió entre els ports del patch panel i els ports dels switches mitjançant patch cables curts (0,5m) per minimitzar el cablejat dins el rack.
- Etiquetatge de cada port amb la màquina o dispositiu connectat.

### 2.3 Switches

| Switch | Model | Ports | Funció |
|--------|-------|-------|--------|
| Switch core | Cisco Catalyst 9300-24T | 24x1G + 4x10G uplink | Interconnexió entre racks i uplink internet |
| Switch accés 1 | Cisco Catalyst 2960-24TC | 24x1G | Connexió servidors Rack 1 |
| Switch accés 2 | Cisco Catalyst 2960-24TC | 24x1G | Connexió servidors Rack 2 |
| Switch gestió | Cisco Catalyst 2960-8TC | 8x1G | Gestió IPMI/iLO dels servidors |

---

## 3. Infraestructura elèctrica

### 3.1 Alimentació redundant

- **2 línies elèctriques independents** des del quadre general de l'edifici.
- Cada rack disposa de **2 PDUs** (Power Distribution Units) connectades a línies elèctriques diferents.
- Tots els servidors tenen **2 fonts d'alimentació** redundants (PSU1 → Línia A, PSU2 → Línia B).

```
Xarxa elèctrica
      │
  ┌───┴───┐
  │  UPS  │  (APC Symmetra LX 16 kVA)
  └───┬───┘
      │
  ┌───┴─────────┐
  │             │
Línia A      Línia B
  │             │
PDU Rack1A  PDU Rack1B    PDU Rack2A  PDU Rack2B
  │             │             │           │
PSU1        PSU2          PSU1        PSU2
(tots els servidors)
```

### 3.2 SAI — Sistemes d'Alimentació Ininterrompuda

#### Model seleccionat

**APC Symmetra LX 16 kVA** amb bateries externes.

#### Càlcul de consum

| Equip | Consum estimat |
|-------|---------------|
| 7 servidors Dell PowerEdge R350 | 7 × 350W = 2.450W |
| 2 switches Cisco Catalyst | 2 × 370W = 740W |
| 1 switch core Cisco 9300 | 1 × 715W = 715W |
| 2 climatitzadors APC InRow | 2 × 200W (ventiladors) = 400W |
| 1 NAS Synology | 1 × 100W = 100W |
| **TOTAL** | **4.405W** |
| **Marge de seguretat (+20%)** | **5.286W ≈ 5,3 kW** |

#### Autonomia

- **SAI APC Symmetra LX 16 kVA** amb 2 mòduls de bateries externs (SYBATT).
- Cada mòdul proporciona ~15 minuts a plena càrrega.
- Amb 2 mòduls: **~30 minuts d'autonomia** a plena càrrega.
- Temps suficient per a un apagat ordenat de tots els servidors o per a que el grup electrogen entri en funcionament.

> **Justificació**: 30 minuts és el temps estàndard recomanat per a CPDs de mida mitjana, suficient per a engegar un generador de backup o fer un apagat controlat.

---

## 4. Seguretat física

### 4.1 Control d'accés

- **Porta blindada** amb tancament electrònic i obertura per doble factor:
  1. Targeta RFID (HID iClass)
  2. PIN de 6 dígits
- **Registre d'accessos**: tot accés queda registrat amb data, hora i identitat.
- **Llista blanca**: únicament personal autoritzat explícitament pot accedir.
- **Accés d'emergència**: clau física en caixa forta al despatx del responsable IT.
- **Alarma**: activació automàtica si la porta s'obre sense autenticació o queda oberta més de 60 segons.

### 4.2 Videovigilància

- **4 càmeres IP** de 4K (Axis P3245-V) distribuïdes:
  - 1 a l'entrada de la sala
  - 1 al corredor fred (davant dels racks)
  - 1 al corredor calent (darrere dels racks)
  - 1 a la zona del quadre elèctric i SAI
- **Enregistrament**: 24/7 amb retenció de 30 dies en NAS dedicat.
- **Alertes**: detecció de moviment amb notificació per correu electrònic.
- **Accés remot**: visualització en temps real des del sistema de gestió IT.

```
┌──────────────────────────────┐
│  📷 Càmera entrada           │
│                              │
│  ┌────────┐   ┌────────┐    │
│  │📷 Rack1│   │ Rack 2 │📷  │
│  └────────┘   └────────┘    │
│                              │
│         📷 SAI/Elèctric      │
└──────────────────────────────┘
```

> 📸 Afegir fotografies de les càmeres instal·lades.

### 4.3 Sistemes de prevenció i extinció d'incendis

- **Detectors de fum** iònics i òptics distribuïts cada 9m² (6 unitats).
- **Detectors de temperatura** amb alarma a 60°C.
- **Sistema d'extinció per gas inert** (Inergen IG-541, 52% N₂, 40% Ar, 8% CO₂):
  - No danya els equips electrònics.
  - Segur per a persones (reducció d'O₂ fins al 12,5%).
  - Activació automàtica als 30 segons de detecció confirmada.
  - Activació manual des del panell de control a l'entrada.
- **Extintors manuals** CO₂ (2 unitats) per a actuació manual.
- **Panell de control d'incendis** a l'entrada connectat al sistema central de l'edifici.

### 4.4 Vies d'evacuació

- **Porta principal** (única entrada/sortida): obertura en mode pànic des de l'interior.
- **Senyalització fotoluminescent** de la ruta d'evacuació.
- **Il·luminació d'emergència** autònoma amb bateria de 3 hores.
- **Punt de trobada** designat a l'aparcament exterior, a 50m de l'edifici.

```
CPD                     Passadís              Sortida
┌──────┐    ←←←←←←←←←    ┌──────┐   →→→→   ┌──────┐
│      │ Porta emergència  │      │           │SORTIDA│
│      │ ──────────────►   │      │           │ 🚪   │
└──────┘                   └──────┘           └──────┘
```

> 📸 Afegir plànol de les vies d'evacuació.

---

## 5. Seguretat lògica

### 5.1 Restricció d'accés per autorització

- **Autenticació centralitzada** via Samba AD (domini BTIS).
- **Rols diferenciats**: cada usuari té accés únicament a les dades del seu departament.
- **Accés SSH** únicament amb clau pública/privada, sense contrasenyes.
- **Usuari administrador**: `adminitb` (específic del projecte, no l'usuari per defecte).
- **Sudo sense contrasenya** per a l'usuari adminitb amb registre de totes les accions.

### 5.2 Firewalls — Security Groups AWS

Cada instància EC2 té el seu Security Group específic amb el **principi de mínim privilegi**:

- Les màquines de la subnet privada (samba-ad, mariadb, logs-server) no tenen IP pública i només accepten trànsit intern.
- Els ports oberts al públic es limiten al mínim necessari (80, 443, 22).
- El servidor de bases de dades (MariaDB) només accepta connexions des de la VPC interna (port 3306).

Vegeu la documentació completa dels Security Groups a `arquitectura.md`.

### 5.3 Monitoratge

- **Logs centralitzats** via rsyslog al servidor `logs-server-private`, emmagatzemats al EFS.
- **Dashboard de logs** al portal web (accessible únicament per `admin.itb`).
- **CloudWatch** d'AWS per a mètriques de CPU, memòria i xarxa de les EC2.
- **Alertes automàtiques** via SNS quan el disc supera el 80% d'ús.
- **Logrotate** configurat per rotar logs diàriament i mantenir 7 dies d'historial.

### 5.4 Còpies de seguretat

- **Backup diari automàtic** de MariaDB via event scheduler:
  ```sql
  CREATE EVENT ev_backup_diari
  ON SCHEDULE EVERY 1 DAY
  DO SELECT * INTO OUTFILE '/var/lib/mysql-files/backup_...'
  ```
- **Taula `backups_control`** a MariaDB per registrar l'estat de cada backup.
- **S3 bucket** `s3-innovatetech-media` per a còpies de seguretat externes.
- **Snapshots EBS** setmanals de totes les instàncies EC2 via AWS Backup.

### 5.5 RAIDs

Tots els servidors físics del CPD incorporen RAID per garantir la disponibilitat de dades:

| Servidor | RAID | Configuració | Justificació |
|----------|------|-------------|--------------|
| Servidors generals | RAID 1 | 2x SSD mirall | Redundància sense pèrdua de rendiment |
| MariaDB | RAID 10 | 4x SSD | Màxim rendiment + redundància per a BD |
| NAS backup | RAID 6 | 12x HDD | Tolerància a 2 fallades simultànies |

> **Justificació RAID 1 per servidors**: Còpia exacta en temps real. Si un disc falla, el servidor continua funcionant sense interrupció. El cost és assumible (doble disc) per a màquines de servei.

> **Justificació RAID 10 per MariaDB**: La base de dades és el component més crític. RAID 10 combina la velocitat de RAID 0 (striping) amb la redundància de RAID 1 (mirroring), ideal per a càrregues de lectura/escriptura intenses.

---

## 6. Prevenció de riscos laborals

### 6.1 Riscos identificats al CPD

| Risc | Mesura preventiva |
|------|-------------------|
| Caigudes per cablejat al terra | Terra tècnic elevat, cablejat canalitzat |
| Descàrregues elèctriques | Totes les instal·lacions per professional certificat, senyalització de perill elèctric |
| Soroll dels equips (>85 dB) | EPI obligatori (protectors auditius) per treballs de >30 min al CPD |
| Temperatura extrema | Sensors i alarmes ambientals, accés limitat a personal format |
| Incendi | Detectors, extinció automàtica, simulacres anuals |
| Atrapament a l'interior | Porta obre des de l'interior sense codi, sistema antipànic |
| Exposició a gasos d'extinció | Protocol d'evacuació previ a activació automàtica (retard 30s) |
| Sobrecàrrega de racks | Racks certificats per càrrega màxima, distribució de pes documentada |

### 6.2 Mesures generals

- **Formació obligatòria** per a tot el personal que accedeixi al CPD: riscos elèctrics, protocol d'incendis i evacuació.
- **Treball en solitari prohibit**: sempre mínim 2 persones durant intervencions als equips.
- **EPIs disponibles** a l'entrada: guants dielèctrics, ulleres de protecció, protectors auditius.
- **Senyalització** de riscos elèctrics, càrregues pesades i zones de pas restringit.
- **Simulacres d'evacuació** almenys 1 vegada a l'any.
- **Registre de manteniment** documentat per a totes les intervencions.

---

## 7. Implementació al núvol AWS

### 7.1 Serveis implementats

| Servei | Màquina | IP Privada | Tecnologia |
|--------|---------|------------|------------|
| Servei web | web-sftp | 10.0.5.140 | NGINX + PHP + HTTPS |
| Servei SFTP | web-sftp | 10.0.5.140 | OpenSSH SFTP + Samba AD |
| Logs centralitzats | logs-server-private | 10.0.133.107 | rsyslog + EFS |
| Directori actiu | samba-ad | 10.0.141.9 | Samba AD (domini BTIS) |
| Base de dades | mariadb | 10.0.142.205 | MariaDB 10.11 |
| Àudio/streaming | multimedia | 10.0.8.36 | Icecast2 + ffmpeg |
| Vídeo | multimedia | 10.0.8.36 | Jellyfin |
| Videoconferència | jitsi-meet | 10.0.14.189 | Jitsi Meet (Docker) |

### 7.2 Serveis per màquina separada

Cada servei principal està instal·lat en una instància EC2 diferent, complint el requisit del projecte:

- ✅ Servei web + SFTP → `web-sftp` (excepció permesa)
- ✅ Logs centralitzats → `logs-server-private`
- ✅ Directori actiu → `samba-ad`
- ✅ Base de dades → `mariadb`
- ✅ Àudio/Vídeo → `multimedia`
- ✅ Videoconferència → `jitsi-meet`
- ✅ Automatització → `ansible-controller`

### 7.3 Automatització amb Ansible

Més de 2 màquines configurades amb Ansible:

- ✅ `logs-server-private`: rsyslog receptor + EFS muntat
- ✅ `web-sftp`: rsyslog client
- ✅ `mariadb`: MariaDB + rsyslog client
- ✅ Totes les EC2: rsyslog client via playbook `logs_clients.yml`

### 7.4 Administració amb usuari específic

- Usuari: `adminitb` (no l'usuari per defecte `ubuntu`)
- Autenticació: clau pública/privada (I.pem, S.pem, T.pem)
- Accés SSH sense contrasenya
- Sudo sense contrasenya per a operacions d'administració
