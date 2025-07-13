# Network Komutları Dokümantasyonu

Bu repository, Cisco ve Huawei network cihazları için kapsamlı komut dokümantasyonu içermektedir.

## 📁 Proje Yapısı

```
cisco/
├── README.md                   # Cisco ana dokümantasyonu
├── uygulamalar/                # Gerçek dünya senaryoları
├── basic-commands.md           # Temel komutlar ve cihaz yönetimi
├── interface-configuration.md  # Interface konfigürasyonları
├── routing-protocols.md        # Routing protokolleri (OSPF, EIGRP, BGP)
└── vlan-configuration.md       # VLAN konfigürasyonları

huawei/
├── README.md                   # Huawei ana dokümantasyonu
├── uygulamalar/                # Gerçek dünya senaryoları
├── basic-commands.md           # Temel komutlar ve cihaz yönetimi
├── interface-configuration.md  # Interface konfigürasyonları
├── routing-protocols.md        # Routing protokolleri (OSPF, ISIS, BGP)
└── vlan-configuration.md       # VLAN konfigürasyonları
```

## 🚀 Hızlı Başlangıç

### Cisco Komutları
- **Temel Format**: `enable` → `configure terminal` → `[komutlar]` → `end` → `write memory`
- **Privilege Levels**: User Mode → Privileged Mode → Global Config Mode
- **Interface Naming**: `GigabitEthernet0/0/1`, `FastEthernet0/1`

### Huawei Komutları
- **Temel Format**: `system-view` → `[komutlar]` → `quit` → `save`
- **View Levels**: User View → System View → Interface View
- **Interface Naming**: `GigabitEthernet0/0/1`, `FastEthernet0/1`

## 📋 İçerik

### Cisco Dokümantasyonu
- **Temel Komutlar**: Privilege levels, show komutları, konfigürasyon yönetimi
- **Interface Konfigürasyonu**: IP address, bandwidth, security features
- **Routing Protokolleri**: OSPF, EIGRP, BGP, static routing
- **VLAN Konfigürasyonu**: VLAN oluşturma, trunk/access port, VTP

### Huawei Dokümantasyonu
- **Temel Komutlar**: View levels, display komutları, undo komutları
- **Interface Konfigürasyonu**: IP address, VRRP, traffic policy
- **Routing Protokolleri**: OSPF, IS-IS, BGP, static routing
- **VLAN Konfigürasyonu**: VLAN batch create, iStack, CSS

## 🔧 Önemli Farklar

| Özellik | Cisco | Huawei |
|---------|-------|--------|
| Show Komutları | `show` | `display` |
| Konfigürasyon Kaydetme | `write memory` | `save` |
| Global Config | `configure terminal` | `system-view` |
| Çıkış | `exit` | `quit` |
| Geri Alma | `no` | `undo` |

## 📚 Öğrenme Sırası

1. **Temel Komutlar**: Her iki platform için temel komutları öğrenin
2. **Interface Konfigürasyonu**: IP address, interface durumu
3. **VLAN Konfigürasyonu**: Network segmentasyonu
4. **Routing Protokolleri**: Network connectivity
5. **Security Features**: Güvenlik konfigürasyonları

## ⚠️ Güvenlik Notları

- Production ortamında debug komutlarını dikkatli kullanın
- Konfigürasyon değişikliklerinden önce backup alın
- Test ortamında deneyin
- Güçlü şifreler kullanın

## 🎯 Hedef Kitle

- IT departmanı stajyerleri
- Network mühendisleri
- Sistem yöneticileri
- Network öğrencileri

**Not**: Bu dokümantasyon eğitim amaçlıdır. Production ortamında kullanmadan önce test ediniz. 