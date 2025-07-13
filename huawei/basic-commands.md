# Huawei Temel Komutlar ve Cihaz Yönetimi

## 🔐 View Level Komutları

### System View'a Geçiş
```bash
# User view'dan system view'a geçiş
system-view
# veya
sys
```

### View'dan Çıkış
```bash
# System view'dan user view'a dönüş
quit
# veya
q

# Interface view'dan system view'a dönüş
quit
# veya
q
```

### Interface View'a Geçiş
```bash
# Interface view'a geçiş
interface GigabitEthernet0/0/1
# veya
int GigabitEthernet0/0/1
```

## 📋 Temel Display Komutları

### Cihaz Bilgileri
```bash
# Cihaz versiyon bilgileri
display version
# veya
dis ver

# Mevcut konfigürasyonu göster
display current-configuration
# veya
dis cur

# Kaydedilmiş konfigürasyonu göster
display saved-configuration
# veya
dis saved

# Interface durumlarını göster
display interface
# veya
dis int

# Belirli interface durumunu göster
display interface GigabitEthernet0/0/1
# veya
dis int GigabitEthernet0/0/1
```

### IP Bilgileri
```bash
# IP interface'lerini göster
display ip interface brief
# veya
dis ip int brief

# Routing tablosunu göster
display ip routing-table
# veya
dis ip routing-table

# ARP tablosunu göster
display arp
# veya
dis arp
```

## 💾 Konfigürasyon Yönetimi

### Konfigürasyon Kaydetme
```bash
# Konfigürasyonu kaydet
save
# veya
save configuration
```

### Konfigürasyon Yükleme
```bash
# Kaydedilmiş konfigürasyonu yükle
load saved-configuration
# veya
load saved
```

### Konfigürasyon Silme
```bash
# Kaydedilmiş konfigürasyonu sil
reset saved-configuration
# veya
reset saved
```

## 🔄 Reload ve Reset

### Cihazı Yeniden Başlatma
```bash
# Cihazı yeniden başlat
reboot

# Yeniden başlatma sırasında kaydetmeyi sor
reboot in 10
```

### Factory Reset
```bash
# Cihazı factory default'a döndür
reset saved-configuration
reboot
```

## 🔧 Hostname ve Banner

### Hostname Değiştirme
```bash
# Hostname'i değiştir
system-view
sysname Router1
quit
```

### Banner Konfigürasyonu
```bash
# Login banner'ı ayarla
system-view
header login "Bu cihaza yetkisiz erişim yasaktır#"
quit

# MOTD (Message of the Day) banner'ı
system-view
header shell "Sistem bakımı 22:00-02:00 arası yapılacaktır"
quit
```

## 🔍 Help Komutları

### Komut Yardımı
```bash
# Komut yardımını göster
?
# veya
help

# Belirli komutla başlayan komutları göster
display ?
# veya
dis ?

# Interface komutlarını göster
interface ?
```

### Tab Completion
```bash
# Komut tamamlama için Tab tuşunu kullan
display inter<Tab>
# Otomatik olarak "display interface" olur
```

## ⚡ Hızlı Komutlar

### Hızlı Erişim
```bash
# Son komutu tekrarla
# Up arrow tuşu

# Komut geçmişini göster
display history-command
# veya
dis history-command

# Terminal ayarlarını göster
display terminal
# veya
dis terminal
```

### Terminal Ayarları
```bash
# Terminal uzunluğunu ayarla
screen-length 0
# veya
screen-len 0

# Terminal genişliğini ayarla
screen-width 80
# veya
screen-wid 80
```

## 🚨 Debug Komutları

### Debug Başlatma
```bash
# IP routing debug'ını başlat
debugging ip routing

# Interface debug'ını başlat
debugging interface GigabitEthernet0/0/1

# OSPF debug'ını başlat
debugging ospf events
```

### Debug Durdurma
```bash
# Tüm debug'ları durdur
undo debugging all
# veya
undo debug all

# Belirli debug'ı durdur
undo debugging ip routing
```

## 📊 Monitoring Komutları

### Real-time Monitoring
```bash
# Interface istatistiklerini göster
display interface GigabitEthernet0/0/1 statistics
# veya
dis int GigabitEthernet0/0/1 statistics

# CPU kullanımını göster
display cpu-usage
# veya
dis cpu-usage

# Memory kullanımını göster
display memory-usage
# veya
dis memory-usage
```

## 🔐 Güvenlik Komutları

### Super Password
```bash
# Super password ayarla
system-view
super password cisco123
quit
```

### Console Password
```bash
# Console password ayarla
system-view
user-interface console 0
authentication-mode password
set authentication password cisco123
quit
```

### Telnet Password
```bash
# Telnet password ayarla
system-view
user-interface vty 0 4
authentication-mode password
set authentication password cisco123
quit
```

## 🔧 Huawei-Specific Features

### iStack Konfigürasyonu
```bash
# iStack port'ları ayarla
system-view
stack-port GigabitEthernet0/0/1 enable
stack-port GigabitEthernet0/0/2 enable
quit
```

### CSS Konfigürasyonu
```bash
# CSS port'ları ayarla
system-view
css-port GigabitEthernet0/0/1 enable
css-port GigabitEthernet0/0/2 enable
quit
```

## 📋 Undo Komutları

### Konfigürasyon Geri Alma
```bash
# Interface'i kapat
system-view
interface GigabitEthernet0/0/1
undo shutdown
quit

# IP address'i kaldır
system-view
interface GigabitEthernet0/0/1
undo ip address
quit
```

### Komut Silme
```bash
# Hostname'i sil
system-view
undo sysname
quit

# Banner'ı sil
system-view
undo header login
quit
```

## 🔍 Information Center

### Log Konfigürasyonu
```bash
# Information center'ı etkinleştir
system-view
info-center enable
info-center console channel
quit
```

### Log Seviyesi
```bash
# Log seviyesini ayarla
system-view
info-center console level informational
quit
```

## ⚠️ Önemli Notlar

1. **View Levels**: Her zaman doğru view level'da olduğunuzdan emin olun
2. **Interface Names**: Tam interface adlarını kullanın (GigabitEthernet0/0/1)
3. **Configuration Backup**: Değişiklik yapmadan önce backup alın
4. **Debug Commands**: Production ortamında debug komutlarını dikkatli kullanın
5. **Password Security**: Güçlü şifreler kullanın ve düzenli değiştirin
6. **Undo Commands**: Huawei'de `undo` komutu ile geri alma yapılır 