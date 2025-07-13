# Uygulama 7: VRRP Konfigürasyonu

## 🎯 Senaryo

DEF Şirketinin network altyapısında VRRP (Virtual Router Redundancy Protocol) konfigürasyonu yapılması gerekiyor. İki router arasında yüksek erişilebilirlik sağlanacak, failover mekanizması kurulacak ve load balancing uygulanacak.

## 📋 Gereksinimler

1. **Router 1**: Master router (Priority 120)
2. **Router 2**: Backup router (Priority 100)
3. **Virtual IP**: 192.168.1.1
4. **VRRP Group 1**: Data VLAN için
5. **VRRP Group 2**: Voice VLAN için
6. **VRRP Group 3**: Management VLAN için
7. VRRP authentication uygulanacak
8. Preemption ayarlanacak

## 🌐 Topoloji

```
[PC-Data] --- [Switch] --- [Router-1] --- [Internet]
[PC-Voice]     |            |              |
[PC-Management] |        [VRRP Virtual IP] |
                |            |              |
                |        [Router-2] --- [Internet]
```

## 🔧 Adım Adım Çözüm

### 1. Router 1 (Master) VRRP Konfigürasyonu

```bash
# Router 1e bağlan
system-view

# Data VLAN interface konfigürasyonu
interface Vlanif10
description Data VLAN Interface
ip address 192.168.10.2 255.255.255.0
undo shutdown
quit

# Voice VLAN interface konfigürasyonu
interface Vlanif20
description Voice VLAN Interface
ip address 192.168.20.2 255.255.255.0
undo shutdown
quit

# Management VLAN interface konfigürasyonu
interface Vlanif30
description Management VLAN Interface
ip address 192.168.30.2 255.255.255.0
undo shutdown
quit

# VRRP Group 1 (Data VLAN)
interface Vlanif10
vrrp vrid 1 virtual-ip 192.168.10.1
vrrp vrid 1 priority 120
vrrp vrid 1 preempt-mode timer delay 5
vrrp vrid 1 authentication-mode md5 DEF123#
vrrp vrid 1 track interface GigabitEthernet0/0/1 reduced 30
quit

# VRRP Group 2 (Voice VLAN)
interface Vlanif20
vrrp vrid 2 virtual-ip 192.168.20.1
vrrp vrid 2 priority 120
vrrp vrid 2 preempt-mode timer delay 5
vrrp vrid 2 authentication-mode md5 DEF123#
vrrp vrid 2 track interface GigabitEthernet0/0/1 reduced 30
quit

# VRRP Group 3 (Management VLAN)
interface Vlanif30
vrrp vrid 3 virtual-ip 192.168.30.1
vrrp vrid 3 priority 120
vrrp vrid 3 preempt-mode timer delay 5
vrrp vrid 3 authentication-mode md5 DEF123#
vrrp vrid 3 track interface GigabitEthernet0/0/1 reduced 30
quit
```

### 2. Router 2 (Backup) VRRP Konfigürasyonu

```bash
# Router 2ye bağlan
system-view

# Data VLAN interface konfigürasyonu
interface Vlanif10
description Data VLAN Interface
ip address 192.168.10.3 255.255.255.0
undo shutdown
quit

# Voice VLAN interface konfigürasyonu
interface Vlanif20
description Voice VLAN Interface
ip address 192.168.20.3 255.255.255.0
undo shutdown
quit

# Management VLAN interface konfigürasyonu
interface Vlanif30
description Management VLAN Interface
ip address 192.168.30.3 255.255.255.0
undo shutdown
quit

# VRRP Group 1 (Data VLAN)
interface Vlanif10
vrrp vrid 1 virtual-ip 192.168.10.1
vrrp vrid 1 priority 100
vrrp vrid 1 preempt-mode timer delay 5
vrrp vrid 1 authentication-mode md5 DEF123#
vrrp vrid 1 track interface GigabitEthernet0/0/1 reduced 30
quit

# VRRP Group 2 (Voice VLAN)
interface Vlanif20
vrrp vrid 2 virtual-ip 192.168.20.1
vrrp vrid 2 priority 100
vrrp vrid 2 preempt-mode timer delay 5
vrrp vrid 2 authentication-mode md5 DEF123#
vrrp vrid 2 track interface GigabitEthernet0/0/1 reduced 30
quit

# VRRP Group 3 (Management VLAN)
interface Vlanif30
vrrp vrid 3 virtual-ip 192.168.30.1
vrrp vrid 3 priority 100
vrrp vrid 3 preempt-mode timer delay 5
vrrp vrid 3 authentication-mode md5 DEF123#
vrrp vrid 3 track interface GigabitEthernet0/0/1 reduced 30
quit
```

### 3. VRRP Load Balancing Konfigürasyonu

```bash
# Router 1de load balancing
interface Vlanif10
vrrp vrid 1 virtual-ip 192.168.10.1
vrrp vrid 1 priority 120
quit

interface Vlanif20
vrrp vrid 2 virtual-ip 192.168.20.1
vrrp vrid 2 priority 100
quit

interface Vlanif30
vrrp vrid 3 virtual-ip 192.168.30.1
vrrp vrid 3 priority 120
quit

# Router 2de load balancing
interface Vlanif10
vrrp vrid 1 virtual-ip 192.168.10.1
vrrp vrid 1 priority 100
quit

interface Vlanif20
vrrp vrid 2 virtual-ip 192.168.20.1
vrrp vrid 2 priority 120
quit

interface Vlanif30
vrrp vrid 3 virtual-ip 192.168.30.1
vrrp vrid 3 priority 100
quit
```

### 4. VRRP Authentication

```bash
# Router 1de authentication
interface Vlanif10
vrrp vrid 1 authentication-mode md5 DEF123#
quit

interface Vlanif20
vrrp vrid 2 authentication-mode md5 DEF123#
quit

interface Vlanif30
vrrp vrid 3 authentication-mode md5 DEF123#
quit

# Router 2de authentication
interface Vlanif10
vrrp vrid 1 authentication-mode md5 DEF123#
quit

interface Vlanif20
vrrp vrid 2 authentication-mode md5 DEF123#
quit

interface Vlanif30
vrrp vrid 3 authentication-mode md5 DEF123#
quit
```

### 5. VRRP Preemption ve Timers

```bash
# Router 1de preemption ayarları
interface Vlanif10
vrrp vrid 1 preempt-mode timer delay 5
vrrp vrid 1 timer advertise 1
quit

interface Vlanif20
vrrp vrid 2 preempt-mode timer delay 5
vrrp vrid 2 timer advertise 1
quit

interface Vlanif30
vrrp vrid 3 preempt-mode timer delay 5
vrrp vrid 3 timer advertise 1
quit

# Router 2de preemption ayarları
interface Vlanif10
vrrp vrid 1 preempt-mode timer delay 5
vrrp vrid 1 timer advertise 1
quit

interface Vlanif20
vrrp vrid 2 preempt-mode timer delay 5
vrrp vrid 2 timer advertise 1
quit

interface Vlanif30
vrrp vrid 3 preempt-mode timer delay 5
vrrp vrid 3 timer advertise 1
quit
```

### 6. VRRP Interface Tracking

```bash
# Router 1de interface tracking
interface Vlanif10
vrrp vrid 1 track interface GigabitEthernet0/0/1 reduced 30
quit

interface Vlanif20
vrrp vrid 2 track interface GigabitEthernet0/0/1 reduced 30
quit

interface Vlanif30
vrrp vrid 3 track interface GigabitEthernet0/0/1 reduced 30
quit

# Router 2de interface tracking
interface Vlanif10
vrrp vrid 1 track interface GigabitEthernet0/0/1 reduced 30
quit

interface Vlanif20
vrrp vrid 2 track interface GigabitEthernet0/0/1 reduced 30
quit

interface Vlanif30
vrrp vrid 3 track interface GigabitEthernet0/0/1 reduced 30
quit
```

### 7. VRRP Monitoring

```bash
# VRRP monitoring ayarları
info-center enable
info-center console channel
info-center console level informational

# VRRP log ayarları
vrrp log enable
```

### 8. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
save
```

## ✅ Verification Komutları

### VRRP Temel Kontrolleri

```bash
# VRRP durumunu göster
display vrrp

# VRRP interfacelerini göster
display vrrp interface Vlanif10

# VRRP statisticslerini göster
display vrrp statistics
```

### VRRP State Kontrolleri

```bash
# VRRP statelerini göster
display vrrp brief

# VRRP priorityleri göster
display vrrp interface Vlanif10 | include priority

# VRRP virtual IPleri göster
display vrrp interface Vlanif10 | include virtual-ip
```

### VRRP Authentication Kontrolleri

```bash
# VRRP authentication durumunu kontrol et
display vrrp interface Vlanif10 | include authentication

# VRRP authentication modeları göster
display vrrp interface Vlanif10 | include authentication-mode
```

### VRRP Tracking Kontrolleri

```bash
# VRRP tracking durumunu kontrol et
display vrrp interface Vlanif10 | include track

# VRRP tracking interfacelerini göster
display vrrp interface Vlanif10 | include track interface
```

## 🔍 Troubleshooting

### Yaygın VRRP Sorunları

#### 1. VRRP State Sorunu
```bash
# VRRP stateini kontrol et
display vrrp

# VRRP interface durumunu kontrol et
display interface Vlanif10

# VRRP priorityleri kontrol et
display vrrp interface Vlanif10 | include priority
```

#### 2. VRRP Authentication Sorunu
```bash
# Authentication durumunu kontrol et
display vrrp interface Vlanif10 | include authentication

# Authenticationı geçici olarak kapat
system-view
interface Vlanif10
undo vrrp vrid 1 authentication-mode
quit
```

#### 3. VRRP Preemption Sorunu
```bash
# Preemption durumunu kontrol et
display vrrp interface Vlanif10 | include preempt

# Preemptionı geçici olarak kapat
system-view
interface Vlanif10
undo vrrp vrid 1 preempt-mode
quit
```

## 📊 Test Senaryoları

### Test 1: VRRP Failover
```bash
# VRRP durumunu kontrol et
display vrrp

# Master routerı kapat
system-view
interface Vlanif10
shutdown
quit

# Backup routerın master olup olmadığını kontrol et
display vrrp
```

### Test 2: VRRP Load Balancing
```bash
# VRRP load balancing durumunu kontrol et
display vrrp brief

# Her VLANda master durumunu kontrol et
display vrrp interface Vlanif10
display vrrp interface Vlanif20
display vrrp interface Vlanif30
```

### Test 3: VRRP Interface Tracking
```bash
# Interface tracking durumunu kontrol et
display vrrp interface Vlanif10 | include track

# Track edilen interfacei kapat
system-view
interface GigabitEthernet0/0/1
shutdown
quit

# VRRP priority değişimini kontrol et
display vrrp interface Vlanif10
```

## 🔐 Güvenlik Kontrolleri

### VRRP Authentication

```bash
# Authentication ayarlarını kontrol et
display vrrp interface Vlanif10 | include authentication

# Authentication modeları kontrol et
display vrrp interface Vlanif10 | include authentication-mode
```

### VRRP Security

```bash
# VRRP security ayarlarını kontrol et
display vrrp interface Vlanif10

# VRRP log ayarlarını kontrol et
display info-center
```

## ⚠️ Önemli Notlar

1. **Priority Planning**: VRRP prioritylerini planlayın
2. **Authentication**: Production ortamında authentication kullanın
3. **Preemption**: Preemption ayarlarını doğru yapın
4. **Interface Tracking**: Interface tracking kullanın
5. **Documentation**: VRRP konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Monitoring**: VRRP durumunu sürekli izleyin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- VRRP protokolü ve konfigürasyonu
- VRRP master/backup yönetimi
- VRRP authentication
- VRRP preemption ve timers
- VRRP interface tracking
- VRRP load balancing
- VRRP troubleshooting teknikleri 