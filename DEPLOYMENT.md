# GA System - Deployment Guide

Panduan lengkap untuk deploy GA System ke production.

## 📋 Persiapan Deployment

### 1. Persyaratan Server
- **OS**: Ubuntu 18.04+ / CentOS 7+ / Windows Server 2016+
- **Node.js**: v14.0.0 atau lebih baru
- **MySQL**: v5.7 atau lebih baru
- **RAM**: Minimum 2GB, Recommended 4GB+
- **Storage**: Minimum 10GB free space
- **Network**: Port 3000 (atau sesuai konfigurasi)

### 2. Persiapan Database
```sql
-- Buat database
CREATE DATABASE ga_system CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Buat user database (opsional, untuk keamanan)
CREATE USER 'ga_user'@'localhost' IDENTIFIED BY 'secure_password_here';
GRANT ALL PRIVILEGES ON ga_system.* TO 'ga_user'@'localhost';
FLUSH PRIVILEGES;
```

## 🚀 Deployment Steps

### Step 1: Upload Files
```bash
# Upload semua file project ke server
# Pastikan struktur folder tetap sama
scp -r ga-system/ user@server:/path/to/deployment/
```

### Step 2: Install Dependencies
```bash
cd /path/to/deployment/ga-system
npm install --production
```

### Step 3: Environment Configuration
```bash
# Copy dan edit environment file
cp .env.example .env
nano .env
```

Edit file `.env` dengan konfigurasi production:
```env
# Database Configuration
DB_HOST=localhost
DB_USER=ga_user
DB_PASSWORD=secure_password_here
DB_NAME=ga_system
DB_PORT=3306

# Server Configuration
PORT=3000

# Security Keys (WAJIB DIGANTI!)
JWT_SECRET=your_very_long_and_secure_jwt_secret_key_here
ENCRYPTION_KEY=your_32_character_encryption_key_here

# Production Settings
NODE_ENV=production
```

### Step 4: Database Setup
```bash
# Import database schema dan data
mysql -u ga_user -p ga_system < scripts/enhancedDatabaseSchema.sql
node scripts/seedData.js
```

### Step 5: Test Application
```bash
# Test run aplikasi
npm start

# Cek di browser: http://server-ip:3000
```

## 🔧 Production Setup dengan PM2

### Install PM2
```bash
npm install -g pm2
```

### Create PM2 Ecosystem File
Buat file `ecosystem.config.js`:
```javascript
module.exports = {
  apps: [{
    name: 'ga-system',
    script: 'server.js',
    instances: 'max',
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'development'
    },
    env_production: {
      NODE_ENV: 'production',
      PORT: 3000
    },
    error_file: './logs/err.log',
    out_file: './logs/out.log',
    log_file: './logs/combined.log',
    time: true
  }]
};
```

### Start dengan PM2
```bash
# Buat folder logs
mkdir logs

# Start aplikasi
pm2 start ecosystem.config.js --env production

# Save PM2 configuration
pm2 save

# Setup PM2 startup
pm2 startup
```

## 🌐 Nginx Configuration (Opsional)

### Install Nginx
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install nginx

# CentOS/RHEL
sudo yum install nginx
```

### Nginx Config
Buat file `/etc/nginx/sites-available/ga-system`:
```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Enable Site
```bash
sudo ln -s /etc/nginx/sites-available/ga-system /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

## 🔒 SSL Certificate (Let's Encrypt)

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Get certificate
sudo certbot --nginx -d your-domain.com

# Auto renewal
sudo crontab -e
# Add: 0 12 * * * /usr/bin/certbot renew --quiet
```

## 📊 Monitoring & Maintenance

### PM2 Monitoring
```bash
# Monitor aplikasi
pm2 monit

# Lihat logs
pm2 logs ga-system

# Restart aplikasi
pm2 restart ga-system

# Reload aplikasi (zero downtime)
pm2 reload ga-system
```

### Database Backup
```bash
# Buat script backup harian
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
mysqldump -u ga_user -p ga_system > /backup/ga_system_$DATE.sql
find /backup -name "ga_system_*.sql" -mtime +7 -delete
```

### Log Rotation
```bash
# Setup logrotate untuk PM2 logs
sudo nano /etc/logrotate.d/pm2

/path/to/ga-system/logs/*.log {
    daily
    missingok
    rotate 52
    compress
    notifempty
    create 644 www-data www-data
    postrotate
        pm2 reloadLogs
    endscript
}
```

## 🔧 Troubleshooting

### Common Issues

1. **Port sudah digunakan**
   ```bash
   sudo lsof -i :3000
   sudo kill -9 <PID>
   ```

2. **Database connection error**
   - Cek kredensial di `.env`
   - Pastikan MySQL service running
   - Cek firewall settings

3. **Permission errors**
   ```bash
   sudo chown -R www-data:www-data /path/to/ga-system
   sudo chmod -R 755 /path/to/ga-system
   ```

4. **Memory issues**
   - Monitor dengan `htop`
   - Adjust PM2 instances
   - Add swap space jika perlu

### Performance Optimization

1. **Enable Gzip di Nginx**
   ```nginx
   gzip on;
   gzip_vary on;
   gzip_min_length 1024;
   gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
   ```

2. **Database Optimization**
   ```sql
   -- Add indexes untuk performa
   CREATE INDEX idx_requests_user_id ON requests(user_id);
   CREATE INDEX idx_requests_status ON requests(status);
   CREATE INDEX idx_items_category ON items(category_id);
   ```

## 📞 Support

Jika mengalami masalah deployment:
1. Cek logs aplikasi: `pm2 logs ga-system`
2. Cek logs Nginx: `sudo tail -f /var/log/nginx/error.log`
3. Cek logs MySQL: `sudo tail -f /var/log/mysql/error.log`

## 🔄 Update Deployment

```bash
# Backup database
mysqldump -u ga_user -p ga_system > backup_before_update.sql

# Pull latest code
git pull origin main

# Install new dependencies
npm install --production

# Restart aplikasi
pm2 reload ga-system
```