# Cisco Temel Komutlar ve Cihaz Yönetimi

## 🔐 Privilege Level Komutları

### Enable/Disable Mode
```bash
# User mode'dan Privileged mode'a geçiş
enable
# veya
en

# Privileged mode'dan User mode'a dönüş
disable
# veya
dis
```

### Configuration Mode
```bash
# Global configuration mode'a geçiş
configure terminal
# veya
conf t

# Global configuration mode'dan çıkış
end
# veya
exit

# Interface configuration mode'a geçiş
interface GigabitEthernet0/0/1
# veya
int Gi0/0/1

# Interface mode'dan çıkış
exit
```

## 📋 Temel Show Komutları

### Cihaz Bilgileri
```bash
# Cihaz versiyon bilgileri
show version
# veya
sh ver

# Mevcut konfigürasyonu göster
show running-config
# veya
sh run

# Kaydedilmiş konfigürasyonu göster
show startup-config
# veya
sh start

# Interface durumlarını göster
show interfaces
# veya
sh int

# Belirli interface durumunu göster
show interface GigabitEthernet0/0/1
# veya
sh int Gi0/0/1
```

### IP Bilgileri
```bash
# IP interface'lerini göster
show ip interface brief
# veya
sh ip int brief

# Routing tablosunu göster
show ip route
# veya
sh ip route

# ARP tablosunu göster
show arp
# veya
sh arp
```

## 💾 Konfigürasyon Yönetimi

### Konfigürasyon Kaydetme
```bash
# Konfigürasyonu kaydet
write memory
# veya
wr mem
# veya
copy running-config startup-config
# veya
copy run start
```

### Konfigürasyon Yükleme
```bash
# Kaydedilmiş konfigürasyonu yükle
copy startup-config running-config
# veya
copy start run
```

### Konfigürasyon Silme
```bash
# Kaydedilmiş konfigürasyonu sil
write erase
# veya
wr erase
# veya
erase startup-config
```

## 🔄 Reload ve Reset

### Cihazı Yeniden Başlatma
```bash
# Cihazı yeniden başlat
reload

# Yeniden başlatma sırasında kaydetmeyi sor
reload in 10
```

### Factory Reset
```bash
# Cihazı factory default'a döndür
write erase
reload
```

## 🔧 Hostname ve Banner

### Hostname Değiştirme
```bash
# Hostname'i değiştir
configure terminal
hostname Router1
end
```

### Banner Konfigürasyonu
```bash
# Login banner'ı ayarla
configure terminal
banner login #
Bu cihaza yetkisiz erişim yasaktır#
#
end

# MOTD (Message of the Day) banner'ı
configure terminal
banner motd #
Sistem bakımı 22:00-02:00 arası yapılacaktır
#
end
```

## 🔍 Help Komutları

### Komut Yardımı
```bash
# Komut yardımını göster
?
# veya
help

# Belirli komutla başlayan komutları göster
show ?
# veya
sh ?

# Interface komutlarını göster
interface ?
```

### Tab Completion
```bash
# Komut tamamlama için Tab tuşunu kullan
show inter<Tab>
# Otomatik olarak "show interface" olur
```

## ⚡ Hızlı Komutlar

### Hızlı Erişim
```bash
# Son komutu tekrarla
# Up arrow tuşu

# Komut geçmişini göster
show history
# veya
sh history

# Terminal ayarlarını göster
show terminal
# veya
sh terminal
```

### Terminal Ayarları
```bash
# Terminal uzunluğunu ayarla
terminal length 0
# veya
term len 0

# Terminal genişliğini ayarla
terminal width 80
# veya
term width 80
```

## 🚨 Debug Komutları

### Debug Başlatma
```bash
# IP routing debug'ını başlat
debug ip routing

# Interface debug'ını başlat
debug interface GigabitEthernet0/0/1

# OSPF debug'ını başlat
debug ip ospf events
```

### Debug Durdurma
```bash
# Tüm debug'ları durdur
no debug all
# veya
undebug all

# Belirli debug'ı durdur
no debug ip routing
```

## 📊 Monitoring Komutları

### Real-time Monitoring
```bash
# Interface istatistiklerini göster
show interface GigabitEthernet0/0/1 counters
# veya
sh int Gi0/0/1 counters

# CPU kullanımını göster
show processes cpu
# veya
sh proc cpu

# Memory kullanımını göster
show memory
# veya
sh mem
```

## 🔐 Güvenlik Komutları

### Enable Password
```bash
# Enable password ayarla
configure terminal
enable password cisco123
end
```

### Enable Secret (Encrypted)
```bash
# Encrypted enable password ayarla
configure terminal
enable secret cisco123
end
```

### Console Password
```bash
# Console password ayarla
configure terminal
line console 0
password cisco123
login
end
```

## ⚠️ Önemli Notlar

1. **Privilege Levels**: Her zaman doğru privilege level'da olduğunuzdan emin olun
2. **Interface Names**: Tam interface adlarını kullanın (GigabitEthernet0/0/1)
3. **Configuration Backup**: Değişiklik yapmadan önce backup alın
4. **Debug Commands**: Production ortamında debug komutlarını dikkatli kullanın
5. **Password Security**: Güçlü şifreler kullanın ve düzenli değiştirin 