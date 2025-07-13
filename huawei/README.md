# Huawei Network Komutları Dokümantasyonu

Bu dokümantasyon, Huawei network cihazları için temel ve ileri seviye komutları içermektedir.

## 📁 Dosya Yapısı

- `basic-commands.md` - Temel komutlar ve cihaz yönetimi
- `interface-configuration.md` - Interface konfigürasyonları
- `routing-protocols.md` - Routing protokolleri (OSPF, ISIS, BGP)
- `vlan-configuration.md` - VLAN konfigürasyonları
- `security-commands.md` - Güvenlik komutları
- `troubleshooting.md` - Sorun giderme komutları
- `switch-commands.md` - Switch özel komutları
- `router-commands.md` - Router özel komutları

## 🚀 Hızlı Başlangıç

### Temel Komut Formatı
```bash
<device-name>system-view
[komut bloğu]
quit
save
```

### View Levels
- **User View**: `<device-name>`
- **System View**: `<device-name>system-view`
- **Interface View**: `<device-name>interface GigabitEthernet0/0/1`

## 📋 Önemli Kurallar

1. **System View**: Tüm global konfigürasyonlar system-view'da
2. **Interface Naming**: Huawei interface naming (GigabitEthernet0/0/1)
3. **VLAN Configuration**: VLAN batch create kullan
4. **Routing Protocols**: ISIS, OSPF area configuration
5. **ACL Configuration**: Advanced ACL ve Basic ACL ayrımı
6. **Security**: AAA, SSH, HTTPS Huawei syntax'ı ile

## 🔧 Verification Komutları

- `display current-configuration` - Mevcut konfigürasyonu göster
- `display interface` - Interface durumlarını göster
- `display ip routing-table` - Routing tablosunu göster
- `display version` - Cihaz bilgilerini göster

## ⚠️ Güvenlik Notları

- Debug komutlarını production ortamında dikkatli kullanın
- `undo debug all` komutunu unutmayın
- Konfigürasyon değişikliklerini test ortamında deneyin
- Backup almayı unutmayın

## 🔄 Huawei-Specific Features

- **Stack Configuration**: iStack, CSS configurations
- **VRRP**: Huawei VRRP implementation
- **Traffic Policy**: Traffic shaping ve QoS
- **WLAN AC**: Access Controller configurations

## 📊 Önemli Farklar

- Cisco'da `show` → Huawei'de `display`
- Cisco'da `write memory` → Huawei'de `save`
- Cisco'da `configure terminal` → Huawei'de `system-view`
- Cisco'da `exit` → Huawei'de `quit` 