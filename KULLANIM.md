# Docker PHP Development Environment
**MAMP/XAMPP/WAMP Alternatifi - macOS için Docker Development Ortamı**

## ✅ Kurulum Tamamlandı!

Sisteminiz hazır. Artık projelerinizi çalıştırabilirsiniz.

## 📁 Proje Klasör Yapısı

```
DockerDevPhp/
├── projects/
│   ├── php73-projects/         ← PHP 7.3 projelerinizi buraya atın
│   │   └── eski-proje/
│   │
│   ├── php74-projects/         ← PHP 7.4 projelerinizi buraya atın
│   │   ├── kurubuzmatik/      (Laravel projesi)
│   │   ├── test-proje/        (Basit PHP projesi)
│   │   └── diger-proje/
│   │
│   ├── php81-projects/         ← PHP 8.1 projelerinizi buraya atın
│   │   └── diger-proje/
│   │
│   ├── php83-projects/         ← PHP 8.3 projelerinizi buraya atın
│   │   ├── laravel-proje/
│   │   └── modern-proje/
│   │
│   └── php84-projects/         ← PHP 8.4 projelerinizi buraya atın
│       └── yeni-proje/
```

## 🚀 Kullanım

### 1. Projeleri Ekleme

**PHP 7.3 Projesi:**
```bash
# Projenizi kopyalayın
cp -r /path/to/eski-proje ./projects/php73-projects/

# Tarayıcıda açın
http://localhost:8000/eski-proje/public/
```

**PHP 7.4 Projesi:**
```bash
# Projenizi kopyalayın
cp -r /path/to/kurubuzmatik ./projects/php74-projects/

# Tarayıcıda açın
http://localhost:8001/kurubuzmatik/public/
```

**PHP 8.1 Projesi:**
```bash
# Projenizi kopyalayın
cp -r /path/to/diger-proje ./projects/php81-projects/

# Tarayıcıda açın
http://localhost:8002/diger-proje/public/
```

**PHP 8.3 Projesi:**
```bash
# Projenizi kopyalayın
cp -r /path/to/modern-proje ./projects/php83-projects/

# Tarayıcıda açın
http://localhost:8003/modern-proje/public/
```

**PHP 8.4 Projesi:**
```bash
# Projenizi kopyalayın
cp -r /path/to/yeni-proje ./projects/php84-projects/

# Tarayıcıda açın
http://localhost:8004/yeni-proje/public/
```

### 2. Laravel Projeleri

Laravel projeleri için `.env` dosyasını düzenleyin:

```env
# projects/php74-projects/kurubuzmatik/.env
APP_URL=http://localhost:8001/kurubuzmatik

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=kurubuzmatik
DB_USERNAME=dev_user
DB_PASSWORD=dev_password
```

Container içinde komut çalıştırma:
```bash
# Composer install
docker exec -it dockerdevphp_php74 bash
cd /var/www/html/kurubuzmatik
composer install
php artisan key:generate
php artisan migrate
```

### 3. Container Yönetimi

```bash
# Başlat
docker-compose up -d

# Durdur
docker-compose down

# Logları görüntüle
docker-compose logs -f

# Container'a gir
docker exec -it dockerdevphp_php73 bash    # PHP 7.3
docker exec -it dockerdevphp_php74 bash    # PHP 7.4
docker exec -it dockerdevphp_php81 bash    # PHP 8.1
docker exec -it dockerdevphp_php83 bash    # PHP 8.3
docker exec -it dockerdevphp_php84 bash    # PHP 8.4
```

## 🌐 Portlar ve Erişim

| Servis | URL | Port |
|--------|-----|------|
| **PHP 7.3 Projeleri** | http://localhost:8000/proje-adi/ | 8000 |
| **PHP 7.4 Projeleri** | http://localhost:8001/proje-adi/ | 8001 |
| **PHP 8.1 Projeleri** | http://localhost:8002/proje-adi/ | 8002 |
| **PHP 8.3 Projeleri** | http://localhost:8003/proje-adi/ | 8003 |
| **PHP 8.4 Projeleri** | http://localhost:8004/proje-adi/ | 8004 |
| **phpMyAdmin** | http://localhost:8080 | 8080 |
| **pgAdmin** | http://localhost:8081 | 8081 |
| **mongo-express** | http://localhost:8082 | 8082 |
| **MySQL** | localhost:3306 | 3306 |
| **PostgreSQL** | localhost:5432 | 5432 |
| **MongoDB** | localhost:27017 | 27017 |

## 🗄️ Veritabanı Bağlantıları

### MySQL
- **Host:** `mysql` (container içinden) veya `localhost` (host'tan)
- **Port:** `3306`
- **Kullanıcı:** `dev_user`
- **Şifre:** `dev_password`
- **Root Şifre:** `root`

### PostgreSQL
- **Host:** `postgresql` (container içinden) veya `localhost` (host'tan)
- **Port:** `5432`
- **Kullanıcı:** `dev_user`
- **Şifre:** `dev_password`
- **Database:** `dev_db`

### MongoDB
- **Host:** `mongodb` (container içinden) veya `localhost` (host'tan)
- **Port:** `27017`
- **Kullanıcı:** `dev_user`
- **Şifre:** `dev_password`
- **Database:** `dev_db`
- **Bağlantı String:** `mongodb://dev_user:dev_password@mongodb:27017/dev_db?authSource=admin`
- **mongo-express:** http://localhost:8082 (Kullanıcı: admin, Şifre: admin)
- **Not:** MongoDB PHP Extension sadece PHP 8.1+ için kurulmuştur (PHP 7.3 ve 7.4 desteklenmiyor)

## 📝 Örnekler

### Basit PHP Projesi
```php
// projects/php74-projects/test-proje/index.php
<?php
echo "PHP Version: " . phpversion();
phpinfo();
?>
```
**Erişim:** http://localhost:8001/test-proje/

### Laravel Projesi
```bash
# Proje yapısı
projects/php74-projects/kurubuzmatik/
├── app/
├── public/
│   └── index.php
├── .env
└── composer.json
```
**Erişim:** http://localhost:8001/kurubuzmatik/public/

## 🔧 Troubleshooting

### Container başlamıyor
```bash
docker-compose down
docker-compose up -d --build
```

### PHP çalışmıyor
```bash
# Nginx loglarını kontrol et
docker-compose logs nginx

# PHP loglarını kontrol et
docker-compose logs php74
```

### Veritabanına bağlanamıyorum
```bash
# MySQL container'ını kontrol et
docker exec -it dockerdevphp_mysql mysql -u root -proot

# PostgreSQL container'ını kontrol et
docker exec -it dockerdevphp_postgresql psql -U dev_user dev_db

# MongoDB container'ını kontrol et
docker exec -it dockerdevphp_mongodb mongosh -u dev_user -p dev_password --authenticationDatabase admin
```

## 🎯 Avantajlar

✅ **MAMP gibi kolay:** Projeyi klasöre at, tarayıcıda aç  
✅ **Çoklu PHP versiyonu:** PHP 7.3, 7.4, 8.1, 8.3 ve 8.4 aynı anda  
✅ **İzole ortam:** Her proje kendi veritabanı  
✅ **Port conflict yok:** 8001, 8002 gibi ayrı portlar  
✅ **Production-ready:** Sunucuya atmadan önce test et  

## 📚 Daha Fazla Bilgi

- Docker Compose dosyası: `docker-compose.yml`
- Nginx yapılandırması: `nginx/nginx.conf`
- PHP 7.3 ayarları: `php73/php.ini`
- PHP 7.4 ayarları: `php74/php.ini`
- PHP 8.1 ayarları: `php81/php.ini`
- PHP 8.3 ayarları: `php83/php.ini`
- PHP 8.4 ayarları: `php84/php.ini`

---

**İyi çalışmalar! 🚀**

