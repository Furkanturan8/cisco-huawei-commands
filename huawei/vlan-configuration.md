# Huawei VLAN Konfigürasyonları

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
system-view
vlan 10
description DATA_VLAN
quit
```

### VLAN Silme
```bash
# VLAN 10'u sil
system-view
undo vlan 10
quit
```

### VLAN Listesi
```bash
# Tüm VLAN'ları göster
display vlan brief
# veya
dis vlan brief

# Detaylı VLAN bilgilerini göster
display vlan
# veya
dis vlan
```

## 🔌 Interface VLAN Konfigürasyonu

### Access Port Konfigürasyonu
```bash
# Interface'i access port yap ve VLAN 10'a ata
system-view
interface GigabitEthernet0/0/1
port link-type access
port default vlan 10
quit
```

### Trunk Port Konfigürasyonu
```bash
# Interface'i trunk port yap
system-view
interface GigabitEthernet0/0/1
port link-type trunk
port trunk allow-pass vlan 10 20 30
quit
```

### Trunk Native VLAN
```bash
# Trunk native VLAN'ını ayarla
system-view
interface GigabitEthernet0/0/1
port link-type trunk
port trunk pvid vlan 99
port trunk allow-pass vlan 10 20 30 99
quit
```

## 🌐 VLAN Interface (SVI) Konfigürasyonu

### VLAN Interface Oluşturma
```bash
# VLAN 10 için interface oluştur
system-view
interface Vlanif10
ip address 192.168.10.1 255.255.255.0
undo shutdown
quit
```

### VLAN Interface Silme
```bash
# VLAN 10 interface'ini sil
system-view
undo interface Vlanif10
quit
```

## 🔄 VLAN Batch Create

### Çoklu VLAN Oluşturma
```bash
# Birden fazla VLAN oluştur
system-view
vlan batch 10 20 30
quit
```

### VLAN Range Oluşturma
```bash
# VLAN aralığı oluştur
system-view
vlan batch 10 to 20
quit
```

## 📊 VLAN Monitoring

### VLAN Interface Durumu
```bash
# VLAN interface durumunu göster
display interface Vlanif10
# veya
dis int Vlanif10

# Tüm VLAN interface'lerini göster
display ip interface brief | include Vlanif
# veya
dis ip int brief | inc Vlanif
```

### Trunk Interface Durumu
```bash
# Trunk interface durumunu göster
display port vlan
# veya
dis port vlan

# Belirli trunk interface'ini göster
display port vlan GigabitEthernet0/0/1
# veya
dis port vlan GigabitEthernet0/0/1
```

## 🔐 VLAN Security

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

### VLAN Access Control
```bash
# VLAN'a erişimi kısıtla
system-view
interface GigabitEthernet0/0/1
port link-type access
port default vlan 10
port-security enable
port-security mac-address sticky
quit
```

## 🌟 Voice VLAN Konfigürasyonu

### Voice VLAN Ayarlama
```bash
# Voice VLAN 20'yi ayarla
system-view
interface GigabitEthernet0/0/1
port link-type access
port default vlan 10
voice-vlan 20 enable
quit
```

### Voice VLAN QoS
```bash
# Voice VLAN için QoS ayarları
system-view
interface GigabitEthernet0/0/1
voice-vlan 20 enable
voice-vlan qos trust cos
quit
```

## 🔧 VLAN Trunking Protocol (VTP)

### VTP Server Konfigürasyonu
```bash
# VTP server ayarla
system-view
vtp mode server
vtp domain COMPANY
vtp password cisco123
quit
```

### VTP Client Konfigürasyonu
```bash
# VTP client ayarla
system-view
vtp mode client
vtp domain COMPANY
vtp password cisco123
quit
```

### VTP Verification
```bash
# VTP durumunu göster
display vtp status
# veya
dis vtp status

# VTP password'ü göster
display vtp password
# veya
dis vtp password
```

## 🌟 Huawei-Specific Features

### iStack VLAN Konfigürasyonu
```bash
# iStack VLAN ayarları
system-view
stack-port GigabitEthernet0/0/1 enable
stack-port GigabitEthernet0/0/2 enable
vlan 10
stack-port GigabitEthernet0/0/1
stack-port GigabitEthernet0/0/2
quit
```

### CSS VLAN Konfigürasyonu
```bash
# CSS VLAN ayarları
system-view
css-port GigabitEthernet0/0/1 enable
css-port GigabitEthernet0/0/2 enable
vlan 10
css-port GigabitEthernet0/0/1
css-port GigabitEthernet0/0/2
quit
```

## 📋 VLAN Template'leri

### Standart VLAN Template
```bash
# Yeni VLAN oluşturma template'i
system-view
vlan [VLAN_ID]
description [VLAN_NAME]
quit
interface Vlanif[VLAN_ID]
ip address [IP_ADDRESS] [SUBNET_MASK]
undo shutdown
quit
```

### Access Port Template
```bash
# Access port konfigürasyonu template'i
system-view
interface [INTERFACE_NAME]
port link-type access
port default vlan [VLAN_ID]
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
quit
```

### Trunk Port Template
```bash
# Trunk port konfigürasyonu template'i
system-view
interface [INTERFACE_NAME]
port link-type trunk
port trunk allow-pass vlan [VLAN_LIST]
port trunk pvid vlan [NATIVE_VLAN]
quit
```

## 🔍 VLAN Troubleshooting

### VLAN Debug Komutları
```bash
# VLAN debug'ını başlat
debugging vlan events
# veya
debugging vtp events
```

### VLAN Verification Komutları
```bash
# VLAN bilgilerini göster
display vlan brief
# veya
dis vlan brief

# VLAN interface'lerini göster
display ip interface brief | include Vlanif
# veya
dis ip int brief | inc Vlanif

# Trunk interface'lerini göster
display port vlan
# veya
dis port vlan
```

## 🔧 Advanced VLAN Features

### VLAN Mapping
```bash
# VLAN mapping konfigürasyonu
system-view
interface GigabitEthernet0/0/1
port link-type trunk
port trunk allow-pass vlan 10 20
port vlan mapping 10 20
quit
```

### VLAN Aggregation
```bash
# VLAN aggregation konfigürasyonu
system-view
vlan 10
aggregate-vlan
access-vlan 11 to 15
quit
```

## ⚠️ Önemli Notlar

1. **VLAN Numbers**: VLAN numaralarını planlayın (1-4094)
2. **Native VLAN**: Trunk port'larda native VLAN'ı ayarlayın
3. **VTP Domain**: VTP domain adını tüm switch'lerde aynı yapın
4. **Port Security**: Production ortamında port security kullanın
5. **Voice VLAN**: VoIP için ayrı VLAN kullanın
6. **Undo Commands**: Huawei'de `undo` komutu ile geri alma yapılır 