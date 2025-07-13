# Cisco VLAN Konfigürasyonları

## 🏷️ VLAN (Virtual Local Area Network)

VLAN'lar, fiziksel network'ü mantıksal olarak bölen sanal network'lerdir.

## 📋 VLAN Türleri

### Data VLAN'ları
- Kullanıcı trafiği için ayrılmış VLAN'lar
- Genellikle VLAN 10, 20, 30 gibi numaralar

### Voice VLAN'ları
- VoIP telefonları için ayrılmış VLAN'lar
- QoS (Quality of Service) gerektirir

### Management VLAN'ları
- Network yönetimi için ayrılmış VLAN'lar
- Genellikle VLAN 99 kullanılır

## 🔧 VLAN Konfigürasyonu

### VLAN Oluşturma
```bash
# VLAN 10 oluştur
configure terminal
vlan 10
name DATA_VLAN
end
```

### VLAN Silme
```bash
# VLAN 10'u sil
configure terminal
no vlan 10
end
```

### VLAN Listesi
```bash
# Tüm VLAN'ları göster
show vlan brief
# veya
sh vlan brief

# Detaylı VLAN bilgilerini göster
show vlan
# veya
sh vlan
```

## 🔌 Interface VLAN Konfigürasyonu

### Access Port Konfigürasyonu
```bash
# Interface'i access port yap ve VLAN 10'a ata
configure terminal
interface GigabitEthernet0/0/1
switchport mode access
switchport access vlan 10
end
```

### Trunk Port Konfigürasyonu
```bash
# Interface'i trunk port yap
configure terminal
interface GigabitEthernet0/0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
end
```

### Trunk Native VLAN
```bash
# Trunk native VLAN'ını ayarla
configure terminal
interface GigabitEthernet0/0/1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,99
end
```

## 🌐 VLAN Interface (SVI) Konfigürasyonu

### VLAN Interface Oluşturma
```bash
# VLAN 10 için interface oluştur
configure terminal
interface Vlan10
ip address 192.168.10.1 255.255.255.0
no shutdown
end
```

### VLAN Interface Silme
```bash
# VLAN 10 interface'ini sil
configure terminal
no interface Vlan10
end
```

## 🔄 VLAN Database

### VLAN Database'den VLAN Silme
```bash
# VLAN database'den VLAN 10'u sil
configure terminal
vlan database
no vlan 10
exit
end
```

### VLAN Database'den VLAN Ekleme
```bash
# VLAN database'e VLAN 10 ekle
configure terminal
vlan database
vlan 10 name DATA_VLAN
exit
end
```

## 📊 VLAN Monitoring

### VLAN Interface Durumu
```bash
# VLAN interface durumunu göster
show interface Vlan10
# veya
sh int Vlan10

# Tüm VLAN interface'lerini göster
show ip interface brief | include Vlan
# veya
sh ip int brief | inc Vlan
```

### Trunk Interface Durumu
```bash
# Trunk interface durumunu göster
show interface trunk
# veya
sh int trunk

# Belirli trunk interface'ini göster
show interface GigabitEthernet0/0/1 trunk
# veya
sh int Gi0/0/1 trunk
```

## 🔐 VLAN Security

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

### VLAN Access Control
```bash
# VLAN'a erişimi kısıtla
configure terminal
interface GigabitEthernet0/0/1
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security mac-address sticky
end
```

## 🌟 Voice VLAN Konfigürasyonu

### Voice VLAN Ayarlama
```bash
# Voice VLAN 20'yi ayarla
configure terminal
interface GigabitEthernet0/0/1
switchport mode access
switchport access vlan 10
switchport voice vlan 20
end
```

### Voice VLAN QoS
```bash
# Voice VLAN için QoS ayarları
configure terminal
interface GigabitEthernet0/0/1
switchport voice vlan 20
mls qos trust cos
end
```

## 🔧 VLAN Trunking Protocol (VTP)

### VTP Server Konfigürasyonu
```bash
# VTP server ayarla
configure terminal
vtp mode server
vtp domain COMPANY
vtp password cisco123
end
```

### VTP Client Konfigürasyonu
```bash
# VTP client ayarla
configure terminal
vtp mode client
vtp domain COMPANY
vtp password cisco123
end
```

### VTP Verification
```bash
# VTP durumunu göster
show vtp status
# veya
sh vtp status

# VTP password'ü göster
show vtp password
# veya
sh vtp password
```

## 📋 VLAN Template'leri

### Standart VLAN Template
```bash
# Yeni VLAN oluşturma template'i
configure terminal
vlan [VLAN_ID]
name [VLAN_NAME]
exit
interface Vlan[VLAN_ID]
ip address [IP_ADDRESS] [SUBNET_MASK]
no shutdown
end
```

### Access Port Template
```bash
# Access port konfigürasyonu template'i
configure terminal
interface [INTERFACE_NAME]
switchport mode access
switchport access vlan [VLAN_ID]
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
end
```

### Trunk Port Template
```bash
# Trunk port konfigürasyonu template'i
configure terminal
interface [INTERFACE_NAME]
switchport mode trunk
switchport trunk allowed vlan [VLAN_LIST]
switchport trunk native vlan [NATIVE_VLAN]
end
```

## 🔍 VLAN Troubleshooting

### VLAN Debug Komutları
```bash
# VLAN debug'ını başlat
debug sw-vlan vtp events
# veya
debug sw-vlan vlan events
```

### VLAN Verification Komutları
```bash
# VLAN bilgilerini göster
show vlan brief
# veya
sh vlan brief

# VLAN interface'lerini göster
show ip interface brief | include Vlan
# veya
sh ip int brief | inc Vlan

# Trunk interface'lerini göster
show interface trunk
# veya
sh int trunk
```

## ⚠️ Önemli Notlar

1. **VLAN Numbers**: VLAN numaralarını planlayın (1-4094)
2. **Native VLAN**: Trunk port'larda native VLAN'ı ayarlayın
3. **VTP Domain**: VTP domain adını tüm switch'lerde aynı yapın
4. **Port Security**: Production ortamında port security kullanın
5. **Voice VLAN**: VoIP için ayrı VLAN kullanın 