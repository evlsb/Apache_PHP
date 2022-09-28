### Комманды Apache 2.4 ###
* ***httpd.exe -k install***  : _Установить_
* ***httpd.exe -k start***    : _Запустить_
* ***httpd.exe -k restart***  : _Перезапустить_
* ***httpd -k shutdown***     : _Остановить_
* ***httpd -k stop***         : _Остановитть_
* ***httpd -k uninstall***    : _Удалить_ 
* ***sc delete Apache2.4***    : _Удалить_ 

# Подключение PHP8 в качестве модуля Apache

## Подготовительные работы
---
### Структура папок
 - ***tmp_GIT***
    - ***Apache24*** _(Файлы Apache 2.4)_
        - ***bin*** _(Файл httpd)_
        - ***conf***
            - ***httpd.conf*** _(Файл конфигурации)_
    - ***log*** _(файлы с логами)_
    - ***php*** _(Файлы php)_
        - ***php.ini*** _(Файл конфигурации)_
        - ***php8apache2_4.dll*** _(Библиотека php)_
    - ***tmp***
    - ***www*** _(файлы сайта)_
        - ***index.html***
        - ***index.php***
---

 ***В системную переменную PATH добавляем пути:***
 ```python

D:\WEB\tmp_GIT\php_th

```
 ```python

D:\WEB\tmp_GIT\Apache24\bin

```


## Установка Apache 2.4

### Изменения конфигурационного файла

_Заменяем:_

|строка|с|на|пояснение|
|-----|-----|-----|-----|
|37|***Define SRVROOT "c:/Apache24"***|***Define SRVROOT "D:/WEB/tmp_GIT/Apache24"***|_Путь папки Apache_|
|217|***ServerAdmin admin@example.com"***|***ServerAdmin localhost:80"***|_адрес сайта_|
|250|***DocumentRoot "${SRVROOT}/htdocs"***|***DocumentRoot "D:/WEB/tmp_GIT/www***|_каталог сайта_|
|251|***DocumentRoot "${SRVROOT}/htdocs"***|***<Directory "D:/WEB/tmp_GIT/www">***|_каталог сайта_|

_Добавляем в конец файла строки:_

```python

LoadModule php_module "D:/WEB/tmp_GIT/php/php8apache2_4.dll"  
PHPIniDir "D:/WEB/tmp_GIT/php"

```

_Обновляем директиву "IfModule mime_module":_
```python

<IfModule mime_module>
    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz
    AddType application/x-httpd-php .php
    AddType application/x-httpd-php-source .phps
</IfModule>

```

_Обновляем директиву "IfModule dir_module":_
```python

<IfModule dir_module>
    DirectoryIndex index.html index.php
</IfModule>

```

_Устанавливаем сервис Apache (от имени Администратора):_
```python

httpd.exe -k install

```

### Добавление виртуальных хостов

В файле "C:\Windows\System32\drivers\etc\hosts" добавим 2 строки:

```python

127.0.0.1 host1.localhost
127.0.0.1 host2.localhost

```

Добавим 2 папки хостов и логов

- ***log*** _(файлы с логами)_
    - ***host1.localhost*** _(логи host1)_
    - ***host2.localhost*** _(логи host2)_
- ***www*** _(файлы сайта)_
    - ***host1.localhost*** _(файлы host1)_
        - ***index.html***
        - ***index.php***
    - ***host2.localhost*** _(файлы host2)_
        - ***index.html***
        - ***index.php***
    - ***index.html***
    - ***index.php***


_Расскоментируем строку 511:_
```python

#Include conf/extra/httpd-vhosts.conf

```

Правим файл "D:\WEB\tmp_GIT\Apache24\conf\extra\httpd-vhosts.conf"
```python

<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "D:/WEB/tmp_GIT/www/host1.localhost"
    ServerName host1.localhost
    ServerAlias www.host1.localhost
    ErrorLog "D:/WEB/tmp_GIT/log/host1.localhost/error.log"
    CustomLog "D:/WEB/tmp_GIT/log/host1.localhost/access.log" common
</VirtualHost>

<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host2.example.com
    DocumentRoot "D:/WEB/tmp_GIT/www/host1.localhost"
    ServerName host2.localhost
    ServerAlias www.host2.localhost
    ErrorLog "D:/WEB/tmp_GIT/log/host2.localhost/error.log"
    CustomLog "D:/WEB/tmp_GIT/log/host2.localhost/access.log" common
</VirtualHost>

```

# Apache + mod_fcgi

## Подготовительные работы
---
### Структура папок
 - ***tmp_GIT***
    - ***Apache24*** _(Файлы Apache 2.4)_
        - ***bin*** _(Файл httpd)_
        - ***conf***
            - ***httpd.conf*** _(Файл конфигурации)_
    - ***php*** _(Файлы php)_
        - ***php.ini*** _(Файл конфигурации)_
    - ***tmp***
    - ***htdocs*** _(файлы сайта)_
        - ***index.php***
---

Копирем в папку "Apache24\modules" файл mod_fcgid.so

### Изменения конфигурационного файла

_Заменяем:_

|строка|с|на|пояснение|
|-----|-----|-----|-----|
|37|***Define SRVROOT "c:/Apache24"***|***Define SRVROOT "D:/WEB/tmp_GIT/Apache24"***|_Путь папки Apache_|
|250|***DocumentRoot "${SRVROOT}/htdocs"***|***DocumentRoot "D:/WEB/tmp_GIT/www***|_каталог сайта_|
|251|***DocumentRoot "${SRVROOT}/htdocs"***|***<Directory "D:/WEB/tmp_GIT/www">***|_каталог сайта_|

Подключаем модуль FastCGI
```python

LoadModule fcgid_module modules/mod_fcgid.so

```

_Обновляем директиву "IfModule dir_module":_
```python

<IfModule dir_module>
    DirectoryIndex index.html index.php
</IfModule>

```

_Подключаем виртуальные хосты:_
```python

# Virtual hosts
Include conf/extra/httpd-vhosts.conf

```

_Подключаем внешний файл настройки FastCGI:_
```python

# FastCGI
Include conf/extra/httpd-fastcgi.conf

```

_Настраиваем внений файл conf/extra/httpd-fastcgi.conf:_
```python

FcgidInitialEnv PATH "D:/WEB/tmp_GIT/php_nts;C:/WINDOWS/system32;C:/WINDOWS;C:/WINDOWS/System32/Wbem;"
FcgidInitialEnv SystemRoot "C:/Windows"
FcgidInitialEnv SystemDrive "C:"
FcgidInitialEnv TEMP "C:/WINDOWS/Temp"
FcgidInitialEnv TMP "C:/WINDOWS/Temp"
FcgidInitialEnv windir "C:/WINDOWS"
FcgidIOTimeout 64
FcgidConnectTimeout 16
FcgidMaxRequestsPerProcess 1000 
FcgidMaxProcesses 50 
FcgidMaxRequestLen 8131072
# Location php.ini:
FcgidInitialEnv PHPRC "D:/WEB/tmp_GIT/php_nts"
FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 1000

<Files ~ "\.php$>"
  AddHandler fcgid-script .php
  FcgidWrapper "D:/WEB/tmp_GIT/php_nts/php-cgi.exe" .php
</Files>

```

_В файле php.ini настраиваем временную зону:_
```python

date.timezone = "Asia/Yekaterinburg"

```

_Настраиваем внений файл conf/extra/httpd-vhosts.conf:_
```python

<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "D:/WEB/tmp_GIT/htdocs"
    ServerName localhost
    ServerAlias www.dummy-host.example.com
    ErrorLog "logs/error.log"
    CustomLog "logs/access.log" common

    DirectoryIndex index.php index.htm index.html

    <Directory "D:/WEB/tmp_GIT/htdocs">
        Options Indexes ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>

</VirtualHost>

```