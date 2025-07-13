# Uygulama 7: Access Control List (ACL) Konfigürasyonu

## 🎯 Senaryo

ABC Şirketinin network altyapısında güvenlik için Access Control List (ACL) konfigürasyonu yapılması gerekiyor. Farklı network segmentleri için güvenlik politikaları uygulanacak, traffic filtering yapılacak ve network güvenliği sağlanacak.

## 📋 Gereksinimler

1. **Standard ACL**: Network segmentleri için
2. **Extended ACL**: Port ve protokol bazlı
3. **Named ACL**: Özel isimlendirilmiş ACLler
4. **Time-based ACL**: Zaman bazlı erişim kontrolü
5. **Reflexive ACL**: Dinamik güvenlik
6. **ACL logging**: ACL log kayıtları
7. **ACL optimization**: ACL performans optimizasyonu

## 🌐 Topoloji

```
[Internet] --- [Router] --- [Switch] --- [Server Network]
                    |              |
                    |          [User Network]
                    |
                [Management Network]
```

## 🔧 Adım Adım Çözüm

### 1. Standard ACL Oluşturma

```bash
# Routera bağlan
enable
configure terminal

# Standard ACL - Network erişim kontrolü
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 permit 192.168.20.0 0.0.0.255
access-list 10 deny any

# Standard ACL - Management erişimi
access-list 20 permit 192.168.99.0 0.0.0.255
access-list 20 deny any

# Standard ACL - Server erişimi
access-list 30 permit 192.168.100.0 0.0.0.255
access-list 30 deny any
```

### 2. Extended ACL Oluşturma

```bash
# Extended ACL - Web trafiği
access-list 100 permit tcp any any eq 80
access-list 100 permit tcp any any eq 443
access-list 100 permit tcp any any eq 8080
access-list 100 deny tcp any any

# Extended ACL - FTP trafiği
access-list 101 permit tcp any any eq 21
access-list 101 permit tcp any any eq 20
access-list 101 deny tcp any any

# Extended ACL - SSH trafiği
access-list 102 permit tcp any any eq 22
access-list 102 deny tcp any any

# Extended ACL - DNS trafiği
access-list 103 permit udp any any eq 53
access-list 103 permit tcp any any eq 53
access-list 103 deny udp any any
access-list 103 deny tcp any any
```

### 3. Named ACL Oluşturma

```bash
# Named ACL - Voice trafiği
ip access-list extended VOICE-TRAFFIC
permit udp any any range 16384 32767
permit tcp any any eq 5060
permit tcp any any eq 5061
deny ip any any
exit

# Named ACL - Video trafiği
ip access-list extended VIDEO-TRAFFIC
permit udp any any range 32768 65535
permit tcp any any eq 554
permit tcp any any eq 1935
deny ip any any
exit

# Named ACL - Management trafiği
ip access-list extended MANAGEMENT-TRAFFIC
permit tcp 192.168.99.0 0.0.0.255 any eq 22
permit tcp 192.168.99.0 0.0.0.255 any eq 23
permit tcp 192.168.99.0 0.0.0.255 any eq 80
permit tcp 192.168.99.0 0.0.0.255 any eq 443
deny ip any any
exit
```

### 4. Time-based ACL Oluşturma

```bash
# Time range tanımla
time-range WORK-HOURS
periodic weekdays 08:00 to 18:00
periodic weekend 09:00 to 17:00
exit

time-range MAINTENANCE-WINDOW
periodic daily 22:00 to 02:00
exit

# Time-based ACL
ip access-list extended TIME-BASED-ACL
permit tcp any any eq 80 time-range WORK-HOURS
permit tcp any any eq 443 time-range WORK-HOURS
deny tcp any any time-range WORK-HOURS
permit ip any any time-range MAINTENANCE-WINDOW
exit
```

### 5. Reflexive ACL Oluşturma

```bash
# Reflexive ACL - Outbound traffic
ip access-list extended OUTBOUND-TRAFFIC
permit tcp any any reflect TCP-TRAFFIC
permit udp any any reflect UDP-TRAFFIC
permit icmp any any reflect ICMP-TRAFFIC
deny ip any any
exit

# Reflexive ACL - Inbound traffic
ip access-list extended INBOUND-TRAFFIC
evaluate TCP-TRAFFIC
evaluate UDP-TRAFFIC
evaluate ICMP-TRAFFIC
deny ip any any
exit
```

### 6. ACL Interface Uygulama

```bash
# Interfacelerde ACL uygula
interface GigabitEthernet0/0/0
description Internet Link
ip access-group 100 in
ip access-group OUTBOUND-TRAFFIC out
exit

interface GigabitEthernet0/0/1
description User Network
ip access-group 101 in
ip access-group VOICE-TRAFFIC out
exit

interface GigabitEthernet0/0/2
description Server Network
ip access-group 102 in
ip access-group MANAGEMENT-TRAFFIC out
exit

interface GigabitEthernet0/0/3
description Management Network
ip access-group 20 in
ip access-group TIME-BASED-ACL out
exit
```

### 7. ACL Logging ve Monitoring

```bash
# ACL logging ayarları
access-list 100 permit tcp any any eq 80 log
access-list 100 permit tcp any any eq 443 log
access-list 100 deny tcp any any log

# Logging konfigürasyonu
logging console informational
logging buffered 16384
logging host 192.168.99.100
```

### 8. ACL Optimization

```bash
# ACL optimization için fragment handling
interface GigabitEthernet0/0/0
ip access-group 100 in
ip access-group fragment 100 in
exit

# ACL optimization için rate limiting
interface GigabitEthernet0/0/0
rate-limit input access-group 100 1000000 100000 100000 conform-action transmit exceed-action drop
exit
```

### 9. ACL Verification ve Testing

```bash
# ACL konfigürasyonunu göster
show access-lists

# ACL interfacelerini göster
show ip interface | include access-group

# ACL statisticslerini göster
show access-lists 100
```

### 10. Konfigürasyonu Kaydetme

```bash
# Konfigürasyonu kaydet
write memory
```

## ✅ Verification Komutları

### ACL Temel Kontrolleri

```bash
# ACL listesini göster
show access-lists

# Named ACLleri göster
show ip access-lists

# ACL interfacelerini göster
show ip interface | include access-group

# ACL statisticslerini göster
show access-lists 100
```

### ACL Performance Kontrolleri

```bash
# ACL hit countlarını göster
show access-lists | include matches

# ACL fragment handlingi göster
show ip interface | include fragment

# ACL rate limitingi göster
show interface GigabitEthernet0/0/0 | include rate-limit
```

### ACL Logging Kontrolleri

```bash
# ACL log kayıtlarını göster
show logging

# ACL log bufferını göster
show logging buffer

# ACL log hostunu göster
show logging host
```

### Time-based ACL Kontrolleri

```bash
# Time rangeleri göster
show time-range

# Time-based ACLleri göster
show ip access-lists | include time-range
```

## 🔍 Troubleshooting

### Yaygın ACL Sorunları

#### 1. ACL Uygulanmıyor
```bash
# ACL durumunu kontrol et
show access-lists

# Interface ACLlerini kontrol et
show ip interface | include access-group

# ACL syntaxını kontrol et
show running-config | include access-list
```

#### 2. ACL Performance Sorunu
```bash
# ACL hit countlarını kontrol et
show access-lists | include matches

# ACL optimization yap
# En çok kullanılan ACLleri başa taşı
```

#### 3. ACL Logging Sorunu
```bash
# Logging durumunu kontrol et
show logging

# Logging ayarlarını kontrol et
show running-config | include logging
```

## 📊 Test Senaryoları

### Test 1: ACL Connectivity
```bash
# ACLli interfaceden ping testi
ping 192.168.10.1

# ACLli interfaceden telnet testi
telnet 192.168.10.1 80

# ACLli interfaceden SSH testi
ssh 192.168.10.1
```

### Test 2: ACL Logging
```bash
# ACL log kayıtlarını kontrol et
show logging

# ACL log bufferını kontrol et
show logging buffer
```

### Test 3: Time-based ACL
```bash
# Time range durumunu kontrol et
show time-range

# Time-based ACLleri test et
ping 192.168.99.1
```

## 🔐 Güvenlik Kontrolleri

### ACL Security

```bash
# ACL güvenlik ayarlarını kontrol et
show access-lists

# ACL interface güvenliğini kontrol et
show ip interface | include access-group
```

### ACL Monitoring

```bash
# ACL monitoring ayarlarını kontrol et
show access-lists | include log

# ACL statisticslerini kontrol et
show access-lists 100
```

## ⚠️ Önemli Notlar

1. **ACL Planning**: ACLleri planlayın
2. **Order**: ACL sırasını doğru yapın
3. **Logging**: ACL logging kullanın
4. **Optimization**: ACLleri optimize edin
5. **Documentation**: ACL konfigürasyonunu dokümante edin
6. **Backup**: Konfigürasyon değişikliklerinden önce backup alın
7. **Testing**: ACLleri test edin

## 🎯 Öğrenme Çıktıları

Bu uygulamayı tamamladıktan sonra şunları öğrenmiş olacaksınız:
- Standard ve Extended ACL konfigürasyonu
- Named ACL oluşturma
- Time-based ACL konfigürasyonu
- Reflexive ACL kullanımı
- ACL interface uygulama
- ACL logging ve monitoring
- ACL optimization ve troubleshooting 