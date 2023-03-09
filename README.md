# Output the path

```
<?php echo __FILE__ . '<br />'; ?>
```

# htaccess

RewriteEngine On    
Enables the RewriteEngine, which allows for URL rewriting and redirection.    

> Redirect HTTP to HTTPS

```
RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```

```
RewriteCond %{HTTPS} !on    
```
This line sets a condition that checks if the incoming request is not already using HTTPS. The %{HTTPS} variable is a server variable that is set to "on" if the connection is using HTTPS.

```
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]   
```
This line sets the rule for URL redirection. If the condition in the previous line is true (i.e., the request is not already using HTTPS), this rule will redirect the request to the same URL but using HTTPS instead. The %{HTTP_HOST} variable contains the hostname of the server, and the %{REQUEST_URI} variable contains the path and query string of the original request. The [R=301,L] flags at the end of the line indicate that the redirection should use a 301 status code (indicating a permanent redirect) and that this should be the last rule applied to the request.


> Redirect HTTP to HTTPS with password

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

```
SetEnvIf Request_URI ".*" AllowCountry    
SetEnvIf Request_URI ".*" AllowRestApi   
``` 
These two lines set environment variables based on the request URI. In this case, the two variables, "AllowCountry" and "AllowRestApi", are set for all URIs (".*"). The purpose of these variables is not clear from this code snippet alone, but they are likely used elsewhere in the server configuration to allow or deny access to specific URIs based on the value of the variables.   

```
AuthUserFile "/srv/path/.htpasswd"    
AuthName "Member Site"    
AuthType BASIC    
require valid-user    
```
These lines configure basic authentication for the server. The "AuthUserFile" directive specifies the location of the password file, which is used to authenticate users. The "AuthName" directive specifies the name of the authentication realm, which is displayed to users when they are prompted to enter their credentials. The "AuthType" directive specifies the type of authentication, which in this case is "BASIC". Finally, the "require valid-user" directive specifies that only authenticated users are allowed to access any URI not explicitly allowed by the environment variables.


> Force https without the "www" subdomain

```
RewriteCond %{HTTP_HOST} ^(www\.)(.+) [OR]
RewriteCond %{HTTPS} off
RewriteCond %{HTTP_HOST} ^(www\.)?(.+)
RewriteRule ^ https://%2%{REQUEST_URI} [R=301,L]
```

```
RewriteCond %{HTTP_HOST} ^(www\.)(.+) [OR]    
RewriteCond %{HTTPS} off    
```
These two lines set conditions for the redirection. The first condition checks if the request URL includes the "www" subdomain, and the second condition checks if the request is not already using HTTPS. The [OR] flag indicates that either condition can trigger the rule.

```
RewriteCond %{HTTP_HOST} ^(www\.)?(.+)    
RewriteRule ^ https://%2%{REQUEST_URI} [R=301,L]   
```
These two lines set the rule for URL redirection. If the conditions in the previous lines are true (i.e., the request includes the "www" subdomain or is not using HTTPS), this rule will redirect the request to the same URL without the "www" subdomain and using HTTPS instead. The %{HTTP_HOST} variable contains the hostname of the server, and the %{REQUEST_URI} variable contains the path and query string of the original request. The %2 variable refers to the second capture group from the previous RewriteCond line, which matches the domain name without the "www" subdomain. The [R=301,L] flags at the end of the line indicate that the redirection should use a 301 status code (indicating a permanent redirect) and that this should be the last rule applied to the request.


> Force https with the "www" subdomain

```
RewriteCond %{HTTP_HOST} !^$
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteCond %{HTTPS}s ^on(s)|
RewriteRule ^ http%1://www.%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```

```
RewriteCond %{HTTP_HOST} !^$    
```
This line sets a condition that checks if the request includes a hostname.    

```
RewriteCond %{HTTP_HOST} !^www\. [NC]   
```
This line sets a condition that checks if the request does not include the "www" subdomain, using a case-insensitive match ([NC] flag).   

```
RewriteCond %{HTTPS}s ^on(s)|   
```
This line sets a condition that checks if the request is using HTTPS. The %{HTTPS} variable is a server variable that is set to "on" if the connection is using HTTPS. The (s) captures the "s" in "https" if present.

```
RewriteRule ^ http%1://www.%{HTTP_HOST}%{REQUEST_URI} [R=301,L]   
```
This line sets the rule for URL redirection. If the conditions in the previous lines are true (i.e., the request does not include the "www" subdomain or is not using HTTPS), this rule will redirect the request to the same URL with the "www" subdomain and using HTTPS instead. The %{HTTP_HOST} variable contains the hostname of the server, and the %{REQUEST_URI} variable contains the path and query string of the original request. The %1 variable refers to the first capture group from the previous RewriteCond line, which captures the "s" in "https" if present. The [R=301,L] flags at the end of the line indicate that the redirection should use a 301 status code (indicating a permanent redirect) and that this should be the last rule applied to the request.    



> PHP file without the ".php" extension

```
RewriteEngine on 
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}\.php -f
RewriteRule ^(.*)$ $1.php
```

```
RewriteCond %{REQUEST_FILENAME} !-d   
```
This line sets a condition that the requested URL is not a directory on the server. This is necessary because the following rule only applies to URLs that are not directories.

```
RewriteCond %{REQUEST_FILENAME}\.php -f   
```
This line sets a condition that the requested URL with ".php" appended to the end is a valid file on the server. This is how mod_rewrite checks if the requested URL corresponds to a PHP file without the ".php" extension.    

```
RewriteRule ^(.*)$ $1.php   
```
This line sets the rule for URL rewriting. If the conditions in the previous lines are true, this rule will rewrite the requested URL to append ".php" to the end of the URL. The ^(.*)$ regular expression matches any URL, and the $1 in the replacement string refers to the captured URL. This effectively allows a request to a URL without the ".php" extension to be processed by the corresponding PHP file with the extension. For example, a request to /example would be rewritten to /example.php.
