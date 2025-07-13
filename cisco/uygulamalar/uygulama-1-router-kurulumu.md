# Uygulama 1: Yeni Router Kurulumu ve Temel Konfigürasyon

## 🎯 Senaryo

ABC Şirketi yeni bir ofis açıyor ve yeni bir Cisco router kurulumu yapılması gerekiyor. Routerın temel konfigürasyonu, güvenlik ayarları ve network bağlantısı kurulacak.

## 📋 Gereksinimler

1. Router hostnamei "ABC-Router-01" olarak ayarlanacak
2. Console, Telnet ve SSH erişimi konfigüre edilecek
3. Interfaceler IP address alacak
4. Güvenlik ayarları yapılacak
5. Banner mesajları eklenmeli
6. Konfigürasyon kaydedilmeli

## 🌐 Topoloji

```
[Internet] --- [ABC-Router-01] --- [LAN Network]
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

# User modedan privileged modea geç
enable

# Global configuration modea geç
configure terminal
# veya
conf t
```

### 2. Hostname ve Banner Ayarları

```bash
# Hostnamei ayarla
hostname ABC-Router-01

# Banner mesajları ekle
banner login #
===========================================
    ABC Şirketi Network Cihazı
    Yetkisiz erişim yasaktır#
    Tarih: $(date)
===========================================
#

banner motd #
Sistem bakımı 22:00-02:00 arası yapılacaktır
#
```

### 3. Güvenlik Ayarları

```bash
# Enable password ayarla (encrypted)
enable secret ABC123#

# Console password ayarla
line console 0
password ABC123#
login
exit

# Telnet password ayarla
line vty 0 4
password ABC123#
login
exit

# SSH konfigürasyonu
ip domain-name abc.com
crypto key generate rsa
# 1024 bit RSA key oluştur
# Enter tuşuna bas

# SSH versiyon 2yi etkinleştir
ip ssh version 2

# SSH timeout ayarla
ip ssh time-out 60
ip ssh authentication-retries 3
```

### 4. Interface Konfigürasyonu

```bash
# WAN interfaceini konfigüre et
interface GigabitEthernet0/0/0
description WAN Interface - Internet Connection
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

# LAN interfaceini konfigüre et
interface GigabitEthernet0/0/1
description LAN Interface - Office Network
ip address 10.0.0.1 255.255.255.0
no shutdown
exit
```

### 5. DNS ve NTP Ayarları

```bash
# DNS server ayarla
ip name-server 8.8.8.8
ip name-server 8.8.4.4

# NTP server ayarla
ntp server 0.pool.ntp.org
ntp server 1.pool.ntp.org
```

### 6. Logging Ayarları

```bash
# Logging ayarları
logging buffered 16384
logging console informational
logging host 10.0.0.100
```

### 7. Konfigürasyonu Kaydetme

```bash
# Global config modedan çık
exit

# Konfigürasyonu kaydet
write memory
# veya
copy running-config startup-config
```

## ✅ Verification Komutları

### Temel Kontroller

```bash
# Hostnamei kontrol et
show running-config | include hostname

# Bannerları kontrol et
show running-config | include banner

# Interface durumlarını kontrol et
show ip interface brief

# SSH durumunu kontrol et
show ip ssh

# Konfigürasyonu kontrol et
show running-config
```

### Güvenlik Kontrolleri

```bash
# Enable passwordü kontrol et
show running-config | include enable secret

# Line konfigürasyonlarını kontrol et
show running-config | section line

# Interface güvenlik ayarlarını kontrol et
show interface GigabitEthernet0/0/0
show interface GigabitEthernet0/0/1
```

## 🔍 Troubleshooting

### Yaygın Sorunlar ve Çözümleri

#### 1. Interface Down Durumu
```bash
# Interfacei kontrol et
show interface GigabitEthernet0/0/0

# Interfacei reset et
configure terminal
interface GigabitEthernet0/0/0
shutdown
no shutdown
exit
```

#### 2. SSH Bağlantı Sorunu
```bash
# SSH durumunu kontrol et
show ip ssh

# SSH debugını başlat
debug ip ssh
# Sorun çözüldükten sonra durdur
no debug ip ssh
```

#### 3. Password Hatırlama
```bash
# Enable passwordü değiştir
configure terminal
enable secret YeniSifre123#
exit
write memory
```

## 📊 Test Senaryoları

### Test 1: Console Erişimi
```bash
# Routera console ile bağlan
# Username/password ile giriş yap
# Privileged modea geç
enable
# Password: ABC123#
```

### Test 2: Telnet Erişimi
```bash
# Başka bir cihazdan telnet ile bağlan
telnet 192.168.1.1
# Password: ABC123#
```

### Test 3: SSH Erişimi
```bash
# SSH ile bağlan
ssh -l admin 192.168.1.1
# Password: ABC123#
```

## ⚠️ Önemli Notlar

1. **Güvenlik**: Güçlü şifreler kullanın
2. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
3. **Documentation**: Yapılan değişiklikleri dokümante edin
4. **Testing**: Productiona almadan önce test edin
5. **Monitoring**: Log dosyalarını düzenli kontrol edin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- Router temel konfigürasyonu
- Güvenlik ayarları (SSH, Telnet, Console)
- Interface konfigürasyonu
- Banner ve hostname ayarları
- Verification komutları
- Troubleshooting teknikleri 