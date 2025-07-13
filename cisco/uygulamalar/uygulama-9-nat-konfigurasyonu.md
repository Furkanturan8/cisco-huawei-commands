# Uygulama 9: NAT Konfigürasyonu

## 🎯 Senaryo

ABC Şirketinin network altyapısında NAT (Network Address Translation) konfigürasyonu yapılması gerekiyor. Farklı NAT türleri uygulanacak, port forwarding yapılacak ve network güvenliği sağlanacak.

## 📋 Gereksinimler

1. **Dynamic NAT**: Internal network için
2. **Static NAT**: Serverlar için
3. **PAT (Port Address Translation)**: Internet erişimi için
4. **Port Forwarding**: Web server için
5. **NAT overload**: Multiple internal IPler için
6. **NAT logging**: NAT log kayıtları
7. **NAT optimization**: NAT performans optimizasyonu

## 🌐 Topoloji

```
[Internet] --- [Router NAT] --- [Switch] --- [Internal Network]
                    |              |
                    |          [Web Server]
                    |
                [Mail Server]
```

## 🔧 Adım Adım Çözüm

### 1. Interface Konfigürasyonu

```bash
# Routera bağlan
enable
configure terminal

# Outside interface (Internet)
interface GigabitEthernet0/0/0
description Internet Link
ip address 203.0.113.2 255.255.255.0
ip nat outside
no shutdown
exit

# Inside interface (Internal Network)
interface GigabitEthernet0/0/1
description Internal Network
ip address 192.168.1.1 255.255.255.0
ip nat inside
no shutdown
exit

# DMZ interface (Server Network)
interface GigabitEthernet0/0/2
description DMZ Network
ip address 192.168.100.1 255.255.255.0
ip nat inside
no shutdown
exit
```

### 2. Access List Oluşturma

```bash
# NAT için access list oluştur
access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 permit 192.168.100.0 0.0.0.255

# Serverlar için access list
access-list 2 permit host 192.168.100.10
access-list 2 permit host 192.168.100.20

# Web server için access list
access-list 3 permit host 192.168.100.10
```

### 3. Dynamic NAT Konfigürasyonu

```bash
# NAT pool oluştur
ip nat pool NAT-POOL 203.0.113.10 203.0.113.50 netmask 255.255.255.0

# Dynamic NAT konfigürasyonu
ip nat inside source list 1 pool NAT-POOL

# NAT overload (PAT) konfigürasyonu
ip nat inside source list 1 interface GigabitEthernet0/0/0 overload
```

### 4. Static NAT Konfigürasyonu

```bash
# Web server için static NAT
ip nat inside source static 192.168.100.10 203.0.113.5

# Mail server için static NAT
ip nat inside source static 192.168.100.20 203.0.113.6

# DNS server için static NAT
ip nat inside source static 192.168.100.30 203.0.113.7
```

### 5. Port Forwarding Konfigürasyonu

```bash
# Web server port forwarding
ip nat inside source static tcp 192.168.100.10 80 203.0.113.5 80

# Mail server port forwarding
ip nat inside source static tcp 192.168.100.20 25 203.0.113.6 25
ip nat inside source static tcp 192.168.100.20 110 203.0.113.6 110

# FTP server port forwarding
ip nat inside source static tcp 192.168.100.40 21 203.0.113.8 21
ip nat inside source static tcp 192.168.100.40 20 203.0.113.8 20
```

### 6. NAT Logging ve Monitoring

```bash
# NAT logging ayarları
ip nat log translations syslog

# NAT logging konfigürasyonu
logging console informational
logging buffered 16384
logging host 192.168.1.100
```

### 7. NAT Optimization

```bash
# NAT timeout ayarları
ip nat translation timeout 86400
ip nat translation tcp-timeout 86400
ip nat translation udp-timeout 300
ip nat translation dns-timeout 60
ip nat translation finrst-timeout 60
ip nat translation icmp-timeout 60
ip nat translation syn-timeout 60

# NAT rate limiting
interface GigabitEthernet0/0/0
rate-limit input access-group 1 10000000 1000000 1000000 conform-action transmit exceed-action drop
exit
```

### 8. NAT Security

```bash
# NAT security ayarları
ip nat translation max-entries 10000
ip nat translation max-entries all-host 1000

# NAT access list
access-list 100 permit ip 192.168.1.0 0.0.0.255 any
access-list 100 permit ip 192.168.100.0 0.0.0.255 any
access-list 100 deny ip any any
```

### 9. NAT Verification ve Testing

```bash
# NAT konfigürasyonunu göster
show ip nat translations

# NAT statisticslerini göster
show ip nat statistics

# NAT interfacelerini göster
show ip nat interface
```

### 10. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
write memory
```

## ✅ Verification Komutları

### NAT Temel Kontrolleri

```bash
# NAT translationsları göster
show ip nat translations

# NAT statisticslerini göster
show ip nat statistics

# NAT interfacelerini göster
show ip nat interface

# NAT konfigürasyonunu göster
show running-config | include ip nat
```

### NAT Performance Kontrolleri

```bash
# NAT hit countlarını göster
show ip nat statistics | include hits

# NAT timeout ayarlarını göster
show ip nat statistics | include timeout

# NAT rate limitingi göster
show interface GigabitEthernet0/0/0 | include rate-limit
```

### NAT Logging Kontrolleri

```bash
# NAT log kayıtlarını göster
show logging

# NAT log bufferını göster
show logging buffer

# NAT log hostunu göster
show logging host
```

### NAT Security Kontrolleri

```bash
# NAT security ayarlarını kontrol et
show ip nat statistics | include max-entries

# NAT access listlerini kontrol et
show access-list 100
```

## 🔍 Troubleshooting

### Yaygın NAT Sorunları

#### 1. NAT Translation Sorunu
```bash
# NAT translationsları kontrol et
show ip nat translations

# NAT interfacelerini kontrol et
show ip nat interface

# NAT poolları kontrol et
show running-config | include ip nat pool
```

#### 2. NAT Performance Sorunu
```bash
# NAT statisticslerini kontrol et
show ip nat statistics

# NAT timeout ayarlarını kontrol et
show ip nat statistics | include timeout
```

#### 3. NAT Logging Sorunu
```bash
# NAT logging durumunu kontrol et
show logging

# NAT logging ayarlarını kontrol et
show running-config | include ip nat log
```

## 📊 Test Senaryoları

### Test 1: NAT Connectivity
```bash
# Internaldan externala ping testi
ping 8.8.8.8

# NAT translationı kontrol et
show ip nat translations
```

### Test 2: Port Forwarding
```bash
# Web server port forwarding test et
telnet 203.0.113.5 80

# Mail server port forwarding test et
telnet 203.0.113.6 25
```

### Test 3: NAT Logging
```bash
# NAT log kayıtlarını kontrol et
show logging

# NAT log bufferını kontrol et
show logging buffer
```

## 🔐 Güvenlik Kontrolleri

### NAT Security

```bash
# NAT security ayarlarını kontrol et
show ip nat statistics | include max-entries

# NAT access listlerini kontrol et
show access-list 100
```

### NAT Monitoring

```bash
# NAT monitoring ayarlarını kontrol et
show ip nat statistics

# NAT log ayarlarını kontrol et
show logging
```

## ⚠️ Önemli Notlar

1. **NAT Planning**: NAT stratejisini planlayın
2. **Interface Configuration**: NAT interfacelerini doğru ayarlayın
3. **Logging**: NAT logging kullanın
4. **Optimization**: NATı optimize edin
5. **Documentation**: NAT konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Testing**: NATı test edin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- Dynamic ve Static NAT konfigürasyonu
- PAT (Port Address Translation) kullanımı
- Port forwarding konfigürasyonu
- NAT logging ve monitoring
- NAT optimization ve security
- NAT troubleshooting teknikleri
- NAT best practices 