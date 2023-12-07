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
