# Huawei Interface Konfigürasyonları

## 🔌 Interface Türleri

### Ethernet Interface'leri
```bash
# GigabitEthernet interface'i
interface GigabitEthernet0/0/1
# veya
int GigabitEthernet0/0/1

# FastEthernet interface'i
interface FastEthernet0/1
# veya
int FastEthernet0/1

# Ethernet interface'i
interface Ethernet0/0
# veya
int Ethernet0/0
```

### Serial Interface'leri
```bash
# Serial interface'i
interface Serial0/0/0
# veya
int Serial0/0/0
```

### Loopback Interface'leri
```bash
# Loopback interface'i
interface Loopback0
# veya
int Loopback0
```

## 🌐 IP Konfigürasyonu

### IP Address Atama
```bash
# Interface'e IP address ata
system-view
interface GigabitEthernet0/0/1
ip address 192.168.1.1 255.255.255.0
undo shutdown
quit
```

### Secondary IP Address
```bash
# Secondary IP address ekle
system-view
interface GigabitEthernet0/0/1
ip address 192.168.1.1 255.255.255.0
ip address 192.168.2.1 255.255.255.0 sub
undo shutdown
quit
```

### IP Address Kaldırma
```bash
# IP address'i kaldır
system-view
interface GigabitEthernet0/0/1
undo ip address
quit
```

## ⚡ Interface Durumu

### Interface Açma/Kapama
```bash
# Interface'i aç
system-view
interface GigabitEthernet0/0/1
undo shutdown
quit

# Interface'i kapat
system-view
interface GigabitEthernet0/0/1
shutdown
quit
```

### Interface Durumu Kontrolü
```bash
# Interface durumunu göster
display interface GigabitEthernet0/0/1
# veya
dis int GigabitEthernet0/0/1

# Tüm interface'leri özet olarak göster
display ip interface brief
# veya
dis ip int brief

# Interface istatistiklerini göster
display interface GigabitEthernet0/0/1 statistics
# veya
dis int GigabitEthernet0/0/1 statistics
```

## 🔧 Interface Özellikleri

### Bandwidth Ayarlama
```bash
# Interface bandwidth'ini ayarla (Mbps)
system-view
interface GigabitEthernet0/0/1
bandwidth 1000
quit
```

### Description Ekleme
```bash
# Interface'e açıklama ekle
system-view
interface GigabitEthernet0/0/1
description LAN Interface - Office Network
quit
```

### MTU Ayarlama
```bash
# MTU değerini ayarla
system-view
interface GigabitEthernet0/0/1
mtu 1500
quit
```

## 🔄 Interface Reset

### Interface Reset
```bash
# Interface'i reset et
system-view
interface GigabitEthernet0/0/1
shutdown
undo shutdown
quit
```

### Interface Clear
```bash
# Interface istatistiklerini temizle
reset counters interface GigabitEthernet0/0/1
# veya
reset counters int GigabitEthernet0/0/1

# Tüm interface istatistiklerini temizle
reset counters interface
```

## 🌐 DHCP Konfigürasyonu

### DHCP Server
```bash
# DHCP server'ı etkinleştir
system-view
dhcp enable
ip pool LAN_POOL
gateway-list 192.168.1.1
network 192.168.1.0 mask 255.255.255.0
dns-list 8.8.8.8
lease day 7
quit
```

### DHCP Client
```bash
# Interface'i DHCP client yap
system-view
interface GigabitEthernet0/0/1
ip address dhcp-alloc
undo shutdown
quit
```

## 🔐 Security Features

### Port Security
```bash
# Port security'yi etkinleştir
system-view
interface GigabitEthernet0/0/1
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
quit
```

### Access Control
```bash
# Interface'e ACL uygula
system-view
interface GigabitEthernet0/0/1
traffic-filter inbound acl 3000
quit
```

## 📊 Monitoring ve Troubleshooting

### Interface Monitoring
```bash
# Interface durumunu sürekli izle
display interface GigabitEthernet0/0/1 | include line protocol
# veya
dis int GigabitEthernet0/0/1 | inc line protocol

# Interface error'larını göster
display interface GigabitEthernet0/0/1 | include error
# veya
dis int GigabitEthernet0/0/1 | inc error
```

### Interface Debug
```bash
# Interface debug'ını başlat
debugging interface GigabitEthernet0/0/1
# veya
debug int GigabitEthernet0/0/1

# IP interface debug'ını başlat
debugging ip interface GigabitEthernet0/0/1
# veya
debug ip int GigabitEthernet0/0/1
```

## 🔧 Advanced Interface Features

### Interface Backup
```bash
# Interface backup ayarla
system-view
interface GigabitEthernet0/0/1
standby interface GigabitEthernet0/0/2
standby delay 10 30
quit
```

### Interface Load Balancing
```bash
# Load balancing ayarla
system-view
interface GigabitEthernet0/0/1
ip load-balancing per-packet
quit
```

## 🌟 Huawei-Specific Features

### VRRP Konfigürasyonu
```bash
# VRRP konfigürasyonu
system-view
interface GigabitEthernet0/0/1
vrrp vrid 1 virtual-ip 192.168.1.254
vrrp vrid 1 priority 120
vrrp vrid 1 preempt-mode timer delay 20
quit
```

### Traffic Policy
```bash
# Traffic policy oluştur
system-view
traffic classifier CLASS1
if-match acl 3000
quit
traffic behavior BEHAVIOR1
remark dscp af11
quit
traffic policy POLICY1
classifier CLASS1 behavior BEHAVIOR1
quit
interface GigabitEthernet0/0/1
traffic-policy POLICY1 inbound
quit
```

## 📋 Interface Template

### Standart Interface Template
```bash
# Yeni interface konfigürasyonu için template
system-view
interface GigabitEthernet0/0/1
description [Interface Açıklaması]
ip address [IP_ADDRESS] [SUBNET_MASK]
bandwidth [BANDWIDTH_MBPS]
mtu [MTU_VALUE]
undo shutdown
quit
```

### VLAN Interface Template
```bash
# VLAN interface konfigürasyonu
system-view
interface Vlanif10
description VLAN 10 - Data Network
ip address 192.168.10.1 255.255.255.0
undo shutdown
quit
```

## 🔍 Interface Troubleshooting

### Interface Debug Komutları
```bash
# Interface debug'ını başlat
debugging interface GigabitEthernet0/0/1
# veya
debug int GigabitEthernet0/0/1

# IP interface debug'ını başlat
debugging ip interface GigabitEthernet0/0/1
# veya
debug ip int GigabitEthernet0/0/1
```

### Interface Verification Komutları
```bash
# Interface durumunu göster
display interface GigabitEthernet0/0/1
# veya
dis int GigabitEthernet0/0/1

# Interface istatistiklerini göster
display interface GigabitEthernet0/0/1 statistics
# veya
dis int GigabitEthernet0/0/1 statistics

# Interface error'larını göster
display interface GigabitEthernet0/0/1 | include error
# veya
dis int GigabitEthernet0/0/1 | inc error
```

## ⚠️ Önemli Notlar

1. **Interface Naming**: Her zaman tam interface adını kullanın
2. **Undo Shutdown**: Interface'i açmak için `undo shutdown` komutunu unutmayın
3. **IP Address**: Interface'e IP atamadan önce subnet mask'ı doğru belirleyin
4. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
5. **Testing**: Konfigürasyon sonrası connectivity testi yapın
6. **Undo Commands**: Huawei'de `undo` komutu ile geri alma yapılır 