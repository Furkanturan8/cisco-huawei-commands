# Cisco Routing Protokolleri

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
configure terminal
ip route 192.168.2.0 255.255.255.0 192.168.1.2
end
```

### Default Route
```bash
# Default route ekle
configure terminal
ip route 0.0.0.0 0.0.0.0 192.168.1.1
end
```

### Static Route Silme
```bash
# Static route sil
configure terminal
no ip route 192.168.2.0 255.255.255.0 192.168.1.2
end
```

## 🔄 RIP (Routing Information Protocol)

### RIP Konfigürasyonu
```bash
# RIP'yi etkinleştir
configure terminal
router rip
version 2
network 192.168.1.0
network 192.168.2.0
no auto-summary
end
```

### RIP Verification
```bash
# RIP routing tablosunu göster
show ip rip database
# veya
sh ip rip database

# RIP neighbors'ları göster
show ip rip neighbors
# veya
sh ip rip neighbors

# RIP konfigürasyonunu göster
show ip protocols
# veya
sh ip protocols
```

### RIP Debug
```bash
# RIP debug'ını başlat
debug ip rip
# veya
debug ip rip events
```

## ⚡ EIGRP (Enhanced Interior Gateway Routing Protocol)

### EIGRP Konfigürasyonu
```bash
# EIGRP AS 100 ile başlat
configure terminal
router eigrp 100
network 192.168.1.0 0.0.0.255
network 192.168.2.0 0.0.0.255
no auto-summary
end
```

### EIGRP Advanced Konfigürasyon
```bash
# EIGRP metric ayarları
configure terminal
router eigrp 100
metric weights 0 1 0 1 0 0
end
```

### EIGRP Verification
```bash
# EIGRP neighbors'ları göster
show ip eigrp neighbors
# veya
sh ip eigrp neighbors

# EIGRP topology tablosunu göster
show ip eigrp topology
# veya
sh ip eigrp topology

# EIGRP konfigürasyonunu göster
show ip protocols
# veya
sh ip protocols
```

### EIGRP Debug
```bash
# EIGRP debug'ını başlat
debug ip eigrp
# veya
debug ip eigrp neighbors
```

## 🌟 OSPF (Open Shortest Path First)

### OSPF Temel Konfigürasyonu
```bash
# OSPF process 1 ile başlat
configure terminal
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 192.168.2.0 0.0.0.255 area 0
end
```

### OSPF Area Konfigürasyonu
```bash
# OSPF area 1 (stub area)
configure terminal
router ospf 1
area 1 stub
network 192.168.3.0 0.0.0.255 area 1
end
```

### OSPF Interface Konfigürasyonu
```bash
# Interface'de OSPF ayarları
configure terminal
interface GigabitEthernet0/0/1
ip ospf hello-interval 10
ip ospf dead-interval 40
ip ospf cost 10
end
```

### OSPF Verification
```bash
# OSPF neighbors'ları göster
show ip ospf neighbor
# veya
sh ip ospf neighbor

# OSPF database'ini göster
show ip ospf database
# veya
sh ip ospf database

# OSPF interface'lerini göster
show ip ospf interface
# veya
sh ip ospf interface

# OSPF konfigürasyonunu göster
show ip protocols
# veya
sh ip protocols
```

### OSPF Debug
```bash
# OSPF debug'ını başlat
debug ip ospf events
# veya
debug ip ospf neighbor
```

## 🌐 BGP (Border Gateway Protocol)

### BGP Temel Konfigürasyonu
```bash
# BGP AS 65001 ile başlat
configure terminal
router bgp 65001
neighbor 192.168.1.2 remote-as 65002
network 192.168.10.0 mask 255.255.255.0
end
```

### BGP iBGP Konfigürasyonu
```bash
# iBGP neighbor konfigürasyonu
configure terminal
router bgp 65001
neighbor 192.168.1.3 remote-as 65001
neighbor 192.168.1.3 update-source Loopback0
end
```

### BGP Route Advertisement
```bash
# BGP route advertisement
configure terminal
router bgp 65001
network 192.168.10.0 mask 255.255.255.0
network 192.168.20.0 mask 255.255.255.0
end
```

### BGP Verification
```bash
# BGP neighbors'ları göster
show ip bgp neighbors
# veya
sh ip bgp neighbors

# BGP routing tablosunu göster
show ip bgp
# veya
sh ip bgp

# BGP summary'yi göster
show ip bgp summary
# veya
sh ip bgp summary
```

### BGP Debug
```bash
# BGP debug'ını başlat
debug ip bgp updates
# veya
debug ip bgp events
```

## 🔧 Route Redistribution

### OSPF'den EIGRP'ye Redistribution
```bash
# OSPF route'larını EIGRP'ye redistribute et
configure terminal
router eigrp 100
redistribute ospf 1 metric 10000 100 255 1 1500
end
```

### Static Route'ları OSPF'ye Redistribute Etme
```bash
# Static route'ları OSPF'ye redistribute et
configure terminal
router ospf 1
redistribute static subnets
end
```

## 📊 Route Filtering

### Access List ile Route Filtering
```bash
# Access list oluştur
configure terminal
access-list 10 permit 192.168.1.0 0.0.0.255
access-list 10 deny any

# OSPF'de distribute-list uygula
router ospf 1
distribute-list 10 in GigabitEthernet0/0/1
end
```

### Prefix List ile Route Filtering
```bash
# Prefix list oluştur
configure terminal
ip prefix-list PERMIT_192 permit 192.168.0.0/16 le 24

# BGP'de prefix-list uygula
router bgp 65001
neighbor 192.168.1.2 prefix-list PERMIT_192 in
end
```

## 🔍 Route Troubleshooting

### Route Debug Komutları
```bash
# IP routing debug'ını başlat
debug ip routing

# OSPF debug'ını başlat
debug ip ospf events

# BGP debug'ını başlat
debug ip bgp updates
```

### Route Verification Komutları
```bash
# Routing tablosunu göster
show ip route
# veya
sh ip route

# Routing protokollerini göster
show ip protocols
# veya
sh ip protocols

# Route summary'yi göster
show ip route summary
# veya
sh ip route summary
```

## 📋 Routing Template'leri

### OSPF Template
```bash
# OSPF konfigürasyonu için template
configure terminal
router ospf [PROCESS_ID]
network [NETWORK] [WILDCARD] area [AREA_ID]
area [AREA_ID] stub
end
```

### EIGRP Template
```bash
# EIGRP konfigürasyonu için template
configure terminal
router eigrp [AS_NUMBER]
network [NETWORK] [WILDCARD]
no auto-summary
end
```

### BGP Template
```bash
# BGP konfigürasyonu için template
configure terminal
router bgp [AS_NUMBER]
neighbor [NEIGHBOR_IP] remote-as [REMOTE_AS]
network [NETWORK] mask [SUBNET_MASK]
end
```

## ⚠️ Önemli Notlar

1. **AS Numbers**: EIGRP ve BGP için AS numaralarını doğru belirleyin
2. **Area Numbers**: OSPF area numaralarını planlayın
3. **Network Statements**: Network komutlarında wildcard mask'ları doğru kullanın
4. **Redistribution**: Route redistribution'da metric değerlerini ayarlayın
5. **Debug Commands**: Production ortamında debug komutlarını dikkatli kullanın 