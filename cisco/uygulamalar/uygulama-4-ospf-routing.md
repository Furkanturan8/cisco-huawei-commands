# Uygulama 4: OSPF Routing Konfigürasyonu

## 🎯 Senaryo

ABC Şirketinin network altyapısında OSPF routing protokolü kullanılacak. Farklı arealar oluşturulacak, route redistribution yapılacak ve network optimizasyonu sağlanacak.

## 📋 Gereksinimler

1. **Area 0**: Backbone area (Core network)
2. **Area 1**: IT Departmanı (Stub area)
3. **Area 2**: Muhasebe Departmanı (Stub area)
4. **Area 3**: Satış Departmanı (NSSA area)
5. Route redistribution konfigüre edilecek
6. OSPF authentication uygulanacak
7. OSPF timers optimize edilecek

## 🌐 Topoloji

```
[Core Router] --- [IT Router] --- [IT Network]
      |                |
      |            [Muhasebe Router] --- [Muhasebe Network]
      |
[Satış Router] --- [Satış Network]
      |
[Internet Router]
```

## 🔧 Adım Adım Çözüm

### 1. Core Router OSPF Konfigürasyonu

```bash
# Core Routera bağlan
enable
configure terminal

# OSPF process 1 başlat
router ospf 1

# Area 0 (Backbone) konfigürasyonu
network 192.168.1.0 0.0.0.255 area 0
network 192.168.2.0 0.0.0.255 area 0
network 192.168.3.0 0.0.0.255 area 0

# OSPF authentication
area 0 authentication message-digest

# OSPF timers optimize et
timers throttle spf 10 100 5000
timers throttle lsa all 10 100 5000

# Route redistribution
redistribute static subnets
redistribute connected subnets

# Default route oluştur
default-information originate always

exit
```

### 2. IT Router OSPF Konfigürasyonu

```bash
# IT Routera bağlan
enable
configure terminal

# OSPF process 1 başlat
router ospf 1

# Area 1 (Stub area) konfigürasyonu
area 1 stub
network 192.168.10.0 0.0.0.255 area 1
network 192.168.1.0 0.0.0.255 area 0

# OSPF authentication
area 1 authentication message-digest

# OSPF timers
timers throttle spf 10 100 5000

exit
```

### 3. Muhasebe Router OSPF Konfigürasyonu

```bash
# Muhasebe Routera bağlan
enable
configure terminal

# OSPF process 1 başlat
router ospf 1

# Area 2 (Stub area) konfigürasyonu
area 2 stub
network 192.168.20.0 0.0.0.255 area 2
network 192.168.2.0 0.0.0.255 area 0

# OSPF authentication
area 2 authentication message-digest

# OSPF timers
timers throttle spf 10 100 5000

exit
```

### 4. Satış Router OSPF Konfigürasyonu

```bash
# Satış Routera bağlan
enable
configure terminal

# OSPF process 1 başlat
router ospf 1

# Area 3 (NSSA area) konfigürasyonu
area 3 nssa
network 192.168.30.0 0.0.0.255 area 3
network 192.168.3.0 0.0.0.255 area 0

# OSPF authentication
area 3 authentication message-digest

# OSPF timers
timers throttle spf 10 100 5000

# NSSA default route
area 3 nssa default-information-originate

exit
```

### 5. Interface OSPF Konfigürasyonu

```bash
# Core Router interface ayarları
interface GigabitEthernet0/0/0
description Link to IT Router
ip ospf hello-interval 10
ip ospf dead-interval 40
ip ospf cost 10
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 ABC123#
no shutdown
exit

interface GigabitEthernet0/0/1
description Link to Muhasebe Router
ip ospf hello-interval 10
ip ospf dead-interval 40
ip ospf cost 10
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 ABC123#
no shutdown
exit

interface GigabitEthernet0/0/2
description Link to Satış Router
ip ospf hello-interval 10
ip ospf dead-interval 40
ip ospf cost 10
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 ABC123#
no shutdown
exit
```

### 6. Route Filtering ve Summarization

```bash
# Core Routerda route summarization
router ospf 1
area 1 range 192.168.10.0 255.255.255.0
area 2 range 192.168.20.0 255.255.255.0
area 3 range 192.168.30.0 255.255.255.0

# Route filtering
distribute-list 10 in GigabitEthernet0/0/0
distribute-list 20 in GigabitEthernet0/0/1
exit

# Access list oluştur
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 deny any

access-list 20 permit 192.168.20.0 0.0.0.255
access-list 20 deny any
```

### 7. OSPF Authentication

```bash
# Tüm routerlarda authentication ayarla
# Core Router
router ospf 1
area 0 authentication message-digest
area 1 authentication message-digest
area 2 authentication message-digest
area 3 authentication message-digest
exit

# Interfacelerde authentication
interface GigabitEthernet0/0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 ABC123#
exit
```

### 8. Konfigürasyonu Kaydetme

```bash
# Tüm routerlarda konfigürasyonu kaydet
write memory
```

## ✅ Verification Komutları

### OSPF Temel Kontrolleri

```bash
# OSPF neighborsları göster
show ip ospf neighbor

# OSPF databaseini göster
show ip ospf database

# OSPF interfacelerini göster
show ip ospf interface

# OSPF konfigürasyonunu göster
show ip protocols
```

### OSPF Route Kontrolleri

```bash
# OSPF routelarını göster
show ip route ospf

# OSPF topologyyi göster
show ip ospf topology

# OSPF summaryyi göster
show ip ospf summary
```

### OSPF Authentication Kontrolleri

```bash
# Authentication durumunu kontrol et
show ip ospf interface | include authentication

# Message digest keyleri göster
show ip ospf interface | include message-digest
```

### OSPF Area Kontrolleri

```bash
# Area bilgilerini göster
show ip ospf area

# Stub areaları kontrol et
show ip ospf database summary

# NSSA areaları kontrol et
show ip ospf database nssa
```

## 🔍 Troubleshooting

### Yaygın OSPF Sorunları

#### 1. OSPF Neighbor Sorunu
```bash
# Neighbor durumunu kontrol et
show ip ospf neighbor

# OSPF debugını başlat
debug ip ospf events
debug ip ospf neighbor
# Sorun çözüldükten sonra durdur
no debug ip ospf events
no debug ip ospf neighbor
```

#### 2. OSPF Authentication Sorunu
```bash
# Authentication durumunu kontrol et
show ip ospf interface | include authentication

# Authenticationı geçici olarak kapat
configure terminal
interface GigabitEthernet0/0/0
no ip ospf authentication
exit
router ospf 1
no area 0 authentication
exit
```

#### 3. OSPF Route Sorunu
```bash
# Routeları kontrol et
show ip route ospf

# OSPF databaseini kontrol et
show ip ospf database

# Route redistributionı kontrol et
show ip ospf database external
```

## 📊 Test Senaryoları

### Test 1: OSPF Neighbor Connectivity
```bash
# Neighborları kontrol et
show ip ospf neighbor

# Ping testi yap
ping 192.168.1.2
ping 192.168.2.2
ping 192.168.3.2
```

### Test 2: Inter-Area Routing
```bash
# ITden muhasebeye ping
ping 192.168.20.1

# Muhasebeden satışa ping
ping 192.168.30.1

# Satıştan ITye ping
ping 192.168.10.1
```

### Test 3: External Route Redistribution
```bash
# External routeları kontrol et
show ip route ospf

# Redistributed routeları kontrol et
show ip ospf database external
```

## 🔐 Güvenlik Kontrolleri

### OSPF Authentication

```bash
# Authentication ayarlarını kontrol et
show ip ospf interface | include authentication

# Message digest keyleri kontrol et
show running-config | include message-digest-key
```

### Route Filtering

```bash
# Distribute-listleri kontrol et
show running-config | include distribute-list

# Access listleri kontrol et
show access-list 10
show access-list 20
```

## ⚠️ Önemli Notlar

1. **Area Planning**: OSPF arealarını planlayın
2. **Authentication**: Production ortamında authentication kullanın
3. **Route Summarization**: Networkü optimize etmek için summarization yapın
4. **Timers**: OSPF timersı optimize edin
5. **Documentation**: OSPF konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- OSPF area yapısı ve konfigürasyonu
- Stub ve NSSA areaları
- OSPF authentication
- Route redistribution
- Route filtering ve summarization
- OSPF timers optimization
- OSPF troubleshooting teknikleri 