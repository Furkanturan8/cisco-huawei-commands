# Uygulama 5: IS-IS Routing Konfigürasyonu

## 🎯 Senaryo

DEF Şirketinin network altyapısında IS-IS routing protokolü kullanılacak. Farklı levellar oluşturulacak, route redistribution yapılacak ve network optimizasyonu sağlanacak.

## 📋 Gereksinimler

1. **Level-2**: Backbone network (Core network)
2. **Level-1**: IT Departmanı (Internal routing)
3. **Level-1**: Muhasebe Departmanı (Internal routing)
4. **Level-1**: Satış Departmanı (Internal routing)
5. Route redistribution konfigüre edilecek
6. IS-IS authentication uygulanacak
7. IS-IS timers optimize edilecek

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

### 1. Core Router IS-IS Konfigürasyonu

```bash
# Core Routera bağlan
system-view

# IS-IS process 1 başlat
isis 1

# Level-2 konfigürasyonu
is-level level-2

# Network entity ayarla
network-entity 49.0001.0001.0001.0001.00

# Interfaceleri IS-ISe ekle
interface GigabitEthernet0/0/0
description Link to IT Router
isis enable 1
isis circuit-type level-2
isis authentication-mode md5 ABC123#
quit

interface GigabitEthernet0/0/1
description Link to Muhasebe Router
isis enable 1
isis circuit-type level-2
isis authentication-mode md5 ABC123#
quit

interface GigabitEthernet0/0/2
description Link to Satış Router
isis enable 1
isis circuit-type level-2
isis authentication-mode md5 ABC123#
quit

# Route redistribution
import-route static
import-route direct

# Default route oluştur
default-route-advertise always

quit
```

### 2. IT Router IS-IS Konfigürasyonu

```bash
# IT Routera bağlan
system-view

# IS-IS process 1 başlat
isis 1

# Level-1-2 konfigürasyonu
is-level level-1-2

# Network entity ayarla
network-entity 49.0001.0001.0002.0001.00

# Interfaceleri IS-ISe ekle
interface GigabitEthernet0/0/0
description Link to Core Router
isis enable 1
isis circuit-type level-1-2
isis authentication-mode md5 ABC123#
quit

interface GigabitEthernet0/0/1
description IT Network
isis enable 1
isis circuit-type level-1
quit

# Route redistribution
import-route static
import-route direct

quit
```

### 3. Muhasebe Router IS-IS Konfigürasyonu

```bash
# Muhasebe Routera bağlan
system-view

# IS-IS process 1 başlat
isis 1

# Level-1-2 konfigürasyonu
is-level level-1-2

# Network entity ayarla
network-entity 49.0001.0001.0003.0001.00

# Interfaceleri IS-ISe ekle
interface GigabitEthernet0/0/0
description Link to Core Router
isis enable 1
isis circuit-type level-1-2
isis authentication-mode md5 ABC123#
quit

interface GigabitEthernet0/0/1
description Muhasebe Network
isis enable 1
isis circuit-type level-1
quit

# Route redistribution
import-route static
import-route direct

quit
```

### 4. Satış Router IS-IS Konfigürasyonu

```bash
# Satış Routera bağlan
system-view

# IS-IS process 1 başlat
isis 1

# Level-1-2 konfigürasyonu
is-level level-1-2

# Network entity ayarla
network-entity 49.0001.0001.0004.0001.00

# Interfaceleri IS-ISe ekle
interface GigabitEthernet0/0/0
description Link to Core Router
isis enable 1
isis circuit-type level-1-2
isis authentication-mode md5 ABC123#
quit

interface GigabitEthernet0/0/1
description Satış Network
isis enable 1
isis circuit-type level-1
quit

# Route redistribution
import-route static
import-route direct

quit
```

### 5. IS-IS Timers ve Optimization

```bash
# Core Routerda IS-IS timers
isis 1
timer lsp-generation 10 100 5000
timer spf 10 100 5000
timer lsp-refresh 900
quit

# IT Routerda IS-IS timers
isis 1
timer lsp-generation 10 100 5000
timer spf 10 100 5000
timer lsp-refresh 900
quit

# Muhasebe Routerda IS-IS timers
isis 1
timer lsp-generation 10 100 5000
timer spf 10 100 5000
timer lsp-refresh 900
quit

# Satış Routerda IS-IS timers
isis 1
timer lsp-generation 10 100 5000
timer spf 10 100 5000
timer lsp-refresh 900
quit
```

### 6. IS-IS Authentication

```bash
# Tüm routerlarda authentication ayarla
# Core Router
isis 1
area-authentication-mode md5 ABC123#
domain-authentication-mode md5 ABC123#
quit

# IT Router
isis 1
area-authentication-mode md5 ABC123#
domain-authentication-mode md5 ABC123#
quit

# Muhasebe Router
isis 1
area-authentication-mode md5 ABC123#
domain-authentication-mode md5 ABC123#
quit

# Satış Router
isis 1
area-authentication-mode md5 ABC123#
domain-authentication-mode md5 ABC123#
quit
```

### 7. Route Filtering ve Summarization

```bash
# Core Routerda route summarization
isis 1
summary 192.168.10.0 255.255.255.0
summary 192.168.20.0 255.255.255.0
summary 192.168.30.0 255.255.255.0
quit

# Route filtering
acl 2000
rule 10 permit source 192.168.10.0 0.0.0.255
rule 20 deny source any
quit

isis 1
filter-policy 2000 import
quit
```

### 8. Konfigürasyonu Kaydetme

```bash
# Tüm routerlarda konfigürasyonu kaydet
save
```

## ✅ Verification Komutları

### IS-IS Temel Kontrolleri

```bash
# IS-IS neighborsları göster
display isis peer

# IS-IS databaseini göster
display isis lsdb

# IS-IS interfacelerini göster
display isis interface

# IS-IS konfigürasyonunu göster
display isis
```

### IS-IS Route Kontrolleri

```bash
# IS-IS routelarını göster
display ip routing-table protocol isis

# IS-IS topologyyi göster
display isis topology

# IS-IS summaryyi göster
display isis summary
```

### IS-IS Authentication Kontrolleri

```bash
# Authentication durumunu kontrol et
display isis interface | include authentication

# Authentication modeları göster
display isis authentication
```

### IS-IS Level Kontrolleri

```bash
# Level bilgilerini göster
display isis level

# Level-1 areaları kontrol et
display isis lsdb level-1

# Level-2 areaları kontrol et
display isis lsdb level-2
```

## 🔍 Troubleshooting

### Yaygın IS-IS Sorunları

#### 1. IS-IS Neighbor Sorunu
```bash
# Neighbor durumunu kontrol et
display isis peer

# IS-IS debugını başlat
debugging isis events
debugging isis neighbor
# Sorun çözüldükten sonra durdur
undo debugging isis events
undo debugging isis neighbor
```

#### 2. IS-IS Authentication Sorunu
```bash
# Authentication durumunu kontrol et
display isis authentication

# Authenticationı geçici olarak kapat
system-view
isis 1
undo area-authentication-mode
undo domain-authentication-mode
quit
```

#### 3. IS-IS Route Sorunu
```bash
# Routeları kontrol et
display ip routing-table protocol isis

# IS-IS databaseini kontrol et
display isis lsdb

# Route redistributionı kontrol et
display ip routing-table protocol isis | include external
```

## 📊 Test Senaryoları

### Test 1: IS-IS Neighbor Connectivity
```bash
# Neighborları kontrol et
display isis peer

# Ping testi yap
ping 192.168.1.2
ping 192.168.2.2
ping 192.168.3.2
```

### Test 2: Inter-Level Routing
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
display ip routing-table protocol isis

# Redistributed routeları kontrol et
display ip routing-table protocol isis | include external
```

## 🔐 Güvenlik Kontrolleri

### IS-IS Authentication

```bash
# Authentication ayarlarını kontrol et
display isis authentication

# Authentication modeları kontrol et
display isis interface | include authentication
```

### Route Filtering

```bash
# Filter-policyleri kontrol et
display current-configuration | include filter-policy

# Access listleri kontrol et
display acl 2000
```

## ⚠️ Önemli Notlar

1. **Level Planning**: IS-IS levellarını planlayın
2. **Authentication**: Production ortamında authentication kullanın
3. **Route Summarization**: Networkü optimize etmek için summarization yapın
4. **Timers**: IS-IS timersı optimize edin
5. **Documentation**: IS-IS konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Network Entity**: Her router için unique network entity kullanın

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- IS-IS level yapısı ve konfigürasyonu
- Level-1 ve Level-2 areaları
- IS-IS authentication
- Route redistribution
- Route filtering ve summarization
- IS-IS timers optimization
- IS-IS troubleshooting teknikleri 