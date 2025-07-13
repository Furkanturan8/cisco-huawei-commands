# Uygulama 6: BGP Routing Konfigürasyonu

## 🎯 Senaryo

ABC Şirketinin Internet bağlantısı için BGP routing protokolü kullanılacak. eBGP ve iBGP konfigürasyonu yapılacak, route filtering uygulanacak ve network optimizasyonu sağlanacak.

## 📋 Gereksinimler

1. **AS 65001**: ABC Şirketi (Internal AS)
2. **AS 65002**: ISP Provider (External AS)
3. **AS 65003**: Backup ISP (External AS)
4. eBGP ve iBGP konfigürasyonu
5. Route filtering ve prefix-list
6. BGP authentication uygulanacak
7. BGP timers optimize edilecek

## 🌐 Topoloji

```
[ISP-1 AS65002] --- [ABC-Border-1 AS65001] --- [ABC-Core AS65001]
                           |                              |
                           |                          [ABC-Border-2 AS65001]
                           |                              |
[ISP-2 AS65003] --- [ABC-Border-2 AS65001] --- [ABC-Core AS65001]
```

## 🔧 Adım Adım Çözüm

### 1. ABC Border Router 1 BGP Konfigürasyonu

```bash
# ABC Border Router 1e bağlan
enable
configure terminal

# BGP AS 65001 başlat
router bgp 65001

# eBGP neighbor ISP-1
neighbor 203.0.113.1 remote-as 65002
neighbor 203.0.113.1 description ISP-1 eBGP
neighbor 203.0.113.1 password ABC123#
neighbor 203.0.113.1 update-source GigabitEthernet0/0/0

# iBGP neighbor ABC Core
neighbor 192.168.1.1 remote-as 65001
neighbor 192.168.1.1 description ABC Core iBGP
neighbor 192.168.1.1 update-source Loopback0
neighbor 192.168.1.1 next-hop-self

# iBGP neighbor ABC Border 2
neighbor 192.168.1.2 remote-as 65001
neighbor 192.168.1.2 description ABC Border 2 iBGP
neighbor 192.168.1.2 update-source Loopback0
neighbor 192.168.1.2 next-hop-self

# Route advertisement
network 192.168.0.0 mask 255.255.0.0
network 10.0.0.0 mask 255.0.0.0

# Route redistribution
redistribute connected
redistribute static

exit
```

### 2. ABC Border Router 2 BGP Konfigürasyonu

```bash
# ABC Border Router 2ye bağlan
enable
configure terminal

# BGP AS 65001 başlat
router bgp 65001

# eBGP neighbor ISP-2
neighbor 203.0.113.5 remote-as 65003
neighbor 203.0.113.5 description ISP-2 eBGP
neighbor 203.0.113.5 password ABC123#
neighbor 203.0.113.5 update-source GigabitEthernet0/0/0

# iBGP neighbor ABC Core
neighbor 192.168.1.1 remote-as 65001
neighbor 192.168.1.1 description ABC Core iBGP
neighbor 192.168.1.1 update-source Loopback0
neighbor 192.168.1.1 next-hop-self

# iBGP neighbor ABC Border 1
neighbor 192.168.1.3 remote-as 65001
neighbor 192.168.1.3 description ABC Border 1 iBGP
neighbor 192.168.1.3 update-source Loopback0
neighbor 192.168.1.3 next-hop-self

# Route advertisement
network 192.168.0.0 mask 255.255.0.0
network 10.0.0.0 mask 255.0.0.0

# Route redistribution
redistribute connected
redistribute static

exit
```

### 3. ABC Core Router BGP Konfigürasyonu

```bash
# ABC Core Routera bağlan
enable
configure terminal

# BGP AS 65001 başlat
router bgp 65001

# iBGP neighbor ABC Border 1
neighbor 192.168.1.3 remote-as 65001
neighbor 192.168.1.3 description ABC Border 1 iBGP
neighbor 192.168.1.3 update-source Loopback0
neighbor 192.168.1.3 next-hop-self

# iBGP neighbor ABC Border 2
neighbor 192.168.1.2 remote-as 65001
neighbor 192.168.1.2 description ABC Border 2 iBGP
neighbor 192.168.1.2 update-source Loopback0
neighbor 192.168.1.2 next-hop-self

# Route advertisement
network 192.168.0.0 mask 255.255.0.0
network 10.0.0.0 mask 255.0.0.0

# Route redistribution
redistribute connected
redistribute static

exit
```

### 4. Route Filtering ve Prefix Lists

```bash
# Prefix list oluştur
ip prefix-list PERMIT_LOCAL permit 192.168.0.0/16 le 24
ip prefix-list PERMIT_LOCAL permit 10.0.0.0/8 le 24
ip prefix-list DENY_ALL deny 0.0.0.0/0 le 32

# ABC Border Router 1de prefix list uygula
router bgp 65001
neighbor 203.0.113.1 prefix-list PERMIT_LOCAL out
neighbor 203.0.113.1 prefix-list DENY_ALL in
exit

# ABC Border Router 2de prefix list uygula
router bgp 65001
neighbor 203.0.113.5 prefix-list PERMIT_LOCAL out
neighbor 203.0.113.5 prefix-list DENY_ALL in
exit
```

### 5. BGP Authentication

```bash
# Tüm eBGP neighborlarda authentication
# ABC Border Router 1
router bgp 65001
neighbor 203.0.113.1 password ABC123#
exit

# ABC Border Router 2
router bgp 65001
neighbor 203.0.113.5 password ABC123#
exit
```

### 6. BGP Timers ve Optimization

```bash
# ABC Border Router 1de BGP timers
router bgp 65001
neighbor 203.0.113.1 timers 30 90 0
neighbor 192.168.1.1 timers 30 90 0
neighbor 192.168.1.2 timers 30 90 0
exit

# ABC Border Router 2de BGP timers
router bgp 65001
neighbor 203.0.113.5 timers 30 90 0
neighbor 192.168.1.1 timers 30 90 0
neighbor 192.168.1.3 timers 30 90 0
exit

# ABC Core Routerda BGP timers
router bgp 65001
neighbor 192.168.1.3 timers 30 90 0
neighbor 192.168.1.2 timers 30 90 0
exit
```

### 7. BGP Route Maps

```bash
# Route map oluştur
route-map LOCAL_NETWORKS permit 10
match ip address prefix-list PERMIT_LOCAL
set local-preference 100
exit

route-map ISP_PREFERENCE permit 10
match as-path 1
set local-preference 200
exit

# AS path access list
ip as-path access-list 1 permit ^65002$

# Route mapleri uygula
router bgp 65001
neighbor 203.0.113.1 route-map LOCAL_NETWORKS out
neighbor 203.0.113.5 route-map LOCAL_NETWORKS out
exit
```

### 8. BGP Communities

```bash
# Community ayarları
router bgp 65001
neighbor 203.0.113.1 send-community
neighbor 203.0.113.5 send-community
exit

# Community route map
route-map SET_COMMUNITY permit 10
set community 65001:100
exit
```

### 9. Konfigürasyonu Kaydetme

```bash
# Tüm routerlarda konfigürasyonu kaydet
write memory
```

## ✅ Verification Komutları

### BGP Temel Kontrolleri

```bash
# BGP neighborsları göster
show ip bgp neighbors

# BGP summaryyi göster
show ip bgp summary

# BGP routing tableını göster
show ip bgp

# BGP konfigürasyonunu göster
show ip protocols
```

### BGP Route Kontrolleri

```bash
# BGP routelarını göster
show ip route bgp

# BGP pathleri göster
show ip bgp paths

# BGP communitiesleri göster
show ip bgp community
```

### BGP Authentication Kontrolleri

```bash
# Authentication durumunu kontrol et
show ip bgp neighbors | include password

# BGP passwordleri göster
show running-config | include neighbor.*password
```

### BGP Filtering Kontrolleri

```bash
# Prefix listleri kontrol et
show ip prefix-list

# Route mapleri kontrol et
show route-map

# AS path access listleri kontrol et
show ip as-path access-list
```

## 🔍 Troubleshooting

### Yaygın BGP Sorunları

#### 1. BGP Neighbor Sorunu
```bash
# Neighbor durumunu kontrol et
show ip bgp neighbors

# BGP debugını başlat
debug ip bgp events
debug ip bgp updates
# Sorun çözüldükten sonra durdur
no debug ip bgp events
no debug ip bgp updates
```

#### 2. BGP Authentication Sorunu
```bash
# Authentication durumunu kontrol et
show ip bgp neighbors | include password

# Authenticationı geçici olarak kapat
configure terminal
router bgp 65001
no neighbor 203.0.113.1 password
exit
```

#### 3. BGP Route Sorunu
```bash
# Routeları kontrol et
show ip route bgp

# BGP tableını kontrol et
show ip bgp

# Route advertisementları kontrol et
show ip bgp advertised-routes
```

## 📊 Test Senaryoları

### Test 1: BGP Neighbor Connectivity
```bash
# Neighborları kontrol et
show ip bgp neighbors

# Ping testi yap
ping 203.0.113.1
ping 203.0.113.5
ping 192.168.1.1
```

### Test 2: BGP Route Advertisement
```bash
# Route advertisementları kontrol et
show ip bgp advertised-routes

# Received routeları kontrol et
show ip bgp received-routes
```

### Test 3: BGP Path Selection
```bash
# BGP pathleri kontrol et
show ip bgp paths

# Best pathleri kontrol et
show ip bgp | include \*
```

## 🔐 Güvenlik Kontrolleri

### BGP Authentication

```bash
# Authentication ayarlarını kontrol et
show ip bgp neighbors | include password

# BGP passwordleri kontrol et
show running-config | include neighbor.*password
```

### Route Filtering

```bash
# Prefix listleri kontrol et
show ip prefix-list

# Route mapleri kontrol et
show route-map

# AS path access listleri kontrol et
show ip as-path access-list
```

## ⚠️ Önemli Notlar

1. **AS Planning**: AS numaralarını planlayın
2. **Authentication**: Production ortamında authentication kullanın
3. **Route Filtering**: Route filtering uygulayın
4. **Timers**: BGP timersı optimize edin
5. **Documentation**: BGP konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Route Maps**: Complex routing policies için route map kullanın

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- eBGP ve iBGP konfigürasyonu
- BGP authentication
- Route filtering ve prefix lists
- BGP communities
- Route maps ve AS path filtering
- BGP timers optimization
- BGP troubleshooting teknikleri 