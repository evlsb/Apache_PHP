### Комманды Apache 2.4 ###
* ***httpd.exe -k install***  : _Установить_
* ***httpd.exe -k start***    : _Запустить_
* ***httpd.exe -k restart***  : _Перезапустить_
* ***httpd -k shutdown***     : _Остановить_
* ***httpd -k stop***         : _Остановитть_
* ***httpd -k uninstall***    : _Удалить_

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
> ***D:\WEB\tmp_GIT\php_th***

> ***D:\WEB\tmp_GIT\Apache24\bin***

## Установка Apache 2.4

### Изменения конфигурационного файла

_Заменяем:_

|строка|с|на|пояснение|
|-----|-----|-----|-----|
|37|***Define SRVROOT "c:/Apache24"***|***Define SRVROOT "D:/WEB/tmp_GIT/Apache24"***|_Путь папки Apache_|
|217|***ServerAdmin admin@example.com"***|***ServerAdmin localhost:80"***|_адрес сайта_|
|250|***DocumentRoot "${SRVROOT}/htdocs"***|***DocumentRoot "D:/WEB/tmp_GIT/www"***|_каталог сайта_|
|251|***DocumentRoot "${SRVROOT}/htdocs"***|***<Directory "D:/WEB/tmp_GIT/www">***|_каталог сайта_|

_Добавляем в конец файла строки:_

```python

LoadModule php_module "D:/WEB/tmp_GIT/php_th/php8apache2_4.dll"  
PHPIniDir "D:/WEB/tmp_GIT/php_th"

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




> Устанавливаем сервис Apache (от имени Администратора): httpd.exe -k install

> Удалить службу Апач "sc delete Apache2.4"

> Создаем на рабочем столе ярлык "D:\WEB\tmp_GIT\Apache24\bin\ApacheMonitor.exe"

> В браузере заходим на http://localhost/ - должны увидеть It works!

## Установка PHP 8



