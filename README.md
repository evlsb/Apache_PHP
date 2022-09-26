### Комманды Apache 2.4 ###
* ***httpd.exe -k install***  : _Установить_
* ***httpd.exe -k start***    : _Запустить_
* ***httpd.exe -k restart***  : _Перезапустить_
* ***httpd -k shutdown***     : _Остановить_
* ***httpd -k stop***         : _Остановитть_
* ***httpd -k uninstall***    : _Удалить_

# Подключение PHP8 в качестве модуля Apache

## Подготовительные работы
- tmp_GIT
    - Apache24
        - \*\*\*
        - bin
        - \*\*\*
    - log
    - php_th
    - tmp
    - www
        - index.html
        - index.php

1. Создаем папку где будем хранить все файлы. К примеру ***D:\WEB\tmp_GIT***

1. Распаковываем из архива ***httpd-2.4.38-win64-VC11.zip*** папку ***Apache24*** в ***D:\WEB\tmp_GIT***

2. Также в ***D:\WEB\tmp_GIT*** создаем папки ***log***, ***php_th***, ***tmp***, ***www***

3. В каталоге ***www*** создаем  файла ***index.html*** с содержимым "It works!" и "index.php" с содержимым "<?php phpinfo(); ?>"

4. Копируем каталог ***php_th*** в папку ***D:\WEB\tmp_GIT***

5. В системную переменную PATH добавляем пути: ***D:\WEB\tmp_GIT\Apache24\bin***, ***D:\WEB\tmp_GIT\php_th***

## Установка Apache 2.4




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