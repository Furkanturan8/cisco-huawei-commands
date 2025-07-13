# Uygulama 5: EIGRP Routing Konfigürasyonu

## 🎯 Senaryo

ABC Şirketinin network altyapısında EIGRP routing protokolü kullanılacak. Farklı AS numaraları kullanılacak, route filtering uygulanacak ve network optimizasyonu sağlanacak.

## 📋 Gereksinimler

1. **AS 100**: Core network (EIGRP 100)
2. **AS 200**: Branch network (EIGRP 200)
3. **AS 300**: Remote network (EIGRP 300)
4. Route redistribution konfigüre edilecek
5. EIGRP authentication uygulanacak
6. EIGRP timers optimize edilecek
7. Stub routing konfigüre edilecek

## 🌐 Topoloji

```
[Core Router AS100] --- [Branch Router AS200] --- [Remote Router AS300]
       |                        |                        |
       |                    [Branch Network]         [Remote Network]
       |
[Core Network]
```

## 🔧 Adım Adım Çözüm

### 1. Core Router EIGRP Konfigürasyonu

```bash
# Core Routera bağlan
enable
configure terminal

# EIGRP AS 100 başlat
router eigrp 100

# Networkleri tanımla
network 192.168.1.0 0.0.0.255
network 192.168.2.0 0.0.0.255
network 10.0.0.0 0.255.255.255

# EIGRP authentication
authentication mode md5
authentication key-chain EIGRP-KEY

# EIGRP timers optimize et
timers active-time 3
timers nsf converge-time 120

# Route redistribution
redistribute static
redistribute connected

# Default route oluştur
network 0.0.0.0

# Stub routing
eigrp stub connected summary

exit
```

### 2. Branch Router EIGRP Konfigürasyonu

```bash
# Branch Routera bağlan
enable
configure terminal

# EIGRP AS 200 başlat
router eigrp 200

# Networkleri tanımla
network 192.168.10.0 0.0.0.255
network 192.168.20.0 0.0.0.255
network 192.168.1.0 0.0.0.255

# EIGRP authentication
authentication mode md5
authentication key-chain EIGRP-KEY

# EIGRP timers
timers active-time 3
timers nsf converge-time 120

# Route redistribution
redistribute static
redistribute connected

# Stub routing
eigrp stub connected summary

exit
```

### 3. Remote Router EIGRP Konfigürasyonu

```bash
# Remote Routera bağlan
enable
configure terminal

# EIGRP AS 300 başlat
router eigrp 300

# Networkleri tanımla
network 192.168.30.0 0.0.0.255
network 192.168.40.0 0.0.0.255
network 192.168.20.0 0.0.0.255

# EIGRP authentication
authentication mode md5
authentication key-chain EIGRP-KEY

# EIGRP timers
timers active-time 3
timers nsf converge-time 120

# Route redistribution
redistribute static
redistribute connected

# Stub routing
eigrp stub connected summary

exit
```

### 4. EIGRP Authentication

```bash
# Key chain oluştur
key chain EIGRP-KEY
key 1
key-string ABC123#
accept-lifetime 00:00:00 Jan 1 2023 infinite
send-lifetime 00:00:00 Jan 1 2023 infinite
exit
exit

# Interfacelerde authentication
interface GigabitEthernet0/0/0
description Link to Branch Router
ip authentication mode eigrp 100 md5
ip authentication key-chain eigrp 100 EIGRP-KEY
no shutdown
exit

interface GigabitEthernet0/0/1
description Link to Remote Router
ip authentication mode eigrp 200 md5
ip authentication key-chain eigrp 200 EIGRP-KEY
no shutdown
exit
```

### 5. EIGRP Route Filtering

```bash
# Access list oluştur
access-list 100 permit ip 192.168.10.0 0.0.0.255 any
access-list 100 permit ip 192.168.20.0 0.0.0.255 any
access-list 100 deny ip any any

# Distribute list uygula
router eigrp 100
distribute-list 100 out GigabitEthernet0/0/0
exit

# Prefix list oluştur
ip prefix-list EIGRP-FILTER permit 192.168.10.0/24
ip prefix-list EIGRP-FILTER permit 192.168.20.0/24
ip prefix-list EIGRP-FILTER deny 0.0.0.0/0 le 32

# Prefix list uygula
router eigrp 100
distribute-list prefix EIGRP-FILTER out GigabitEthernet0/0/0
exit
```

### 6. EIGRP Timers ve Optimization

```bash
# Core Routerda EIGRP timers
router eigrp 100
timers active-time 3
timers nsf converge-time 120
timers nsf signal-time 20
timers nsf converge-time 120
exit

# Branch Routerda EIGRP timers
router eigrp 200
timers active-time 3
timers nsf converge-time 120
timers nsf signal-time 20
timers nsf converge-time 120
exit

# Remote Routerda EIGRP timers
router eigrp 300
timers active-time 3
timers nsf converge-time 120
timers nsf signal-time 20
timers nsf converge-time 120
exit
```

### 7. EIGRP Stub Routing

```bash
# Core Routerda stub routing
router eigrp 100
eigrp stub connected summary
exit

# Branch Routerda stub routing
router eigrp 200
eigrp stub connected summary
exit

# Remote Routerda stub routing
router eigrp 300
eigrp stub connected summary
exit
```

### 8. EIGRP Route Summarization

```bash
# Core Routerda route summarization
interface GigabitEthernet0/0/0
ip summary-address eigrp 100 192.168.10.0 255.255.240.0
exit

interface GigabitEthernet0/0/1
ip summary-address eigrp 100 192.168.30.0 255.255.240.0
exit

# Branch Routerda route summarization
interface GigabitEthernet0/0/0
ip summary-address eigrp 200 192.168.30.0 255.255.240.0
exit
```

### 9. Konfigürasyonu Kaydetme

```bash
# Tüm routerlarda konfigürasyonu kaydet
write memory
```

## ✅ Verification Komutları

### EIGRP Temel Kontrolleri

```bash
# EIGRP neighborsları göster
show ip eigrp neighbors

# EIGRP topologyyi göster
show ip eigrp topology

# EIGRP interfacelerini göster
show ip eigrp interfaces

# EIGRP konfigürasyonunu göster
show ip protocols
```

### EIGRP Route Kontrolleri

```bash
# EIGRP routelarını göster
show ip route eigrp

# EIGRP topology tableını göster
show ip eigrp topology all-links

# EIGRP summaryyi göster
show ip eigrp summary
```

### EIGRP Authentication Kontrolleri

```bash
# Authentication durumunu kontrol et
show ip eigrp neighbors | include authentication

# Key chainleri göster
show key chain
```

### EIGRP Stub Kontrolleri

```bash
# Stub routing durumunu kontrol et
show ip eigrp neighbors | include stub

# Stub konfigürasyonunu göster
show running-config | include eigrp stub
```

## 🔍 Troubleshooting

### Yaygın EIGRP Sorunları

#### 1. EIGRP Neighbor Sorunu
```bash
# Neighbor durumunu kontrol et
show ip eigrp neighbors

# EIGRP debugını başlat
debug ip eigrp
# Sorun çözüldükten sonra durdur
no debug ip eigrp
```

#### 2. EIGRP Authentication Sorunu
```bash
# Authentication durumunu kontrol et
show ip eigrp neighbors | include authentication

# Authenticationı geçici olarak kapat
configure terminal
interface GigabitEthernet0/0/0
no ip authentication mode eigrp 100 md5
exit
```

#### 3. EIGRP Route Sorunu
```bash
# Routeları kontrol et
show ip route eigrp

# EIGRP topologyyi kontrol et
show ip eigrp topology

# Route redistributionı kontrol et
show ip eigrp topology | include external
```

## 📊 Test Senaryoları

### Test 1: EIGRP Neighbor Connectivity
```bash
# Neighborları kontrol et
show ip eigrp neighbors

# Ping testi yap
ping 192.168.1.2
ping 192.168.2.2
ping 192.168.10.1
```

### Test 2: EIGRP Route Advertisement
```bash
# Route advertisementları kontrol et
show ip eigrp topology

# External routeları kontrol et
show ip eigrp topology | include external
```

### Test 3: EIGRP Stub Routing
```bash
# Stub routing durumunu kontrol et
show ip eigrp neighbors | include stub

# Stub routeları kontrol et
show ip route eigrp
```

## 🔐 Güvenlik Kontrolleri

### EIGRP Authentication

```bash
# Authentication ayarlarını kontrol et
show ip eigrp neighbors | include authentication

# Key chainleri kontrol et
show key chain
```

### Route Filtering

```bash
# Distribute listleri kontrol et
show running-config | include distribute-list

# Prefix listleri kontrol et
show ip prefix-list
```

## ⚠️ Önemli Notlar

1. **AS Planning**: EIGRP AS numaralarını planlayın
2. **Authentication**: Production ortamında authentication kullanın
3. **Route Filtering**: Route filtering uygulayın
4. **Timers**: EIGRP timersı optimize edin
5. **Documentation**: EIGRP konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Stub Routing**: Stub routing kullanın

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- EIGRP protokolü ve konfigürasyonu
- EIGRP authentication
- Route filtering ve prefix lists
- EIGRP stub routing
- Route summarization
- EIGRP timers optimization
- EIGRP troubleshooting teknikleri 