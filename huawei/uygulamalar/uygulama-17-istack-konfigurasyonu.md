# Uygulama 17: iStack Konfigürasyonu

## 🎯 Senaryo

DEF Şirketinin data centerında switch stacking yapılması gerekiyor. iStack teknolojisi kullanılarak iki switch birleştirilecek, yük dengeleme yapılacak ve yüksek erişilebilirlik sağlanacak.

## 📋 Gereksinimler

1. **Switch 1**: Master switch (Slot 1)
2. **Switch 2**: Slave switch (Slot 2)
3. iStack portları konfigüre edilecek
4. Stack priority ayarlanacak
5. Stack domain ID ayarlanacak
6. VLANlar stack üzerinde çalışacak
7. Link aggregation konfigüre edilecek

## 🌐 Topoloji

```
[Server 1] --- [iStack Switch 1+2] --- [Server 2]
      |                    |                |
      |              [Server 3]        [Server 4]
      |
[Server 5]
```

## 🔧 Adım Adım Çözüm

### 1. Switch 1 (Master) Temel Konfigürasyonu

```bash
# Switch 1e bağlan
system-view

# Hostname ayarla
sysname DEF-SW-Stack-1

# Stack priority ayarla (Master için yüksek priority)
stack slot 1 priority 32

# Stack domain ID ayarla
stack domain 1

# iStack portlarını etkinleştir
interface GigabitEthernet0/0/1
description iStack Port 1
stack-port enable
quit

interface GigabitEthernet0/0/2
description iStack Port 2
stack-port enable
quit
```

### 2. Switch 2 (Slave) Temel Konfigürasyonu

```bash
# Switch 2ye bağlan
system-view

# Hostname ayarla
sysname DEF-SW-Stack-2

# Stack priority ayarla (Slave için düşük priority)
stack slot 2 priority 16

# Stack domain ID ayarla (aynı domain)
stack domain 1

# iStack portlarını etkinleştir
interface GigabitEthernet0/0/1
description iStack Port 1
stack-port enable
quit

interface GigabitEthernet0/0/2
description iStack Port 2
stack-port enable
quit
```

### 3. iStack Port Konfigürasyonu

```bash
# Switch 1de iStack port ayarları
interface GigabitEthernet0/0/1
stack-port enable
stack-port mode stack
quit

interface GigabitEthernet0/0/2
stack-port enable
stack-port mode stack
quit

# Switch 2de iStack port ayarları
interface GigabitEthernet0/0/1
stack-port enable
stack-port mode stack
quit

interface GigabitEthernet0/0/2
stack-port enable
stack-port mode stack
quit
```

### 4. VLAN Konfigürasyonu (Stack Üzerinde)

```bash
# Stack üzerinde VLANları oluştur
vlan batch 10 20 30 99

# VLAN açıklamalarını ekle
vlan 10
description DATA_VLAN
quit

vlan 20
description VOICE_VLAN
quit

vlan 30
description MANAGEMENT_VLAN
quit

vlan 99
description NATIVE_VLAN
quit
```

### 5. Stack Interface Konfigürasyonu

```bash
# Stack interfacelerini konfigüre et
interface Vlanif10
description Data VLAN Interface
ip address 192.168.10.1 255.255.255.0
undo shutdown
quit

interface Vlanif20
description Voice VLAN Interface
ip address 192.168.20.1 255.255.255.0
undo shutdown
quit

interface Vlanif30
description Management VLAN Interface
ip address 192.168.30.1 255.255.255.0
undo shutdown
quit
```

### 6. Access Port Konfigürasyonu

```bash
# Server portlarını konfigüre et
interface GigabitEthernet0/0/3
description Server 1 Port
port link-type access
port default vlan 10
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
undo shutdown
quit

interface GigabitEthernet0/0/4
description Server 2 Port
port link-type access
port default vlan 10
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
undo shutdown
quit

interface GigabitEthernet0/0/5
description Server 3 Port
port link-type access
port default vlan 20
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
undo shutdown
quit

interface GigabitEthernet0/0/6
description Server 4 Port
port link-type access
port default vlan 20
port-security enable
port-security max-mac-num 1
port-security protect-action shutdown
undo shutdown
quit
```

### 7. Link Aggregation (LACP) Konfigürasyonu

```bash
# LACP trunk oluştur
interface Eth-Trunk 1
description LACP Trunk to Core Switch
port link-type trunk
port trunk allow-pass vlan 10 20 30 99
port trunk pvid vlan 99
undo shutdown
quit

# Member portları ekle
interface GigabitEthernet0/0/7
description LACP Member Port 1
eth-trunk 1
undo shutdown
quit

interface GigabitEthernet0/0/8
description LACP Member Port 2
eth-trunk 1
undo shutdown
quit
```

### 8. Stack Monitoring ve Alerts

```bash
# Stack monitoring ayarları
stack slot 1 priority 32
stack slot 2 priority 16

# Stack alarm ayarları
stack alarm enable

# Stack log ayarları
info-center enable
info-center console channel
info-center console level informational
```

### 9. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
save
```

## ✅ Verification Komutları

### iStack Temel Kontrolleri

```bash
# Stack durumunu göster
display stack

# Stack slot bilgilerini göster
display stack slot

# Stack port durumunu göster
display stack-port

# Stack topologyyi göster
display stack topology
```

### Stack Interface Kontrolleri

```bash
# Stack interfacelerini göster
display interface brief

# Stack VLANlarını göster
display vlan brief

# Stack port security durumunu göster
display port-security
```

### Link Aggregation Kontrolleri

```bash
# Eth-Trunk durumunu göster
display eth-trunk

# LACP durumunu göster
display lacp statistics

# Member portları göster
display eth-trunk 1
```

### Stack Performance Kontrolleri

```bash
# Stack CPU kullanımını göster
display cpu-usage

# Stack memory kullanımını göster
display memory-usage

# Stack interface istatistiklerini göster
display interface statistics
```

## 🔍 Troubleshooting

### Yaygın iStack Sorunları

#### 1. Stack Bağlantı Sorunu
```bash
# Stack durumunu kontrol et
display stack

# Stack portlarını kontrol et
display stack-port

# Stack portlarını reset et
system-view
interface GigabitEthernet0/0/1
shutdown
undo shutdown
quit
```

#### 2. Stack Master/Slave Sorunu
```bash
# Stack priorityleri kontrol et
display stack slot

# Stack priorityyi değiştir
system-view
stack slot 1 priority 64
stack slot 2 priority 32
quit
```

#### 3. Stack VLAN Sorunu
```bash
# VLAN durumunu kontrol et
display vlan brief

# Stack VLANlarını kontrol et
display vlan 10
display vlan 20
```

## 📊 Test Senaryoları

### Test 1: Stack Connectivity
```bash
# Stack durumunu kontrol et
display stack

# Ping testi yap
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.30.1
```

### Test 2: Stack Failover
```bash
# Master switchi kapat
system-view
stack slot 1 priority 0
quit

# Slaveın master olup olmadığını kontrol et
display stack
```

### Test 3: Link Aggregation
```bash
# Eth-Trunk durumunu kontrol et
display eth-trunk

# Member portları kontrol et
display eth-trunk 1
```

## 🔐 Güvenlik Kontrolleri

### Stack Security

```bash
# Stack port security durumunu kontrol et
display port-security

# Stack access controlü kontrol et
display current-configuration | include port-security
```

### VLAN Security

```bash
# VLAN access controlü kontrol et
display vlan brief

# VLAN interface güvenliğini kontrol et
display interface Vlanif10
display interface Vlanif20
```

## ⚠️ Önemli Notlar

1. **Stack Planning**: Stack topologysini planlayın
2. **Priority Settings**: Master/Slave prioritylerini doğru ayarlayın
3. **Port Configuration**: iStack portlarını doğru konfigüre edin
4. **VLAN Management**: Stack üzerinde VLANları yönetin
5. **Documentation**: Stack konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Monitoring**: Stack performanceını sürekli izleyin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- iStack teknolojisi ve konfigürasyonu
- Stack master/slave yönetimi
- Stack port konfigürasyonu
- Stack üzerinde VLAN yönetimi
- Link aggregation (LACP) konfigürasyonu
- Stack monitoring ve troubleshooting
- Stack security best practices 