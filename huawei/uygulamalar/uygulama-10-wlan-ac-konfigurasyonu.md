# Uygulama 10: WLAN AC Konfigürasyonu

## 🎯 Senaryo

DEF Şirketinin network altyapısında WLAN AC (Access Controller) konfigürasyonu yapılması gerekiyor. Wireless network yönetimi, AP konfigürasyonu, security ayarları ve roaming yapılacak.

## 📋 Gereksinimler

1. **AC Controller**: WLAN yönetimi için
2. **AP (Access Point)**: Wireless coverage için
3. **SSID**: Wireless network adları
4. **Security**: WPA2/WPA3 authentication
5. **VLAN**: Wireless VLAN yönetimi
6. **Roaming**: Seamless roaming
7. **QoS**: Wireless QoS ayarları

## 🌐 Topoloji

```
[AC Controller] --- [Switch] --- [AP-1] --- [Wireless Clients]
       |              |              |
       |          [AP-2] --- [Wireless Clients]
       |              |
       |          [AP-3] --- [Wireless Clients]
       |
[Management Network]
```

## 🔧 Adım Adım Çözüm

### 1. AC Controller Temel Konfigürasyonu

```bash
# AC Controllera bağlan
system-view

# AC hostname ayarla
sysname DEF-AC-01

# AC IP address ayarla
interface Vlanif1
description Management VLAN
ip address 192.168.1.10 255.255.255.0
undo shutdown
quit

# AC CAPWAP interface ayarla
interface Vlanif100
description CAPWAP VLAN
ip address 192.168.100.1 255.255.255.0
undo shutdown
quit
```

### 2. AP Registration Konfigürasyonu

```bash
# AP registration ayarları
wlan ac

# AP template oluştur
ap-group name default
quit

# AP registration method
ap-register-mode mac-auth

# AP authentication
ap-authentication-mode mac-auth

# AP discovery
ap-discovery-mode broadcast

quit
```

### 3. SSID ve Wireless Network Konfigürasyonu

```bash
# SSID oluştur
wlan
ssid-profile name DEF-STAFF
ssid DEF-STAFF
quit

ssid-profile name DEF-GUEST
ssid DEF-GUEST
quit

# Security profile oluştur
security-profile name DEF-STAFF-SEC
security wpa2 psk pass-phrase DEF123# aes
quit

security-profile name DEF-GUEST-SEC
security open
quit

# VAP profile oluştur
vap-profile name DEF-STAFF-VAP
ssid-profile DEF-STAFF
security-profile DEF-STAFF-SEC
service-vlan vlan-id 10
quit

vap-profile name DEF-GUEST-VAP
ssid-profile DEF-GUEST
security-profile DEF-GUEST-SEC
service-vlan vlan-id 20
quit
```

### 4. AP Group ve Radio Konfigürasyonu

```bash
# AP group konfigürasyonu
ap-group name default

# 2.4GHz radio ayarları
radio 0
radio-profile name DEF-2.4G
vap-profile DEF-STAFF-VAP wlan 1
vap-profile DEF-GUEST-VAP wlan 2
quit

# 5GHz radio ayarları
radio 1
radio-profile name DEF-5G
vap-profile DEF-STAFF-VAP wlan 1
vap-profile DEF-GUEST-VAP wlan 2
quit

quit
```

### 5. Radio Profile Konfigürasyonu

```bash
# 2.4GHz radio profile
radio-profile name DEF-2.4G
channel 1 6 11
power auto
quit

# 5GHz radio profile
radio-profile name DEF-5G
channel 36 40 44 48
power auto
quit
```

### 6. AP Konfigürasyonu

```bash
# AP template oluştur
ap-template name DEF-AP-TEMPLATE
ap-group default
quit

# APleri kaydet
ap-id 1 ap-mac 00e0-fc12-3456
ap-name AP-01
ap-group default
quit

ap-id 2 ap-mac 00e0-fc12-3457
ap-name AP-02
ap-group default
quit

ap-id 3 ap-mac 00e0-fc12-3458
ap-name AP-03
ap-group default
quit
```

### 7. Wireless Security Konfigürasyonu

```bash
# WPA2 security ayarları
security-profile name DEF-STAFF-SEC
security wpa2 psk pass-phrase DEF123# aes
security wpa2 psk pass-phrase DEF123# tkip
quit

# Guest network security
security-profile name DEF-GUEST-SEC
security open
mac-filter whitelist
quit

# MAC filtering
mac-filter whitelist
mac-address 00:11:22:33:44:55
mac-address 00:11:22:33:44:56
quit
```

### 8. Wireless QoS Konfigürasyonu

```bash
# QoS profile oluştur
qos-profile name DEF-WIRELESS-QOS
user-priority 0 queue 0
user-priority 1 queue 1
user-priority 2 queue 2
user-priority 3 queue 3
user-priority 4 queue 4
user-priority 5 queue 5
user-priority 6 queue 6
user-priority 7 queue 7
quit

# VAP profilea QoS uygula
vap-profile name DEF-STAFF-VAP
qos-profile DEF-WIRELESS-QOS
quit
```

### 9. Roaming Konfigürasyonu

```bash
# Roaming ayarları
wlan ac
roaming enable
roaming-mode seamless
quit

# Fast roaming
wlan ac
fast-roaming enable
quit
```

### 10. Wireless Monitoring

```bash
# Wireless monitoring ayarları
info-center enable
info-center console channel
info-center console level informational

# Wireless log ayarları
wlan ac
log enable
quit
```

### 11. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
save
```

## ✅ Verification Komutları

### WLAN AC Temel Kontrolleri

```bash
# AC durumunu göster
display wlan ac

# AP listesini göster
display ap all

# SSID listesini göster
display ssid-profile all

# VAP listesini göster
display vap-profile all
```

### AP Kontrolleri

```bash
# AP durumunu göster
display ap all

# AP radio durumunu göster
display ap radio all

# AP clientlarını göster
display ap client all
```

### Wireless Security Kontrolleri

```bash
# Security profileları göster
display security-profile all

# MAC filter listesini göster
display mac-filter whitelist

# Wireless clientları göster
display wlan client all
```

### Wireless Performance Kontrolleri

```bash
# Wireless statisticslerini göster
display wlan statistics

# Radio statisticslerini göster
display radio statistics

# QoS statisticslerini göster
display qos statistics
```

## 🔍 Troubleshooting

### Yaygın WLAN AC Sorunları

#### 1. AP Registration Sorunu
```bash
# AP durumunu kontrol et
display ap all

# AP registration ayarlarını kontrol et
display wlan ac

# AP discovery ayarlarını kontrol et
display ap-discovery-mode
```

#### 2. Wireless Connectivity Sorunu
```bash
# SSID durumunu kontrol et
display ssid-profile all

# VAP durumunu kontrol et
display vap-profile all

# Security ayarlarını kontrol et
display security-profile all
```

#### 3. Roaming Sorunu
```bash
# Roaming durumunu kontrol et
display wlan ac | include roaming

# Fast roaming durumunu kontrol et
display wlan ac | include fast-roaming
```

## 📊 Test Senaryoları

### Test 1: Wireless Connectivity
```bash
# Wireless client bağlantısını test et
# Clientı SSIDye bağla
# Ping testi yap
ping 192.168.10.1
```

### Test 2: AP Roaming
```bash
# Clientı farklı APlere taşı
# Roaming durumunu kontrol et
display wlan client all
```

### Test 3: Wireless Security
```bash
# Security authentication test et
# WPA2 password ile bağlan
# MAC filtering test et
display mac-filter whitelist
```

## 🔐 Güvenlik Kontrolleri

### Wireless Security

```bash
# Security profileları kontrol et
display security-profile all

# MAC filtering ayarlarını kontrol et
display mac-filter whitelist
```

### Wireless Monitoring

```bash
# Wireless monitoring ayarlarını kontrol et
display wlan statistics

# Wireless log ayarlarını kontrol et
display info-center
```

## ⚠️ Önemli Notlar

1. **AP Planning**: AP yerleşimini planlayın
2. **Channel Planning**: Channel çakışmalarını önleyin
3. **Security**: Wireless security kullanın
4. **Monitoring**: Wireless performanceını sürekli izleyin
5. **Documentation**: WLAN konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Testing**: Wireless networkü test edin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- WLAN AC konfigürasyonu
- AP registration ve yönetimi
- SSID ve VAP konfigürasyonu
- Wireless security ayarları
- Wireless QoS konfigürasyonu
- Roaming konfigürasyonu
- Wireless troubleshooting teknikleri 