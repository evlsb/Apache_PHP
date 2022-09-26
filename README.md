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

Заменить (37 строка)
> ***Define SRVROOT "c:/Apache24"***
на
> ***Define SRVROOT "D:/WEB/tmp_GIT/Apache24"***



> В файле "D:\WEB\tmp_GIT\Apache24\conf\httpd.conf" делаем следующие правки:
* Define SRVROOT на "D:\WEB\tmp_GIT\Apache24"
* DocumentRoot на "D:\WEB\tmp_GIT\www"
* ServerName на "ServerName localhost:80"

> Устанавливаем сервис Apache (от имени Администратора): httpd.exe -k install

> Удалить службу Апач "sc delete Apache2.4"

> Создаем на рабочем столе ярлык "D:\WEB\tmp_GIT\Apache24\bin\ApacheMonitor.exe"

> В браузере заходим на http://localhost/ - должны увидеть It works!

## Установка PHP 8

> Копируем каталог "php_th" в папку "D:\WEB\tmp_GIT"

> В конец файла "D:\WEB\tmp_GIT\Apache24\conf\httpd.conf" добавляем строки:
* LoadModule php_module "D:/WEB/tmp_GIT/php_th/php8apache2_4.dll"
* PHPIniDir "D:/WEB/tmp_GIT/php_th"

> В блок "IfModule mime_module" файла "D:\WEB\tmp_GIT\Apache24\conf\httpd.conf" добавим две строчки
* AddType application/x-httpd-php .php
* AddType application/x-httpd-php-source .phps

> В блок "IfModule dir_module" файла "D:\WEB\tmp_GIT\Apache24\conf\httpd.conf" добавим 
* DirectoryIndex index.html index.php