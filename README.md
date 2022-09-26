# Подключение PHP8 в качестве модуля Apache

## Подготовительные работы

> Распаковываем из архива "httpd-2.4.38-win64-VC11.zip" папку Apache24 в "D:\WEB\tmp_GIT"

>  Также в "D:\WEB\tmp_GIT" создаем папки "log", "php_th", "tmp", "www"

> В каталоге "www" создаем  файла "index.html" с содержимым "It works!" и "index.php" с содержимым "<?php phpinfo(); ?>"

> Копируем каталог "php_th" в папку "D:\WEB\tmp_GIT"

> В системную переменную PATH добавляем пути: "D:\WEB\tmp_GIT\Apache24\bin", "D:\WEB\tmp_GIT\php_th"

## Установка Apache 2.4


### Комманды Apache 2.4 ###
* _httpd.exe -k install_  : Установить Apache 2.4
* _httpd.exe -k start_    : Запустить Apache 2.4
* _httpd.exe -k restart_  : Перезапустить Apache 2.4
* _httpd -k shutdown_     : Остановка Apache 2.4
* _httpd -k stop_         : Остановка Apache 2.4
* _httpd -k uninstall_    : Удаление Apache 2.4
---

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