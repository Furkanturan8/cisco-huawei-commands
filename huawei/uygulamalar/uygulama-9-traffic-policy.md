# Uygulama 9: Traffic Policy Konfigürasyonu

## 🎯 Senaryo

DEF Şirketinin network altyapısında Traffic Policy konfigürasyonu yapılması gerekiyor. Traffic classification, traffic policing, traffic shaping ve QoS ayarları uygulanacak, bandwidth yönetimi yapılacak ve network optimizasyonu sağlanacak.

## 📋 Gereksinimler

1. **Voice Traffic**: Priority 5 (EF - Expedited Forwarding)
2. **Video Traffic**: Priority 4 (AF - Assured Forwarding)
3. **Data Traffic**: Priority 0 (BE - Best Effort)
4. **Management Traffic**: Priority 6 (CS6 - Network Control)
5. Traffic classification ve marking
6. Traffic policing ve shaping
7. Bandwidth allocation ve queuing

## 🌐 Topoloji

```
[Voice Phone] --- [Switch] --- [Router] --- [Internet]
[Video Camera]     |            |
[Data PC]          |        [Traffic Policy]
[Management PC]     |
```

## 🔧 Adım Adım Çözüm

### 1. Traffic Classifier Oluşturma

```bash
# Switche bağlan
system-view

# Voice traffic classifier
traffic classifier VOICE-TRAFFIC
if-match dscp ef
if-match acl 3000
quit

# Video traffic classifier
traffic classifier VIDEO-TRAFFIC
if-match dscp af41
if-match acl 3001
quit

# Data traffic classifier
traffic classifier DATA-TRAFFIC
if-match dscp default
if-match acl 3002
quit

# Management traffic classifier
traffic classifier MANAGEMENT-TRAFFIC
if-match dscp cs6
if-match acl 3003
quit
```

### 2. ACL Oluşturma

```bash
# Voice traffic ACL
acl 3000
rule 10 permit udp any any range 16384 32767
rule 20 permit tcp any any eq 5060
rule 30 permit tcp any any eq 5061
quit

# Video traffic ACL
acl 3001
rule 10 permit udp any any range 32768 65535
rule 20 permit tcp any any eq 554
rule 30 permit tcp any any eq 1935
quit

# Data traffic ACL
acl 3002
rule 10 permit ip any any
quit

# Management traffic ACL
acl 3003
rule 10 permit tcp any any eq 22
rule 20 permit tcp any any eq 23
rule 30 permit tcp any any eq 80
rule 40 permit tcp any any eq 443
quit
```

### 3. Traffic Behavior Oluşturma

```bash
# Voice traffic behavior
traffic behavior VOICE-BEHAVIOR
priority ef
car cir 1000000 pir 1500000 cbs 100000 pbs 150000 green pass yellow pass red discard
quit

# Video traffic behavior
traffic behavior VIDEO-BEHAVIOR
priority af
car cir 2000000 pir 3000000 cbs 200000 pbs 300000 green pass yellow pass red discard
quit

# Data traffic behavior
traffic behavior DATA-BEHAVIOR
priority be
car cir 5000000 pir 10000000 cbs 500000 pbs 1000000 green pass yellow pass red discard
quit

# Management traffic behavior
traffic behavior MANAGEMENT-BEHAVIOR
priority cs6
car cir 500000 pir 750000 cbs 50000 pbs 75000 green pass yellow pass red discard
quit
```

### 4. Traffic Policy Oluşturma

```bash
# Traffic policy oluştur
traffic policy QOS-POLICY

# Voice traffic policy
classifier VOICE-TRAFFIC behavior VOICE-BEHAVIOR
quit

# Video traffic policy
classifier VIDEO-TRAFFIC behavior VIDEO-BEHAVIOR
quit

# Data traffic policy
classifier DATA-TRAFFIC behavior DATA-BEHAVIOR
quit

# Management traffic policy
classifier MANAGEMENT-TRAFFIC behavior MANAGEMENT-BEHAVIOR
quit

exit
```

### 5. Traffic Policy Interface Uygulama

```bash
# Interfacelerde traffic policy uygula
interface GigabitEthernet0/0/1
description Uplink to Router
traffic-policy QOS-POLICY inbound
quit

# Access portlarda traffic policy
interface GigabitEthernet0/0/2
description Voice Phone Port
port link-type access
port default vlan 20
traffic-policy QOS-POLICY inbound
quit

interface GigabitEthernet0/0/3
description Video Camera Port
port link-type access
port default vlan 30
traffic-policy QOS-POLICY inbound
quit

interface GigabitEthernet0/0/4
description Data PC Port
port link-type access
port default vlan 10
traffic-policy QOS-POLICY inbound
quit

interface GigabitEthernet0/0/5
description Management PC Port
port link-type access
port default vlan 99
traffic-policy QOS-POLICY inbound
quit
```

### 6. Traffic Shaping Konfigürasyonu

```bash
# Traffic shaping policy
traffic policy SHAPING-POLICY

# Voice traffic shaping
classifier VOICE-TRAFFIC
behavior VOICE-SHAPING
quit

# Video traffic shaping
classifier VIDEO-TRAFFIC
behavior VIDEO-SHAPING
quit

# Data traffic shaping
classifier DATA-TRAFFIC
behavior DATA-SHAPING
quit

exit

# Shaping behaviorları
traffic behavior VOICE-SHAPING
queue ef
quit

traffic behavior VIDEO-SHAPING
queue af
quit

traffic behavior DATA-SHAPING
queue be
quit
```

### 7. Queue Management

```bash
# Queue konfigürasyonu
qos queue-profile QOS-QUEUE

# Voice queue
queue ef
queue-length 100
quit

# Video queue
queue af
queue-length 200
quit

# Data queue
queue be
queue-length 500
quit

exit

# Queue profileı interfacee uygula
interface GigabitEthernet0/0/1
qos queue-profile QOS-QUEUE
quit
```

### 8. DSCP Marking

```bash
# DSCP marking policy
traffic policy DSCP-MARKING

# Voice traffic marking
classifier VOICE-TRAFFIC
behavior VOICE-MARKING
quit

# Video traffic marking
classifier VIDEO-TRAFFIC
behavior VIDEO-MARKING
quit

# Data traffic marking
classifier DATA-TRAFFIC
behavior DATA-MARKING
quit

exit

# Marking behaviorları
traffic behavior VOICE-MARKING
remark dscp ef
quit

traffic behavior VIDEO-MARKING
remark dscp af41
quit

traffic behavior DATA-MARKING
remark dscp default
quit
```

### 9. Traffic Monitoring

```bash
# Traffic monitoring ayarları
info-center enable
info-center console channel
info-center console level informational

# Traffic policy monitoring
traffic-policy statistics enable
```

### 10. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
save
```

## ✅ Verification Komutları

### Traffic Policy Temel Kontrolleri

```bash
# Traffic policyleri göster
display traffic policy

# Traffic classifierları göster
display traffic classifier

# Traffic behaviorları göster
display traffic behavior
```

### Traffic Policy Performance Kontrolleri

```bash
# Traffic policy statisticslerini göster
display traffic policy statistics

# Traffic policy interfacelerini göster
display traffic policy interface

# Traffic policy queuelarını göster
display qos queue interface
```

### Traffic Policy Marking Kontrolleri

```bash
# DSCP markingi kontrol et
display traffic policy | include remark

# Traffic markingi kontrol et
display traffic policy interface | include remark
```

### Traffic Policy Monitoring

```bash
# Traffic policy countersları göster
display traffic policy statistics

# Traffic policy dropsları göster
display traffic policy | include discard

# Traffic policy conform/exceed/violate
display traffic policy | include pass
```

## 🔍 Troubleshooting

### Yaygın Traffic Policy Sorunları

#### 1. Traffic Policy Uygulanmıyor
```bash
# Traffic policy durumunu kontrol et
display traffic policy

# Traffic policy interfacelerini kontrol et
display traffic policy interface

# Traffic policy syntaxını kontrol et
display current-configuration | include traffic-policy
```

#### 2. Traffic Classification Sorunu
```bash
# Traffic classifierları kontrol et
display traffic classifier

# ACLleri kontrol et
display acl 3000
display acl 3001
display acl 3002
display acl 3003
```

#### 3. Traffic Behavior Sorunu
```bash
# Traffic behaviorları kontrol et
display traffic behavior

# CAR ayarlarını kontrol et
display traffic behavior | include car

# Priority ayarlarını kontrol et
display traffic behavior | include priority
```

## 📊 Test Senaryoları

### Test 1: Traffic Classification
```bash
# Traffic classification test et
ping 192.168.20.1
# DSCP markingi kontrol et
display traffic policy interface | include remark
```

### Test 2: Traffic Policing
```bash
# Traffic policing test et
# High bandwidth traffic gönder
# Policing statisticslerini kontrol et
display traffic policy statistics
```

### Test 3: Traffic Shaping
```bash
# Traffic shaping test et
# Shaping queuelarını kontrol et
display qos queue interface
```

## 🔐 Güvenlik Kontrolleri

### Traffic Policy Security

```bash
# Traffic policy güvenlik ayarlarını kontrol et
display traffic policy

# Traffic policy interface güvenliğini kontrol et
display traffic policy interface
```

### Traffic Policy Monitoring

```bash
# Traffic policy monitoring ayarlarını kontrol et
display traffic policy statistics

# Traffic policy log ayarlarını kontrol et
display info-center
```

## ⚠️ Önemli Notlar

1. **Traffic Classification**: Traffici doğru sınıflandırın
2. **Bandwidth Planning**: Bandwidthi planlayın
3. **Priority Settings**: Priorityleri doğru ayarlayın
4. **Monitoring**: Traffic policy performanceını sürekli izleyin
5. **Documentation**: Traffic policy konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Testing**: Traffic policy ayarlarını test edin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- Traffic classification ve marking
- Traffic policing ve shaping
- Queue management
- DSCP marking
- Traffic policy monitoring
- Traffic policy optimization
- Traffic policy troubleshooting teknikleri 