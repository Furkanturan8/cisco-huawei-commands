# Huawei Routing Protokolleri

## 🛣️ Routing Protokol Türleri

### Distance Vector Protocols
- **RIP (Routing Information Protocol)**
- **EIGRP (Enhanced Interior Gateway Routing Protocol)**

### Link State Protocols
- **OSPF (Open Shortest Path First)**
- **IS-IS (Intermediate System to Intermediate System)**

### Path Vector Protocols
- **BGP (Border Gateway Protocol)**

## 🌐 Static Routing

### Static Route Ekleme
```bash
# Static route ekle
system-view
ip route-static 192.168.2.0 255.255.255.0 192.168.1.2
quit
```

### Default Route
```bash
# Default route ekle
system-view
ip route-static 0.0.0.0 0.0.0.0 192.168.1.1
quit
```

### Static Route Silme
```bash
# Static route sil
system-view
undo ip route-static 192.168.2.0 255.255.255.0 192.168.1.2
quit
```

## 🔄 RIP (Routing Information Protocol)

### RIP Konfigürasyonu
```bash
# RIP'yi etkinleştir
system-view
rip 1
version 2
network 192.168.1.0
network 192.168.2.0
undo summary
quit
```

### RIP Verification
```bash
# RIP routing tablosunu göster
display rip 1 database
# veya
dis rip 1 database

# RIP neighbors'ları göster
display rip 1 neighbor
# veya
dis rip 1 neighbor

# RIP konfigürasyonunu göster
display rip 1
# veya
dis rip 1
```

### RIP Debug
```bash
# RIP debug'ını başlat
debugging rip 1
# veya
debugging rip 1 events
```

## ⚡ OSPF (Open Shortest Path First)

### OSPF Temel Konfigürasyonu
```bash
# OSPF process 1 ile başlat
system-view
ospf 1
area 0
network 192.168.1.0 0.0.0.255
network 192.168.2.0 0.0.0.255
quit
```

### OSPF Area Konfigürasyonu
```bash
# OSPF area 1 (stub area)
system-view
ospf 1
area 1
stub
network 192.168.3.0 0.0.0.255
quit
```

### OSPF Interface Konfigürasyonu
```bash
# Interface'de OSPF ayarları
system-view
interface GigabitEthernet0/0/1
ospf hello-interval 10
ospf dead-interval 40
ospf cost 10
quit
```

### OSPF Verification
```bash
# OSPF neighbors'ları göster
display ospf peer
# veya
dis ospf peer

# OSPF database'ini göster
display ospf lsdb
# veya
dis ospf lsdb

# OSPF interface'lerini göster
display ospf interface
# veya
dis ospf interface

# OSPF konfigürasyonunu göster
display ospf
# veya
dis ospf
```

### OSPF Debug
```bash
# OSPF debug'ını başlat
debugging ospf events
# veya
debugging ospf neighbor
```

## 🌟 IS-IS (Intermediate System to Intermediate System)

### IS-IS Temel Konfigürasyonu
```bash
# IS-IS process 1 ile başlat
system-view
isis 1
is-level level-2
network-entity 49.0001.0001.0001.0001.00
interface GigabitEthernet0/0/1
isis enable 1
quit
```

### IS-IS Level Konfigürasyonu
```bash
# IS-IS level ayarları
system-view
isis 1
is-level level-1-2
quit
```

### IS-IS Verification
```bash
# IS-IS neighbors'ları göster
display isis peer
# veya
dis isis peer

# IS-IS database'ini göster
display isis lsdb
# veya
dis isis lsdb

# IS-IS konfigürasyonunu göster
display isis
# veya
dis isis
```

### IS-IS Debug
```bash
# IS-IS debug'ını başlat
debugging isis events
# veya
debugging isis neighbor
```

## 🌐 BGP (Border Gateway Protocol)

### BGP Temel Konfigürasyonu
```bash
# BGP AS 65001 ile başlat
system-view
bgp 65001
peer 192.168.1.2 as-number 65002
network 192.168.10.0 255.255.255.0
quit
```

### BGP iBGP Konfigürasyonu
```bash
# iBGP neighbor konfigürasyonu
system-view
bgp 65001
peer 192.168.1.3 as-number 65001
peer 192.168.1.3 connect-interface LoopBack0
quit
```

### BGP Route Advertisement
```bash
# BGP route advertisement
system-view
bgp 65001
network 192.168.10.0 255.255.255.0
network 192.168.20.0 255.255.255.0
quit
```

### BGP Verification
```bash
# BGP neighbors'ları göster
display bgp peer
# veya
dis bgp peer

# BGP routing tablosunu göster
display bgp routing-table
# veya
dis bgp routing-table

# BGP summary'yi göster
display bgp summary
# veya
dis bgp summary
```

### BGP Debug
```bash
# BGP debug'ını başlat
debugging bgp updates
# veya
debugging bgp events
```

## 🔧 Route Redistribution

### OSPF'den BGP'ye Redistribution
```bash
# OSPF route'larını BGP'ye redistribute et
system-view
bgp 65001
import-route ospf 1
quit
```

### Static Route'ları OSPF'ye Redistribute Etme
```bash
# Static route'ları OSPF'ye redistribute et
system-view
ospf 1
import-route static
quit
```

## 📊 Route Filtering

### Access List ile Route Filtering
```bash
# Access list oluştur
system-view
acl 2000
rule 10 permit source 192.168.1.0 0.0.0.255
rule 20 deny source any
quit

# OSPF'de distribute-list uygula
ospf 1
filter-policy 2000 import GigabitEthernet0/0/1
quit
```

### Prefix List ile Route Filtering
```bash
# Prefix list oluştur
system-view
ip prefix-list PERMIT_192 index 10 permit 192.168.0.0 16 less-equal 24
quit

# BGP'de prefix-list uygula
bgp 65001
peer 192.168.1.2 ip-prefix PERMIT_192 import
quit
```

## 🔍 Route Troubleshooting

### Route Debug Komutları
```bash
# IP routing debug'ını başlat
debugging ip routing

# OSPF debug'ını başlat
debugging ospf events

# BGP debug'ını başlat
debugging bgp updates
```

### Route Verification Komutları
```bash
# Routing tablosunu göster
display ip routing-table
# veya
dis ip routing-table

# Routing protokollerini göster
display ip routing-table protocol
# veya
dis ip routing-table protocol

# Route summary'yi göster
display ip routing-table summary
# veya
dis ip routing-table summary
```

## 🌟 Huawei-Specific Features

### VRRP ile Route Advertisement
```bash
# VRRP ile route advertisement
system-view
interface GigabitEthernet0/0/1
vrrp vrid 1 virtual-ip 192.168.1.254
vrrp vrid 1 advertise enable
quit
```

### Traffic Policy ile Route Control
```bash
# Traffic policy ile route control
system-view
traffic classifier ROUTE_CLASS
if-match acl 3000
quit
traffic behavior ROUTE_BEHAVIOR
redirect ip-nexthop 192.168.1.2
quit
traffic policy ROUTE_POLICY
classifier ROUTE_CLASS behavior ROUTE_BEHAVIOR
quit
interface GigabitEthernet0/0/1
traffic-policy ROUTE_POLICY inbound
quit
```

## 📋 Routing Template'leri

### OSPF Template
```bash
# OSPF konfigürasyonu için template
system-view
ospf [PROCESS_ID]
area [AREA_ID]
network [NETWORK] [WILDCARD]
area [AREA_ID] stub
quit
```

### IS-IS Template
```bash
# IS-IS konfigürasyonu için template
system-view
isis [PROCESS_ID]
is-level [LEVEL]
network-entity [NETWORK_ENTITY]
interface [INTERFACE_NAME]
isis enable [PROCESS_ID]
quit
```

### BGP Template
```bash
# BGP konfigürasyonu için template
system-view
bgp [AS_NUMBER]
peer [NEIGHBOR_IP] as-number [REMOTE_AS]
network [NETWORK] mask [SUBNET_MASK]
quit
```

## ⚠️ Önemli Notlar

1. **AS Numbers**: BGP için AS numaralarını doğru belirleyin
2. **Area Numbers**: OSPF area numaralarını planlayın
3. **Network Statements**: Network komutlarında wildcard mask'ları doğru kullanın
4. **Redistribution**: Route redistribution'da metric değerlerini ayarlayın
5. **Debug Commands**: Production ortamında debug komutlarını dikkatli kullanın
6. **Undo Commands**: Huawei'de `undo` komutu ile geri alma yapılır 