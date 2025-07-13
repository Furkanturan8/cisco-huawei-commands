# Cisco Interface Konfigürasyonları

## 🔌 Interface Türleri

### Ethernet Interface'leri
```bash
# GigabitEthernet interface'i
interface GigabitEthernet0/0/1
# veya
int Gi0/0/1

# FastEthernet interface'i
interface FastEthernet0/1
# veya
int Fa0/1

# Ethernet interface'i
interface Ethernet0/0
# veya
int Eth0/0
```

### Serial Interface'leri
```bash
# Serial interface'i
interface Serial0/0/0
# veya
int Se0/0/0
```

### Loopback Interface'leri
```bash
# Loopback interface'i
interface Loopback0
# veya
int Lo0
```

## 🌐 IP Konfigürasyonu

### IP Address Atama
```bash
# Interface'e IP address ata
configure terminal
interface GigabitEthernet0/0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
end
```

### Secondary IP Address
```bash
# Secondary IP address ekle
configure terminal
interface GigabitEthernet0/0/1
ip address 192.168.1.1 255.255.255.0
ip address 192.168.2.1 255.255.255.0 secondary
no shutdown
end
```

### IP Address Kaldırma
```bash
# IP address'i kaldır
configure terminal
interface GigabitEthernet0/0/1
no ip address
end
```

## ⚡ Interface Durumu

### Interface Açma/Kapama
```bash
# Interface'i aç
configure terminal
interface GigabitEthernet0/0/1
no shutdown
# veya
no shut
end

# Interface'i kapat
configure terminal
interface GigabitEthernet0/0/1
shutdown
# veya
shut
end
```

### Interface Durumu Kontrolü
```bash
# Interface durumunu göster
show interface GigabitEthernet0/0/1
# veya
sh int Gi0/0/1

# Tüm interface'leri özet olarak göster
show ip interface brief
# veya
sh ip int brief

# Interface istatistiklerini göster
show interface GigabitEthernet0/0/1 counters
# veya
sh int Gi0/0/1 counters
```

## 🔧 Interface Özellikleri

### Bandwidth Ayarlama
```bash
# Interface bandwidth'ini ayarla (Mbps)
configure terminal
interface GigabitEthernet0/0/1
bandwidth 1000
end
```

### Description Ekleme
```bash
# Interface'e açıklama ekle
configure terminal
interface GigabitEthernet0/0/1
description LAN Interface - Office Network
end
```

### MTU Ayarlama
```bash
# MTU değerini ayarla
configure terminal
interface GigabitEthernet0/0/1
mtu 1500
end
```

## 🔄 Interface Reset

### Interface Reset
```bash
# Interface'i reset et
configure terminal
interface GigabitEthernet0/0/1
shutdown
no shutdown
end
```

### Interface Clear
```bash
# Interface istatistiklerini temizle
clear counters GigabitEthernet0/0/1
# veya
clear counters Gi0/0/1

# Tüm interface istatistiklerini temizle
clear counters
```

## 🌐 DHCP Konfigürasyonu

### DHCP Server
```bash
# DHCP server'ı etkinleştir
configure terminal
ip dhcp pool LAN_POOL
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
lease 7
end
```

### DHCP Client
```bash
# Interface'i DHCP client yap
configure terminal
interface GigabitEthernet0/0/1
ip address dhcp
no shutdown
end
```

## 🔐 Security Features

### Port Security
```bash
# Port security'yi etkinleştir
configure terminal
interface GigabitEthernet0/0/1
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
end
```

### Access Control
```bash
# Interface'e ACL uygula
configure terminal
interface GigabitEthernet0/0/1
ip access-group 100 in
end
```

## 📊 Monitoring ve Troubleshooting

### Interface Monitoring
```bash
# Interface durumunu sürekli izle
show interface GigabitEthernet0/0/1 | include line protocol
# veya
sh int Gi0/0/1 | inc line protocol

# Interface error'larını göster
show interface GigabitEthernet0/0/1 | include error
# veya
sh int Gi0/0/1 | inc error
```

### Interface Debug
```bash
# Interface debug'ını başlat
debug interface GigabitEthernet0/0/1
# veya
debug int Gi0/0/1

# IP interface debug'ını başlat
debug ip interface GigabitEthernet0/0/1
# veya
debug ip int Gi0/0/1
```

## 🔧 Advanced Interface Features

### Interface Redundancy
```bash
# Interface backup ayarla
configure terminal
interface GigabitEthernet0/0/1
backup interface GigabitEthernet0/0/2
backup delay 10 30
end
```

### Interface Load Balancing
```bash
# Load balancing ayarla
configure terminal
interface GigabitEthernet0/0/1
ip load-sharing per-packet
end
```

## 📋 Interface Template

### Standart Interface Template
```bash
# Yeni interface konfigürasyonu için template
configure terminal
interface GigabitEthernet0/0/1
description [Interface Açıklaması]
ip address [IP_ADDRESS] [SUBNET_MASK]
bandwidth [BANDWIDTH_MBPS]
mtu [MTU_VALUE]
no shutdown
end
```

### VLAN Interface Template
```bash
# VLAN interface konfigürasyonu
configure terminal
interface Vlan10
description VLAN 10 - Data Network
ip address 192.168.10.1 255.255.255.0
no shutdown
end
```

## ⚠️ Önemli Notlar

1. **Interface Naming**: Her zaman tam interface adını kullanın
2. **No Shutdown**: Interface'i açmak için `no shutdown` komutunu unutmayın
3. **IP Address**: Interface'e IP atamadan önce subnet mask'ı doğru belirleyin
4. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
5. **Testing**: Konfigürasyon sonrası connectivity testi yapın 