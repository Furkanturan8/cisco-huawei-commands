# Uygulama 8: VLAN Oluşturma ve Yönetimi

## 🎯 Senaryo

GHI Şirketinin network altyapısında VLAN segmentasyonu yapılması gerekiyor. Farklı departmanlar için ayrı VLANlar oluşturulacak ve güvenlik politikaları uygulanacak.

## 📋 Gereksinimler

1. **VLAN 10**: IT Departmanı (192.168.10.0/24)
2. **VLAN 20**: Muhasebe Departmanı (192.168.20.0/24)
3. **VLAN 30**: Satış Departmanı (192.168.30.0/24)
4. **VLAN 99**: Management VLAN (192.168.99.0/24)
5. **VLAN 100**: Voice VLAN (192.168.100.0/24)
6. Trunk portları konfigüre edilecek
7. VTP domain ayarlanacak
8. Port security uygulanacak

## 🌐 Topoloji

```
[Core Switch] --- [Access Switch 1] --- [PC-IT]
      |                    |
      |              [PC-Muhasebe]
      |
[Access Switch 2] --- [PC-Satis]
      |
[IP Phone]
```

## 🔧 Adım Adım Çözüm

### 1. VTP Domain Konfigürasyonu

```bash
# Core Switchte VTP server ayarla
system-view

# VTP domain ayarla
vtp mode server
vtp domain GHI_COMPANY
vtp password GHI123#
```

### 2. VLAN Oluşturma (Batch Create)

```bash
# VLANları batch olarak oluştur
vlan batch 10 20 30 99 100

# VLAN açıklamalarını ekle
vlan 10
description IT_DEPARTMENT
quit

vlan 20
description ACCOUNTING_DEPARTMENT
quit

vlan 30
description SALES_DEPARTMENT
quit

vlan 99
description MANAGEMENT_VLAN
quit

vlan 100
description VOICE_VLAN
quit
```

### 3. VLAN Interface (SVI) Konfigürasyonu

```bash
# VLAN 10 interfacei (IT Departmanı)
interface Vlanif10
description IT Department VLAN
ip address 192.168.10.1 255.255.255.0
undo shutdown
quit

# VLAN 20 interfacei (Muhasebe Departmanı)
interface Vlanif20
description Accounting Department VLAN
ip address 192.168.20.1 255.255.255.0
undo shutdown
quit

# VLAN 30 interfacei (Satış Departmanı)
interface Vlanif30
description Sales Department VLAN
ip address 192.168.30.1 255.255.255.0
undo shutdown
quit

# VLAN 99 interfacei (Management)
interface Vlanif99
description Management VLAN
ip address 192.168.99.1 255.255.255.0
undo shutdown
quit

# VLAN 100 interfacei (Voice)
interface Vlanif100
description Voice VLAN
ip address 192.168.100.1 255.255.255.0
undo shutdown
quit
```

### 4. Trunk Port Konfigürasyonu

```bash
# Core Switchte trunk port ayarla
interface GigabitEthernet0/0/1
description Trunk to Access Switch 1
port link-type trunk
port trunk allow-pass vlan 10 20 30 99 100
port trunk pvid vlan 99
undo shutdown
quit

interface GigabitEthernet0/0/2
description Trunk to Access Switch 2
port link-type trunk
port trunk allow-pass vlan 10 20 30 99 100
port trunk pvid vlan 99
undo shutdown
quit
```

### 5. Access Port Konfigürasyonu

```bash
# Access Switch 1de port konfigürasyonu
# IT Departmanı PCsi
interface GigabitEthernet0/0/1
description IT Department PC
port link-type access
port default vlan 10
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
undo shutdown
quit

# Muhasebe Departmanı PCsi
interface GigabitEthernet0/0/2
description Accounting Department PC
port link-type access
port default vlan 20
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
undo shutdown
quit
```

### 6. Voice VLAN Konfigürasyonu

```bash
# IP Phone portu
interface GigabitEthernet0/0/3
description IP Phone Port
port link-type access
port default vlan 10
voice-vlan 100 enable
voice-vlan qos trust cos
undo shutdown
quit
```

### 7. DHCP Server Konfigürasyonu

```bash
# DHCP serverı etkinleştir
dhcp enable

# IT Departmanı DHCP poolu
ip pool IT_DEPARTMENT
gateway-list 192.168.10.1
network 192.168.10.0 mask 255.255.255.0
dns-list 8.8.8.8
lease day 7
quit

# Muhasebe Departmanı DHCP poolu
ip pool ACCOUNTING_DEPARTMENT
gateway-list 192.168.20.1
network 192.168.20.0 mask 255.255.255.0
dns-list 8.8.8.8
lease day 7
quit

# Satış Departmanı DHCP poolu
ip pool SALES_DEPARTMENT
gateway-list 192.168.30.1
network 192.168.30.0 mask 255.255.255.0
dns-list 8.8.8.8
lease day 7
quit
```

### 8. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
save
```

## ✅ Verification Komutları

### VLAN Kontrolleri

```bash
# VLAN listesini göster
display vlan brief

# Detaylı VLAN bilgilerini göster
display vlan

# VTP durumunu kontrol et
display vtp status

# VLAN interfacelerini kontrol et
display ip interface brief | include Vlanif
```

### Trunk Port Kontrolleri

```bash
# Trunk port durumunu göster
display port vlan

# Belirli trunk portunu göster
display port vlan GigabitEthernet0/0/1

# Trunk allowed VLANları göster
display port vlan | include allow-pass
```

### Access Port Kontrolleri

```bash
# Access port durumunu göster
display port vlan GigabitEthernet0/0/1

# Port security durumunu göster
display port-security

# Port security interfacelerini göster
display port-security interface GigabitEthernet0/0/1
```

### DHCP Kontrolleri

```bash
# DHCP poollarını göster
display ip pool

# DHCP bindinglerini göster
display dhcp server ip-in-use all

# DHCP conflictleri göster
display dhcp server conflict all
```

## 🔍 Troubleshooting

### Yaygın VLAN Sorunları

#### 1. VLAN Görünmüyor
```bash
# VTP domaini kontrol et
display vtp status

# VLAN databaseini kontrol et
display vlan brief

# VTP passwordü kontrol et
display vtp password
```

#### 2. Trunk Port Sorunu
```bash
# Trunk port durumunu kontrol et
display port vlan

# Trunk portu reset et
system-view
interface GigabitEthernet0/0/1
shutdown
undo shutdown
quit
```

#### 3. Port Security Sorunu
```bash
# Port security durumunu kontrol et
display port-security

# Port securityyi reset et
system-view
interface GigabitEthernet0/0/1
undo port-security
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
quit
```

## 📊 Test Senaryoları

### Test 1: VLAN Connectivity
```bash
# IT departmanından ping testi
ping 192.168.10.1

# Muhasebe departmanından ping testi
ping 192.168.20.1

# Satış departmanından ping testi
ping 192.168.30.1
```

### Test 2: Inter-VLAN Routing
```bash
# ITden muhasebeye ping
ping 192.168.20.10

# Muhasebeden satışa ping
ping 192.168.30.10
```

### Test 3: Voice VLAN
```bash
# IP Phonedan voice VLANa ping
ping 192.168.100.1

# QoS ayarlarını kontrol et
display qos interface GigabitEthernet0/0/3
```

## 🔐 Güvenlik Kontrolleri

### Access Control List (ACL)

```bash
# IT departmanı için ACL
acl 2000
rule 10 permit source 192.168.10.0 0.0.0.255
rule 20 deny source any
quit

# Muhasebe departmanı için ACL
acl 2001
rule 10 permit source 192.168.20.0 0.0.0.255
rule 20 deny source any
quit

# ACLleri interfacee uygula
interface Vlanif10
traffic-filter inbound acl 2000
quit

interface Vlanif20
traffic-filter inbound acl 2001
quit
```

## 🌟 Huawei-Specific Features

### iStack VLAN Konfigürasyonu
```bash
# iStack VLAN ayarları
system-view
stack-port GigabitEthernet0/0/1 enable
stack-port GigabitEthernet0/0/2 enable
vlan 10
stack-port GigabitEthernet0/0/1
stack-port GigabitEthernet0/0/2
quit
```

### CSS VLAN Konfigürasyonu
```bash
# CSS VLAN ayarları
system-view
css-port GigabitEthernet0/0/1 enable
css-port GigabitEthernet0/0/2 enable
vlan 10
css-port GigabitEthernet0/0/1
css-port GigabitEthernet0/0/2
quit
```

## ⚠️ Önemli Notlar

1. **VLAN Planning**: VLAN numaralarını planlayın
2. **Native VLAN**: Trunk portlarda native VLANı ayarlayın
3. **VTP Domain**: Tüm switchlerde aynı VTP domain kullanın
4. **Port Security**: Production ortamında port security kullanın
5. **Voice VLAN**: VoIP için ayrı VLAN kullanın
6. **Documentation**: VLAN planını dokümante edin
7. **Undo Commands**: Huaweide `undo` komutu ile geri alma yapılır

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- Huawei VLAN oluşturma ve yönetimi
- VLAN batch create kullanımı
- Trunk ve access port konfigürasyonu
- VTP domain yönetimi
- Port security uygulaması
- Voice VLAN konfigürasyonu
- DHCP server konfigürasyonu
- iStack ve CSS konfigürasyonu
- VLAN troubleshooting teknikleri 