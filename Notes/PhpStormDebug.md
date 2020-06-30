# PhpStorm и OpenServer

Рассказываю как для себя :) На примере самой свежей версии PhpStorm.

Идём на страницу https://ospanel.io/download/ и скачиваем OpenSever (я взял 5.3.7 Full). Устанавливаем, например, в папку `D:\OpenServer`. Выбираем нужные модули:

![open-server-01](img/open-server01.png)

Разрешаем отладку скриптов, для этого нужно отредактировать `php.ini`, но делать это надо средствами самого OpenServer:

![open-server-02](img/open-server02.png)

потому что, если отредактировать самому при помощи Far, то при следующем запуске `php.ini` будет заменён дефолтным.

Меняем две настройки: `zend_extension = xdebug`

![open-server-03](img/open-server03.png)

и `xdebug.remote_enable = on`

![open-server-04](img/open-server04.png)

Заводим в PhpStorm проект прямо внутри папки `D:\OpenServer\domains`

![open-server-05](img/open-server05.png)

Обязательно перезапускаем OpenServer, чтобы он подхватил наши правки и новый домен. Для начала убеждаемся, что OpenServer видит наш домен

![open-server-06](img/open-server06.png)

и PhpStorm подхватывает отладку в CLI-интерпретаторе

![open-server-07](img/open-server07.png)

Теперь организуем отладку на живом веб-сервере. Заводим веб-сервер в PhpStorm

![open-server-08](img/open-server08.png)

чтобы сослаться на него в отладочной конфигурации «Php Web Page»

![open-server-09](img/open-server09.png)

Если всё настроено правильно, сервер легко пройдёт валидацию, т. е. будет пригодным для отладки страниц «вживую».

![open-server-10](img/open-server10.png)

Поместим в папку проекта `.htaccess` следующего содержания (я взял его из соседней папки localhost)

```apacheconfig
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>
AddDefaultCharset UTF-8
AddCharset UTF-8 .html
<FilesMatch "\.(html)$">
   Header set Cache-Control: "no-cache, no-store"
   Header unset ETag
</FilesMatch>
Header set X-Content-Type-Options nosniff
```

пишем очень сложный скрипт index.php, на котором будем тренироваться :)

```php
<?php

$x = 123;
$y = 456;
$z = $x + $y;

echo "<p><strong>$x + $y = $z</strong>, как это ни странно!</p>";
```

Запускаем отладку, вуаля, точки останова и просмотр переменных отлично работают!

![open-server-11](img/open-server11.png)

Наш скрипт в браузере

![open-server-12](img/open-server12.png)

Всё вышеописанное работает в PhpStorm 2020.1

![open-server-13](img/open-server13.png)