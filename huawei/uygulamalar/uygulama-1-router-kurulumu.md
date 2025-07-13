# Uygulama 1: Yeni Router Kurulumu ve Temel Konfigürasyon

## 🎯 Senaryo

DEF Şirketi yeni bir ofis açıyor ve yeni bir Huawei router kurulumu yapılması gerekiyor. Routerın temel konfigürasyonu, güvenlik ayarları ve network bağlantısı kurulacak.

## 📋 Gereksinimler

1. Router hostnamei "DEF-Router-01" olarak ayarlanacak
2. Console, Telnet ve SSH erişimi konfigüre edilecek
3. Interfaceler IP address alacak
4. Güvenlik ayarları yapılacak
5. Banner mesajları eklenmeli
6. Konfigürasyon kaydedilmeli

## 🌐 Topoloji

```
[Internet] --- [DEF-Router-01] --- [LAN Network]
                    |
              GigabitEthernet0/0/0
                    |
              IP: 192.168.1.1/24
```

## 🔧 Adım Adım Çözüm

### 1. Routera Bağlanma ve Temel Ayarlar

```bash
# Routera console ile bağlan
# Terminal programı ile COM porta bağlan

# User viewdan system viewa geç
system-view
# veya
sys
```

### 2. Hostname ve Banner Ayarları

```bash
# Hostnamei ayarla
sysname DEF-Router-01

# Banner mesajları ekle
header login "==========================================="
header login "    DEF Şirketi Network Cihazı"
header login "    Yetkisiz erişim yasaktır#"
header login "    Tarih: $(date)"
header login "==========================================="

header shell "Sistem bakımı 22:00-02:00 arası yapılacaktır"
```

### 3. Güvenlik Ayarları

```bash
# Super password ayarla
super password DEF123#

# Console password ayarla
user-interface console 0
authentication-mode password
set authentication password DEF123#
quit

# Telnet password ayarla
user-interface vty 0 4
authentication-mode password
set authentication password DEF123#
quit

# SSH konfigürasyonu
ssh server enable
ssh server cipher aes128-gcm aes256-gcm aes192-gcm aes128-ctr aes192-ctr aes256-ctr
ssh server hmac sha2-256 sha2-512
ssh server key-exchange dh-group14-sha1 dh-group-exchange-sha1 dh-group1-sha1
ssh server publickey rsa
ssh server authentication-timeout 60
ssh server authentication-retries 3
```

### 4. Interface Konfigürasyonu

```bash
# WAN interfaceini konfigüre et
interface GigabitEthernet0/0/0
description WAN Interface - Internet Connection
ip address 192.168.1.1 255.255.255.0
undo shutdown
quit

# LAN interfaceini konfigüre et
interface GigabitEthernet0/0/1
description LAN Interface - Office Network
ip address 10.0.0.1 255.255.255.0
undo shutdown
quit
```

### 5. DNS ve NTP Ayarları

```bash
# DNS server ayarla
dns server 8.8.8.8
dns server 8.8.4.4

# NTP server ayarla
ntp-service unicast-server 0.pool.ntp.org
ntp-service unicast-server 1.pool.ntp.org
```

### 6. Information Center Ayarları

```bash
# Information centerı etkinleştir
info-center enable
info-center console channel
info-center console level informational
info-center loghost 10.0.0.100
```

### 7. Konfigürasyonu Kaydetme

```bash
# System viewdan çık
quit

# Konfigürasyonu kaydet
save
# veya
save configuration
```

## ✅ Verification Komutları

### Temel Kontroller

```bash
# Hostnamei kontrol et
display current-configuration | include sysname

# Bannerları kontrol et
display current-configuration | include header

# Interface durumlarını kontrol et
display ip interface brief

# SSH durumunu kontrol et
display ssh server status

# Konfigürasyonu kontrol et
display current-configuration
```

### Güvenlik Kontrolleri

```bash
# Super passwordü kontrol et
display current-configuration | include super password

# User-interface konfigürasyonlarını kontrol et
display current-configuration | section user-interface

# Interface güvenlik ayarlarını kontrol et
display interface GigabitEthernet0/0/0
display interface GigabitEthernet0/0/1
```

## 🔍 Troubleshooting

### Yaygın Sorunlar ve Çözümleri

#### 1. Interface Down Durumu
```bash
# Interfacei kontrol et
display interface GigabitEthernet0/0/0

# Interfacei reset et
system-view
interface GigabitEthernet0/0/0
shutdown
undo shutdown
quit
```

#### 2. SSH Bağlantı Sorunu
```bash
# SSH durumunu kontrol et
display ssh server status

# SSH debugını başlat
debugging ssh server
# Sorun çözüldükten sonra durdur
undo debugging ssh server
```

#### 3. Password Hatırlama
```bash
# Super passwordü değiştir
system-view
super password YeniSifre123#
quit
save
```

## 📊 Test Senaryoları

### Test 1: Console Erişimi
```bash
# Routera console ile bağlan
# Username/password ile giriş yap
# System viewa geç
system-view
# Password: DEF123#
```

### Test 2: Telnet Erişimi
```bash
# Başka bir cihazdan telnet ile bağlan
telnet 192.168.1.1
# Password: DEF123#
```

### Test 3: SSH Erişimi
```bash
# SSH ile bağlan
ssh -l admin 192.168.1.1
# Password: DEF123#
```

## 🌟 Huawei-Specific Features

### iStack Konfigürasyonu (Opsiyonel)
```bash
# iStack portları ayarla
system-view
stack-port GigabitEthernet0/0/1 enable
stack-port GigabitEthernet0/0/2 enable
quit
```

### VRRP Konfigürasyonu (Opsiyonel)
```bash
# VRRP konfigürasyonu
system-view
interface GigabitEthernet0/0/1
vrrp vrid 1 virtual-ip 10.0.0.254
vrrp vrid 1 priority 120
vrrp vrid 1 preempt-mode timer delay 20
quit
```

## ⚠️ Önemli Notlar

1. **Güvenlik**: Güçlü şifreler kullanın
2. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
3. **Documentation**: Yapılan değişiklikleri dokümante edin
4. **Testing**: Productiona almadan önce test edin
5. **Monitoring**: Log dosyalarını düzenli kontrol edin
6. **Undo Commands**: Huaweide `undo` komutu ile geri alma yapılır

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- Huawei router temel konfigürasyonu
- Güvenlik ayarları (SSH, Telnet, Console)
- Interface konfigürasyonu
- Banner ve hostname ayarları
- Display komutları
- Undo komutları
- Troubleshooting teknikleri 