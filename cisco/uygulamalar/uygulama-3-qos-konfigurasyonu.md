# Uygulama 3: QoS Konfigürasyonu

## 🎯 Senaryo

ABC Şirketinin network altyapısında QoS (Quality of Service) konfigürasyonu yapılması gerekiyor. Voice, video ve data trafiği için farklı priorityler ayarlanacak, bandwidth yönetimi yapılacak ve congestion management uygulanacak.

## 📋 Gereksinimler

1. **Voice Traffic**: Priority 5 (EF - Expedited Forwarding)
2. **Video Traffic**: Priority 4 (AF - Assured Forwarding)
3. **Data Traffic**: Priority 0 (BE - Best Effort)
4. **Management Traffic**: Priority 6 (CS6 - Network Control)
5. Bandwidth allocation ve policing
6. Congestion management (WRED)
7. Traffic shaping ve queuing

## 🌐 Topoloji

```
[Voice Phone] --- [Switch] --- [Router] --- [Internet]
[Video Camera]     |            |
[Data PC]          |        [QoS Policy]
[Management PC]     |
```

## 🔧 Adım Adım Çözüm

### 1. Class Map Oluşturma

```bash
# Switche bağlan
enable
configure terminal

# Voice traffic için class map
class-map match-all VOICE-TRAFFIC
match dscp ef
match access-group 100
exit

# Video traffic için class map
class-map match-all VIDEO-TRAFFIC
match dscp af41
match access-group 101
exit

# Data traffic için class map
class-map match-all DATA-TRAFFIC
match dscp default
match access-group 102
exit

# Management traffic için class map
class-map match-all MANAGEMENT-TRAFFIC
match dscp cs6
match access-group 103
exit
```

### 2. Access List Oluşturma

```bash
# Voice traffic access list
access-list 100 permit udp any any range 16384 32767
access-list 100 permit tcp any any eq 5060
access-list 100 permit tcp any any eq 5061

# Video traffic access list
access-list 101 permit udp any any range 32768 65535
access-list 101 permit tcp any any eq 554
access-list 101 permit tcp any any eq 1935

# Data traffic access list
access-list 102 permit ip any any

# Management traffic access list
access-list 103 permit tcp any any eq 22
access-list 103 permit tcp any any eq 23
access-list 103 permit tcp any any eq 80
access-list 103 permit tcp any any eq 443
```

### 3. Policy Map Oluşturma

```bash
# Policy map oluştur
policy-map QOS-POLICY

# Voice traffic class
class VOICE-TRAFFIC
priority percent 20
police cir 1000000
police pir 1500000
conform-action transmit
exceed-action drop
violate-action drop
exit

# Video traffic class
class VIDEO-TRAFFIC
bandwidth percent 30
queue-limit 100 packets
random-detect dscp-based
random-detect dscp af41 50 100 20
exit

# Management traffic class
class MANAGEMENT-TRAFFIC
priority percent 10
police cir 500000
police pir 750000
conform-action transmit
exceed-action drop
violate-action drop
exit

# Data traffic class
class DATA-TRAFFIC
bandwidth percent 40
queue-limit 200 packets
random-detect
random-detect precedence 0 50 100 20
exit

# Default class
class class-default
bandwidth percent 10
queue-limit 50 packets
exit

exit
```

### 4. Interface QoS Konfigürasyonu

```bash
# Uplink interfacede QoS uygula
interface GigabitEthernet0/0/1
description Uplink to Router
service-policy output QOS-POLICY
exit

# Access portlarda QoS ayarları
interface FastEthernet0/1
description Voice Phone Port
switchport voice vlan 20
mls qos trust cos
mls qos trust dscp
exit

interface FastEthernet0/2
description Video Camera Port
switchport access vlan 30
mls qos trust cos
mls qos trust dscp
exit

interface FastEthernet0/3
description Data PC Port
switchport access vlan 10
mls qos trust cos
exit
```

### 5. DSCP Marking

```bash
# DSCP marking policy map
policy-map DSCP-MARKING

# Voice traffic marking
class VOICE-TRAFFIC
set dscp ef
exit

# Video traffic marking
class VIDEO-TRAFFIC
set dscp af41
exit

# Management traffic marking
class MANAGEMENT-TRAFFIC
set dscp cs6
exit

# Data traffic marking
class DATA-TRAFFIC
set dscp default
exit

exit

# Interfacede DSCP marking uygula
interface GigabitEthernet0/0/1
service-policy input DSCP-MARKING
exit
```

### 6. WRED (Weighted Random Early Detection)

```bash
# WRED konfigürasyonu
policy-map WRED-POLICY

class VOICE-TRAFFIC
random-detect dscp-based
random-detect dscp ef 50 100 20
exit

class VIDEO-TRAFFIC
random-detect dscp-based
random-detect dscp af41 50 100 20
random-detect dscp af42 50 100 20
exit

class DATA-TRAFFIC
random-detect
random-detect precedence 0 50 100 20
random-detect precedence 1 50 100 20
exit

exit
```

### 7. Traffic Shaping

```bash
# Traffic shaping policy map
policy-map SHAPING-POLICY

class VOICE-TRAFFIC
shape average 1000000
exit

class VIDEO-TRAFFIC
shape average 2000000
exit

class DATA-TRAFFIC
shape average 5000000
exit

exit

# Interfacede traffic shaping uygula
interface GigabitEthernet0/0/1
service-policy output SHAPING-POLICY
exit
```

### 8. Auto QoS

```bash
# Auto QoS konfigürasyonu
interface FastEthernet0/1
auto qos voip trust
exit

interface FastEthernet0/2
auto qos video
exit

interface FastEthernet0/3
auto qos trust
exit
```

### 9. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
write memory
```

## ✅ Verification Komutları

### QoS Temel Kontrolleri

```bash
# Policy mapleri göster
show policy-map

# Interface policylerini göster
show policy-map interface

# QoS statisticslerini göster
show mls qos statistics

# DSCP mappingi göster
show mls qos maps dscp-cos
```

### QoS Performance Kontrolleri

```bash
# Queue statisticslerini göster
show policy-map interface GigabitEthernet0/0/1 output

# Policing statisticslerini göster
show policy-map interface | include police

# WRED statisticslerini göster
show policy-map interface | include random-detect
```

### QoS Marking Kontrolleri

```bash
# DSCP markingi kontrol et
show policy-map interface | include set

# CoS markingi kontrol et
show mls qos interface FastEthernet0/1
```

### QoS Monitoring

```bash
# QoS countersları göster
show mls qos interface statistics

# QoS dropsları göster
show policy-map interface | include drop

# QoS conform/exceed/violate
show policy-map interface | include conform
```

## 🔍 Troubleshooting

### Yaygın QoS Sorunları

#### 1. QoS Policy Uygulanmıyor
```bash
# Policy map durumunu kontrol et
show policy-map

# Interface policysini kontrol et
show policy-map interface GigabitEthernet0/0/1

# QoS trust ayarlarını kontrol et
show mls qos interface
```

#### 2. Voice Quality Sorunu
```bash
# Voice traffic classını kontrol et
show policy-map class-map VOICE-TRAFFIC

# Voice priority ayarlarını kontrol et
show policy-map interface | include priority

# Voice policing ayarlarını kontrol et
show policy-map interface | include police
```

#### 3. Bandwidth Sorunu
```bash
# Bandwidth allocationı kontrol et
show policy-map interface | include bandwidth

# Queue limitsi kontrol et
show policy-map interface | include queue-limit

# Traffic shapingi kontrol et
show policy-map interface | include shape
```

## 📊 Test Senaryoları

### Test 1: Voice QoS
```bash
# Voice traffic test et
ping 192.168.20.1
# DSCP markingi kontrol et
show mls qos interface FastEthernet0/1
```

### Test 2: Video QoS
```bash
# Video traffic test et
ping 192.168.30.1
# Video bandwidth kullanımını kontrol et
show policy-map interface | include VIDEO-TRAFFIC
```

### Test 3: Data QoS
```bash
# Data traffic test et
ping 192.168.10.1
# Data queue durumunu kontrol et
show policy-map interface | include DATA-TRAFFIC
```

## 🔐 Güvenlik Kontrolleri

### QoS Security

```bash
# QoS trust ayarlarını kontrol et
show mls qos interface | include trust

# QoS markingi kontrol et
show mls qos maps dscp-cos
```

### Traffic Control

```bash
# Policing ayarlarını kontrol et
show policy-map interface | include police

# Shaping ayarlarını kontrol et
show policy-map interface | include shape
```

## ⚠️ Önemli Notlar

1. **Traffic Classification**: Traffici doğru sınıflandırın
2. **Bandwidth Planning**: Bandwidthi planlayın
3. **Priority Settings**: Priorityleri doğru ayarlayın
4. **Monitoring**: QoS performanceını sürekli izleyin
5. **Documentation**: QoS konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Testing**: QoS ayarlarını test edin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- QoS traffic classification
- Policy map ve class map konfigürasyonu
- DSCP ve CoS marking
- Traffic policing ve shaping
- WRED (Weighted Random Early Detection)
- Auto QoS konfigürasyonu
- QoS troubleshooting teknikleri 