# Output the path

```
<?php echo __FILE__ . '<br />'; ?>
```

# htaccess

```
RewriteEngine On    
```
Enables the RewriteEngine, which allows for URL rewriting and redirection.    

> Redirect http to https

```
RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```

> Redirect http to https with authentication

```
RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
SetEnvIf Request_URI ".*" AllowCountry
SetEnvIf Request_URI ".*" AllowRestApi
AuthUserFile "/srv/path/.htpasswd"
AuthName "Member Site"
AuthType BASIC
require valid-user
```

> Redirect all incoming requests to the canonical https WITHOUT the "www" subdomain. 

```
RewriteEngine On
RewriteCond %{HTTP_HOST} ^(www\.)(.+) [OR]
RewriteCond %{HTTPS} off
RewriteCond %{HTTP_HOST} ^(www\.)?(.+)
RewriteRule ^ https://%2%{REQUEST_URI} [R=301,L]
```

> Redirect all incoming requests to the canonical https WITH the "www" subdomain. 

```
RewriteEngine On
RewriteCond %{HTTP_HOST} !^$
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteCond %{HTTPS}s ^on(s)|
RewriteRule ^ http%1://www.%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```

> PHP file without the ".php" extension

```
RewriteEngine on 
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}\.php -f
RewriteRule ^(.*)$ $1.php
```
