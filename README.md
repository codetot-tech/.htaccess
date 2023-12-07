# Tối ưu file .htaccess trên WordPress

Tổng hợp từ câu hỏi trên Bard Google.

## Mặc định

```
# BEGIN WordPress

RewriteEngine On
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]

# END WordPress
```

## Chuyển hướng www sang non-www:

```
RewriteCond %{HTTP_HOST} ^www\.example\.com$
RewriteRule ^(.*)$ http://example.com/$1 [R=301,L]
```


## Chuyển hướng non-www sang www:

```
RewriteCond %{HTTP_HOST} ^example\.com$
RewriteRule ^(.*)$ https://www.example.com/$1 [R=301,L]
```

## Bắt buộc sử dụng https:

```
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```

## Chống spam theo URL

```
# Block comment spam
RewriteCond %{REQUEST_METHOD} POST
RewriteCond %{REQUEST_URI} ^/wp-comments-post\.php$
RewriteCond %{HTTP_REFERER} !^http://%{HTTP_HOST} [NC]
RewriteRule ^.*$ - [F]
```

## Chặn truy cập file trái phép

```
# Prevent Direct File Access
<FilesMatch "(index\.php|wp-admin|wp-includes|wp-content|uploads|files)">
Order allow,deny
Deny from all
</FilesMatch>
```

## Bật nén GZIP cho các đuôi file phổ biến

- Bạn nên bật gzip compression cho các file tĩnh, chẳng hạn như file HTML, CSS, và JavaScript.
- Bạn nên tắt gzip compression cho các file động, chẳng hạn như file PHP.
- Bạn nên kiểm tra xem trình duyệt của người dùng có hỗ trợ gzip compression hay không.

```
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html
  AddOutputFilterByType DEFLATE text/css
  AddOutputFilterByType DEFLATE text/javascript
  AddOutputFilterByType DEFLATE application/x-javascript
  AddOutputFilterByType DEFLATE application/javascript
  AddOutputFilterByType DEFLATE application/xml
  AddOutputFilterByType DEFLATE image/svg+xml
</IfModule>
```

Một cách khác là force theo định dạng sử dụng `ForceType`:

```
ForceType application/x-gzip .html
ForceType application/x-gzip .css
ForceType application/x-gzip .js
ForceType application/x-gzip .jpg
ForceType application/x-gzip .jpeg
ForceType application/x-gzip .png
ForceType application/x-gzip .svg
```
