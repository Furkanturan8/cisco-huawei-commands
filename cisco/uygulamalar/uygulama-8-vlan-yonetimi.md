# Uygulama 8: VLAN Oluşturma ve Yönetimi

## 🎯 Senaryo

XYZ Şirketinin network altyapısında VLAN segmentasyonu yapılması gerekiyor. Farklı departmanlar için ayrı VLANlar oluşturulacak ve güvenlik politikaları uygulanacak.

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
enable
configure terminal

# VTP domain ayarla
vtp mode server
vtp domain XYZ_COMPANY
vtp password XYZ123#
```

### 2. VLAN Oluşturma

```bash
# VLANları oluştur
vlan 10
name IT_DEPARTMENT
exit

vlan 20
name ACCOUNTING_DEPARTMENT
exit

vlan 30
name SALES_DEPARTMENT
exit

vlan 99
name MANAGEMENT_VLAN
exit

vlan 100
name VOICE_VLAN
exit
```

### 3. VLAN Interface (SVI) Konfigürasyonu

```bash
# VLAN 10 interfacei (IT Departmanı)
interface Vlan10
description IT Department VLAN
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

# VLAN 20 interfacei (Muhasebe Departmanı)
interface Vlan20
description Accounting Department VLAN
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

# VLAN 30 interfacei (Satış Departmanı)
interface Vlan30
description Sales Department VLAN
ip address 192.168.30.1 255.255.255.0
no shutdown
exit

# VLAN 99 interfacei (Management)
interface Vlan99
description Management VLAN
ip address 192.168.99.1 255.255.255.0
no shutdown
exit

# VLAN 100 interfacei (Voice)
interface Vlan100
description Voice VLAN
ip address 192.168.100.1 255.255.255.0
no shutdown
exit
```

### 4. Trunk Port Konfigürasyonu

```bash
# Core Switchte trunk port ayarla
interface GigabitEthernet0/0/1
description Trunk to Access Switch 1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,99,100
switchport trunk native vlan 99
no shutdown
exit

interface GigabitEthernet0/0/2
description Trunk to Access Switch 2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,99,100
switchport trunk native vlan 99
no shutdown
exit
```

### 5. Access Port Konfigürasyonu

```bash
# Access Switch 1de port konfigürasyonu
# IT Departmanı PCsi
interface FastEthernet0/1
description IT Department PC
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
no shutdown
exit

# Muhasebe Departmanı PCsi
interface FastEthernet0/2
description Accounting Department PC
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
no shutdown
exit
```

### 6. Voice VLAN Konfigürasyonu

```bash
# IP Phone portu
interface FastEthernet0/3
description IP Phone Port
switchport mode access
switchport access vlan 10
switchport voice vlan 100
mls qos trust cos
no shutdown
exit
```

### 7. DHCP Server Konfigürasyonu

```bash
# DHCP serverı etkinleştir
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10

# IT Departmanı DHCP poolu
ip dhcp pool IT_DEPARTMENT
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
lease 7
exit

# Muhasebe Departmanı DHCP poolu
ip dhcp pool ACCOUNTING_DEPARTMENT
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
lease 7
exit

# Satış Departmanı DHCP poolu
ip dhcp pool SALES_DEPARTMENT
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
lease 7
exit
```

### 8. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
write memory
```

## ✅ Verification Komutları

### VLAN Kontrolleri

```bash
# VLAN listesini göster
show vlan brief

# Detaylı VLAN bilgilerini göster
show vlan

# VTP durumunu kontrol et
show vtp status

# VLAN interfacelerini kontrol et
show ip interface brief | include Vlan
```

### Trunk Port Kontrolleri

```bash
# Trunk port durumunu göster
show interface trunk

# Belirli trunk portunu göster
show interface GigabitEthernet0/0/1 trunk

# Trunk allowed VLANları göster
show interface trunk | include allowed
```

### Access Port Kontrolleri

```bash
# Access port durumunu göster
show interface FastEthernet0/1 switchport

# Port security durumunu göster
show port-security

# Port security interfacelerini göster
show port-security interface FastEthernet0/1
```

### DHCP Kontrolleri

```bash
# DHCP poollarını göster
show ip dhcp pool

# DHCP bindinglerini göster
show ip dhcp binding

# DHCP conflictleri göster
show ip dhcp conflict
```

## 🔍 Troubleshooting

### Yaygın VLAN Sorunları

#### 1. VLAN Görünmüyor
```bash
# VTP domaini kontrol et
show vtp status

# VLAN databaseini kontrol et
show vlan brief

# VTP passwordü kontrol et
show vtp password
```

#### 2. Trunk Port Sorunu
```bash
# Trunk port durumunu kontrol et
show interface trunk

# Trunk portu reset et
configure terminal
interface GigabitEthernet0/0/1
shutdown
no shutdown
exit
```

#### 3. Port Security Sorunu
```bash
# Port security durumunu kontrol et
show port-security

# Port securityyi reset et
configure terminal
interface FastEthernet0/1
no switchport port-security
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
exit
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
show mls qos interface FastEthernet0/3
```

## 🔐 Güvenlik Kontrolleri

### Access Control List (ACL)

```bash
# IT departmanı için ACL
access-list 110 permit ip 192.168.10.0 0.0.0.255 any
access-list 110 deny ip any any

# Muhasebe departmanı için ACL
access-list 120 permit ip 192.168.20.0 0.0.0.255 any
access-list 120 deny ip any any

# ACLleri interfacee uygula
interface Vlan10
ip access-group 110 in
exit

interface Vlan20
ip access-group 120 in
exit
```

## ⚠️ Önemli Notlar

1. **VLAN Planning**: VLAN numaralarını planlayın
2. **Native VLAN**: Trunk portlarda native VLANı ayarlayın
3. **VTP Domain**: Tüm switchlerde aynı VTP domain kullanın
4. **Port Security**: Production ortamında port security kullanın
5. **Voice VLAN**: VoIP için ayrı VLAN kullanın
6. **Documentation**: VLAN planını dokümante edin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- VLAN oluşturma ve yönetimi
- Trunk ve access port konfigürasyonu
- VTP domain yönetimi
- Port security uygulaması
- Voice VLAN konfigürasyonu
- DHCP server konfigürasyonu
- VLAN troubleshooting teknikleri 