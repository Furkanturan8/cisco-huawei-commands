# Uygulama 2: Switch Konfigürasyonu ve VLAN Yönetimi

## 🎯 Senaryo

ABC Şirketinin yeni ofisinde switch konfigürasyonu yapılması gerekiyor. Access switchler kurulacak, VLANlar oluşturulacak ve güvenlik ayarları yapılacak.

## 📋 Gereksinimler

1. Switch hostnamei "ABC-SW-01" olarak ayarlanacak
2. VLANlar oluşturulacak (Data, Voice, Management)
3. Access portlar konfigüre edilecek
4. Port security uygulanacak
5. VTP domain ayarlanacak
6. Spanning Tree Protocol konfigüre edilecek

## 🌐 Topoloji

```
[Core Switch] --- [ABC-SW-01] --- [PC-Data]
      |                    |
      |              [PC-Voice]
      |
[ABC-SW-02] --- [PC-Management]
```

## 🔧 Adım Adım Çözüm

### 1. Switch Temel Konfigürasyonu

```bash
# Switche console ile bağlan
enable
configure terminal

# Hostname ayarla
hostname ABC-SW-01

# Banner mesajları ekle
banner login #
===========================================
    ABC Şirketi Switch Cihazı
    Yetkisiz erişim yasaktır#
===========================================
#

banner motd #
Sistem bakımı 22:00-02:00 arası yapılacaktır
#
```

### 2. VTP Domain Konfigürasyonu

```bash
# VTP domain ayarla
vtp mode client
vtp domain ABC_COMPANY
vtp password ABC123#
```

### 3. VLAN Oluşturma

```bash
# VLANları oluştur
vlan 10
name DATA_VLAN
exit

vlan 20
name VOICE_VLAN
exit

vlan 99
name MANAGEMENT_VLAN
exit
```

### 4. Access Port Konfigürasyonu

```bash
# Data VLAN portu
interface FastEthernet0/1
description Data PC Port
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
switchport port-security mac-address sticky
no shutdown
exit

# Voice VLAN portu
interface FastEthernet0/2
description Voice PC Port
switchport mode access
switchport access vlan 10
switchport voice vlan 20
mls qos trust cos
no shutdown
exit

# Management VLAN portu
interface FastEthernet0/3
description Management PC Port
switchport mode access
switchport access vlan 99
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
no shutdown
exit
```

### 5. Trunk Port Konfigürasyonu

```bash
# Uplink trunk portu
interface GigabitEthernet0/1
description Trunk to Core Switch
switchport mode trunk
switchport trunk allowed vlan 10,20,99
switchport trunk native vlan 99
no shutdown
exit
```

### 6. Spanning Tree Protocol Konfigürasyonu

```bash
# STP root bridge ayarla
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root secondary
spanning-tree vlan 99 root secondary

# Port cost ayarları
interface GigabitEthernet0/1
spanning-tree cost 1000
exit
```

### 7. Port Security Ayarları

```bash
# Port security global ayarları
switchport port-security aging time 2
switchport port-security aging type inactivity

# Interfacelerde port security
interface range FastEthernet0/1, FastEthernet0/3
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
switchport port-security mac-address sticky
exit
```

### 8. Güvenlik Ayarları

```bash
# Enable password
enable secret ABC123#

# Console password
line console 0
password ABC123#
login
exit

# Telnet password
line vty 0 4
password ABC123#
login
exit

# SSH konfigürasyonu
ip domain-name abc.com
crypto key generate rsa
# 1024 bit RSA key oluştur
ip ssh version 2
```

### 9. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
write memory
```

## ✅ Verification Komutları

### Switch Temel Kontrolleri

```bash
# Hostnamei kontrol et
show running-config | include hostname

# Bannerları kontrol et
show running-config | include banner

# VTP durumunu kontrol et
show vtp status

# VLAN listesini göster
show vlan brief
```

### Port Kontrolleri

```bash
# Access port durumunu göster
show interface FastEthernet0/1 switchport

# Trunk port durumunu göster
show interface trunk

# Port security durumunu göster
show port-security

# Port security interfacelerini göster
show port-security interface FastEthernet0/1
```

### STP Kontrolleri

```bash
# STP durumunu göster
show spanning-tree

# STP root bridgeini göster
show spanning-tree root

# STP port durumunu göster
show spanning-tree interface GigabitEthernet0/1
```

### Güvenlik Kontrolleri

```bash
# SSH durumunu kontrol et
show ip ssh

# Line konfigürasyonlarını kontrol et
show running-config | section line

# Interface güvenlik ayarlarını kontrol et
show interface FastEthernet0/1
```

## 🔍 Troubleshooting

### Yaygın Switch Sorunları

#### 1. VLAN Görünmüyor
```bash
# VTP domaini kontrol et
show vtp status

# VLAN databaseini kontrol et
show vlan brief

# VTP passwordü kontrol et
show vtp password
```

#### 2. Port Security Sorunu
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

#### 3. STP Loop Sorunu
```bash
# STP durumunu kontrol et
show spanning-tree

# STP debugını başlat
debug spanning-tree events
# Sorun çözüldükten sonra durdur
no debug spanning-tree events
```

## 📊 Test Senaryoları

### Test 1: VLAN Connectivity
```bash
# Data VLANdan ping testi
ping 192.168.10.1

# Voice VLANdan ping testi
ping 192.168.20.1

# Management VLANdan ping testi
ping 192.168.99.1
```

### Test 2: Port Security
```bash
# Farklı MAC address ile bağlanmayı dene
# Port security violation olmalı

# Port security durumunu kontrol et
show port-security interface FastEthernet0/1
```

### Test 3: STP Convergence
```bash
# Linki kapat ve aç
configure terminal
interface GigabitEthernet0/1
shutdown
no shutdown
exit

# STP convergenceı izle
show spanning-tree
```

## 🔐 Güvenlik Kontrolleri

### Access Control List (ACL)

```bash
# Management VLAN için ACL
access-list 100 permit ip 192.168.99.0 0.0.0.255 any
access-list 100 deny ip any any

# ACLyi interfacee uygula
interface Vlan99
ip access-group 100 in
exit
```

## ⚠️ Önemli Notlar

1. **VLAN Planning**: VLAN numaralarını planlayın
2. **Port Security**: Production ortamında port security kullanın
3. **STP Configuration**: Loopları önlemek için STP ayarlayın
4. **VTP Domain**: Tüm switchlerde aynı VTP domain kullanın
5. **Documentation**: Switch konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- Switch temel konfigürasyonu
- VLAN oluşturma ve yönetimi
- Access ve trunk port konfigürasyonu
- Port security uygulaması
- Spanning Tree Protocol konfigürasyonu
- Switch güvenlik ayarları
- Switch troubleshooting teknikleri 