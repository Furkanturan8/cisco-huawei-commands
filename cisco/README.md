# Cisco Network Komutları Dokümantasyonu

Bu dokümantasyon, Cisco network cihazları için temel ve ileri seviye komutları içermektedir.

## 📁 Dosya Yapısı

- `basic-commands.md` - Temel komutlar ve cihaz yönetimi
- `interface-configuration.md` - Interface konfigürasyonları
- `routing-protocols.md` - Routing protokolleri (OSPF, EIGRP, BGP)
- `vlan-configuration.md` - VLAN konfigürasyonları
- `security-commands.md` - Güvenlik komutları
- `troubleshooting.md` - Sorun giderme komutları
- `switch-commands.md` - Switch özel komutları
- `router-commands.md` - Router özel komutları

## 🚀 Hızlı Başlangıç

### Temel Komut Formatı
```bash
! Açıklama
enable
configure terminal
[komut bloğu]
end
write memory
```

### Privilege Levels
- **User Mode**: `Router>`
- **Privileged Mode**: `Router#`
- **Global Config Mode**: `Router(config)#`
- **Interface Config Mode**: `Router(config-if)#`

## 📋 Önemli Kurallar

1. **Interface Naming**: Tam interface adlarını kullan (GigabitEthernet0/0/1)
2. **VLAN Configuration**: VLAN ve interface konfigürasyonlarını ayrı göster
3. **Routing Protocols**: OSPF, EIGRP, BGP için area/AS numaralarını belirt
4. **Access Lists**: Numbering conventions'a uygun numara ver
5. **Security**: AAA, SSH, HTTPS konfigürasyonlarını dahil et

## 🔧 Verification Komutları

- `show running-config` - Mevcut konfigürasyonu göster
- `show interfaces` - Interface durumlarını göster
- `show ip route` - Routing tablosunu göster
- `show version` - Cihaz bilgilerini göster

## ⚠️ Güvenlik Notları

- Debug komutlarını production ortamında dikkatli kullanın
- `no debug all` komutunu unutmayın
- Konfigürasyon değişikliklerini test ortamında deneyin
- Backup almayı unutmayın 