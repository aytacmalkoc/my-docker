# Docker PHP Development Environment

Bu proje, macOS üzerinde PHP 7 ve 8 sürümleri için ayrı development ortamları sağlar. Docker kullanarak her PHP sürümü için izole edilmiş container'lar, MySQL ve PostgreSQL veritabanları ve Nginx reverse proxy ile yönetilen bir geliştirme ortamı sunar.

## Özellikler

- **PHP 7.3, 7.4, 8.1, 8.3 ve 8.4** için ayrı container'lar
- **MySQL 8.0**, **PostgreSQL 15** ve **MongoDB 7** veritabanları
- **Nginx** reverse proxy ile port bazlı routing
- Her PHP sürümü için gerekli extension'lar (PDO, MySQL, PostgreSQL, GD, mbstring, vs.)
- **Composer** kurulumu (her PHP sürümü için uyumlu versiyon)
- Laravel desteği (PHP 8 için)
- Veri kalıcılığı (volume'lar ile)

## Gereksinimler

- macOS (veya Linux/Windows)
- Docker Desktop (veya Docker Engine + Docker Compose)
- En az 4GB RAM (önerilen: 8GB)

## Kurulum

### 1. Docker Desktop Kurulumu

macOS için Docker Desktop'ı [resmi web sitesinden](https://www.docker.com/products/docker-desktop) indirip kurun.

### 2. Projeyi Başlatma

```bash
# Proje dizinine gidin
cd /Users/yunusaktas/Development/Projeler/DockerDevPhp

# Tüm container'ları başlatın
docker-compose up -d

# Container'ların durumunu kontrol edin
docker-compose ps
```

### 3. İlk Build

İlk çalıştırmada Docker image'ları oluşturulacak, bu biraz zaman alabilir (5-10 dakika).

```bash
# Log'ları takip etmek için
docker-compose up
```

## Port Yapılandırması

- **PHP 7.3 Projeleri**: http://localhost:8000
- **PHP 7.4 Projeleri**: http://localhost:8001
- **PHP 8.1 Projeleri**: http://localhost:8002
- **PHP 8.3 Projeleri**: http://localhost:8003
- **PHP 8.4 Projeleri**: http://localhost:8004
- **MySQL**: localhost:3306
- **PostgreSQL**: localhost:5432
- **MongoDB**: localhost:27017

## Veritabanı Bağlantı Bilgileri

### MySQL
- **Host**: mysql (container içinden) veya localhost (host'tan)
- **Port**: 3306
- **Kullanıcı**: dev_user
- **Şifre**: dev_password
- **Root Şifre**: root
- **Varsayılan Veritabanı**: dev_db

### PostgreSQL
- **Host**: postgresql (container içinden) veya localhost (host'tan)
- **Port**: 5432
- **Kullanıcı**: dev_user
- **Şifre**: dev_password
- **Varsayılan Veritabanı**: dev_db

### MongoDB
- **Host**: mongodb (container içinden) veya localhost (host'tan)
- **Port**: 27017
- **Kullanıcı**: dev_user
- **Şifre**: dev_password
- **Varsayılan Veritabanı**: dev_db
- **Bağlantı String**: `mongodb://dev_user:dev_password@mongodb:27017/dev_db?authSource=admin`
- **PHP Extension**: MongoDB PHP Extension sadece PHP 8.1, 8.3 ve 8.4 için kurulmuştur (PHP 7.3 ve 7.4 desteklenmiyor)

## Proje Yapısı

```
DockerDevPhp/
├── docker-compose.yml          # Ana Docker Compose yapılandırması
├── nginx/                       # Nginx yapılandırması
├── php73/                       # PHP 7.3 container yapılandırması
├── php74/                       # PHP 7.4 container yapılandırması
├── php81/                       # PHP 8.1 container yapılandırması
├── php83/                       # PHP 8.3 container yapılandırması
├── php84/                       # PHP 8.4 container yapılandırması
├── mysql/                       # MySQL başlangıç scriptleri
├── postgresql/                  # PostgreSQL başlangıç scriptleri
├── mongodb/                     # MongoDB başlangıç scriptleri
└── projects/                    # Projeleriniz buraya yerleştirilecek
    ├── php73-projects/         # PHP 7.3 projeleri
    ├── php74-projects/         # PHP 7.4 projeleri
    ├── php81-projects/         # PHP 8.1 projeleri
    ├── php83-projects/         # PHP 8.3 projeleri
    └── php84-projects/         # PHP 8.4 projeleri
```

## Kullanım

### Proje Ekleme

1. **PHP 7.3 Projesi**: Projenizi `projects/php73-projects/` klasörüne kopyalayın
2. **PHP 7.4 Projesi**: Projenizi `projects/php74-projects/` klasörüne kopyalayın
3. **PHP 8.1 Projesi**: Projenizi `projects/php81-projects/` klasörüne kopyalayın
4. **PHP 8.3 Projesi**: Projenizi `projects/php83-projects/` klasörüne kopyalayın
5. **PHP 8.4 Projesi**: Projenizi `projects/php84-projects/` klasörüne kopyalayın

### Laravel Projesi Ekleme (PHP 8.1, 8.3 veya 8.4)

```bash
# Laravel projesini oluşturun
cd projects/php83-projects
composer create-project laravel/laravel my-laravel-app

# Veya mevcut Laravel projenizi kopyalayın
cp -r /path/to/your/laravel/project ./my-laravel-app
```

Laravel projeleri için `public` klasörü otomatik olarak root olarak ayarlanmıştır.

### Container'lara Erişim

```bash
# PHP 7.3 container'ına giriş
docker exec -it dockerdevphp_php73 bash

# PHP 7.4 container'ına giriş
docker exec -it dockerdevphp_php74 bash

# PHP 8.1 container'ına giriş
docker exec -it dockerdevphp_php81 bash

# PHP 8.3 container'ına giriş
docker exec -it dockerdevphp_php83 bash

# PHP 8.4 container'ına giriş
docker exec -it dockerdevphp_php84 bash

# MySQL container'ına giriş
docker exec -it dockerdevphp_mysql bash

# PostgreSQL container'ına giriş
docker exec -it dockerdevphp_postgresql bash

# MongoDB container'ına giriş
docker exec -it dockerdevphp_mongodb bash
```

### Composer Kullanımı

```bash
# PHP 8.4 container'ında Composer çalıştırma
docker exec -it dockerdevphp_php84 composer install

# Veya container içine girip çalıştırın
docker exec -it dockerdevphp_php84 bash
cd /var/www/html
composer install
```

### Veritabanı İşlemleri

#### MySQL

```bash
# MySQL'e bağlanma
docker exec -it dockerdevphp_mysql mysql -u dev_user -pdev_password dev_db

# Veya host'tan
mysql -h localhost -P 3306 -u dev_user -pdev_password dev_db
```

#### PostgreSQL

```bash
# PostgreSQL'e bağlanma
docker exec -it dockerdevphp_postgresql psql -U dev_user -d dev_db

# Veya host'tan
psql -h localhost -p 5432 -U dev_user -d dev_db
```

#### MongoDB

```bash
# MongoDB'ye bağlanma (container içinden)
docker exec -it dockerdevphp_mongodb mongosh -u dev_user -p dev_password --authenticationDatabase admin

# Veya host'tan (mongosh kuruluysa)
mongosh mongodb://dev_user:dev_password@localhost:27017/dev_db?authSource=admin

# PHP'den MongoDB kullanımı (MongoDB PHP Extension ile)
# Not: MongoDB PHP Extension sadece PHP 8.1+ için kurulmuştur (PHP 7.3 ve 7.4 desteklenmiyor)
# Bağlantı: mongodb://dev_user:dev_password@mongodb:27017/dev_db?authSource=admin
```

## Yaygın Komutlar

```bash
# Tüm container'ları başlat
docker-compose up -d

# Container'ları durdur
docker-compose stop

# Container'ları durdur ve kaldır
docker-compose down

# Container'ları durdur, kaldır ve volume'ları sil (DİKKAT: Veriler silinir!)
docker-compose down -v

# Log'ları görüntüle
docker-compose logs -f

# Belirli bir servisin log'larını görüntüle
docker-compose logs -f php83

# Container'ları yeniden build et
docker-compose build

# Container'ları yeniden build et ve başlat
docker-compose up -d --build
```

## PHP Yapılandırması

Her PHP sürümü için özel `php.ini` dosyaları mevcuttur:
- `php73/php.ini`
- `php74/php.ini`
- `php81/php.ini`
- `php83/php.ini`
- `php84/php.ini`

Bu dosyaları düzenleyip container'ı yeniden başlatarak değişiklikleri uygulayabilirsiniz:

```bash
docker-compose restart php73
docker-compose restart php74
docker-compose restart php81
docker-compose restart php83
docker-compose restart php84
```

## Sorun Giderme

### Port Çakışması

Eğer portlar zaten kullanılıyorsa, `docker-compose.yml` dosyasındaki port numaralarını değiştirebilirsiniz:

```yaml
ports:
  - "8001:80"  # Sol taraf host portu, sağ taraf container portu
```

### Container Başlamıyor

```bash
# Log'ları kontrol edin
docker-compose logs [servis_adı]

# Container'ı yeniden build edin
docker-compose build --no-cache [servis_adı]
docker-compose up -d [servis_adı]
```

### Veritabanı Bağlantı Sorunu

Container'lar arası iletişim için host adı olarak container adını kullanın:
- MySQL: `mysql` (container içinden) veya `localhost` (host'tan)
- PostgreSQL: `postgresql` (container içinden) veya `localhost` (host'tan)
- MongoDB: `mongodb` (container içinden) veya `localhost` (host'tan)

### PHP Extension Eksik

Eğer bir PHP extension'a ihtiyacınız varsa, ilgili `Dockerfile` dosyasını düzenleyin ve container'ı yeniden build edin:

```bash
docker-compose build php83
docker-compose up -d php83
```

### Dosya İzin Sorunları

macOS'ta bazen dosya izinleri sorun yaratabilir. Container içinde dosya oluşturulduğunda izinleri düzeltmek için:

```bash
docker exec -it dockerdevphp_php84 chown -R www-data:www-data /var/www/html
```

## Performans İpuçları

1. **Docker Desktop Ayarları**: Docker Desktop'ta Resources ayarlarından CPU ve Memory limitlerini artırın
2. **Volume Mounting**: Projelerinizi doğrudan `projects/` klasörüne yerleştirin, böylece hot-reload çalışır
3. **OPcache**: PHP 7 ve 8'de OPcache aktif, production'da devre dışı bırakın

## Güvenlik Notları

⚠️ **ÖNEMLİ**: Bu yapılandırma sadece development ortamı içindir!

- Production ortamında kullanmayın
- Şifreler varsayılan değerlerdir, production'da mutlaka değiştirin
- `display_errors = On` ayarı production'da `Off` yapılmalıdır
- Veritabanı portlarını production'da dışarıya açmayın

## Destek

Sorun yaşarsanız:
1. `docker-compose logs` ile log'ları kontrol edin
2. Container'ların çalıştığından emin olun: `docker-compose ps`
3. Port'ların boş olduğunu kontrol edin: `lsof -i :8001`

## Lisans

Bu proje kişisel kullanım içindir.

