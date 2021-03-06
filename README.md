---
title: Tiun. Bash script for routine Drupal tasks
---

# Tiun. Bash script for routine Drupal tasks

Tiun — скрипт командной строки Linux и MacOS, который облегчает установку, настройку и обслуживание Drupal.

## Что делает Tiun

Для установки и настройки Drupal скрипт Tiun делает:

 1. Создает сайт в локальном Apache.
 2. Добавляет его в /etc/hosts.
 3. Создает базу данных MySQL.
 4. Скачивает последний актуальный Drupal, модули, темы и библиотеки и сохраняет в собственном кэше.
 5. Накладывает патчи для исправления известных ошибок модулей.
 6. Устанавливает Drupal в самом минимальном виде.
 7. Создает пользователя "Редактор сайта", которому дает права только на работу с контентом сайта.
 8. Создает и записывает в отдельный файл мнемонические (удобные для запоминания), но в то же время безопасные пароли для администратора и редактора.
 9. Настраивает правильные права на папки и файлы.
10. Делает перевод интерфейса Drupal на нужный язык (русский или украинский).
11. Делает резервную копию минимальной стандартной рабочей конфигурации Drupal.
12. Устанавливает набор самых распространенных модулей, нужных практически в любой конфигурации Drupal: views, ctools, entity, token.
13. Настраивает набор улучшений админки Drupal (одна из трех минималистичных тем — Adminimal, Rubik или Shiny, упрощенный интерфейс для редактора сайта, возможность администратору легко переключаться в роль редактора сайта и обратно, возможность входить в Drupal как по логину, так и по имейлу, ряд других улучшений).
14. Создает и настраивает основной тип материалов.
15. Настраивает визуальный редактор CKeditor с минимумом самых необходимых стандартных плагинов + скачивает, подключает и настраивает 10 дополнительных плагинов, включая собственный плагин для загрузки изображений.
16. Настраивает HTML-фильтр визуального редактора, благодаря чему вставляемый текст очищается от мусорных тэгов, но сохраняются основные стили выделения текста (курсив, полужирный, выравнивание и т.п.)
17. Устанавливает и подключает библиотеку Colorbox для просмотра изображений во всплывающем окне.
18. Настраивает обработку загружаемых файлов (массовая загрузка и безопасное переименование файлов).
19. Настраивает оптимизацию для поисковиков, включая SEO-friendly адреса страниц, мета-тэги, карту сайта, правильные редиректы, оптимизацию изображений (автоматическое создание alt и title), устанавливает атрибут `nofollow` для внешних ссылок, обеспечивает простое подключение сайта к Google Webmaster Tools, Yandex Webmaster Tools, Google Apps, Bing Webmaster Central, Yahoo! Site Explorer.
20. Настраивает поиск по сайту с учетом русской и украинской морфологии (склонение слов) и возможностью подключить Apache Solr. 
21. Если указано в настройках, то устанавливает и настраивает модули для повышения быстродействия, инструменты для программиста и вебмастера, защиту сайта от вирусов и взлома.
22. Настраивает шаблон на базе субтемы Bottstrap + Grunt + SAAS.
23. Подчищает после установки Drupal мусор.
24. Размещает рядом с папкой сайта инструкции по работе с Drupal с прямыми ссылками на страницы админки этого сайта.
25. Делает вторую резервную копию — уже настроенного Drupal.

Если что-то не получилось во время установки Drupal и вы запускаете Tiun повторно, скрипт сам проверит наличие базы данных. Если база уже создана и доступна, скрипт предложит или воспользоваться ею, или создать новую.

Новые логины и пароли Tiun добавляет в файл info.txt, а старые помечает как устаревшие.

В настройках Tiun можно указать, какие наборы модулей устанавливать сразу, при инсталляции Drupal. Или можно установить только минимальную конфигурацию Drupal, а отдельные наборы модулей установить при последующих запусках Tiun.

Например, можно установить Drupal в конфигурации обычного сайта со всеми удобствами и оптимизациями, а позже установить набор модулей, который добавит на сайт интернет-магазин Commerce.

Модулей должно быть как можно меньше. Поэтому во время установки модулей Tiun ведет подсчет активированных всегда модулей и записывает статистику в info.txt.

После того, как Drupal установлен, Tiun позволяет делать:

* резервные копии Drupal;
* обновление локализации;
* обновление кэша дистрибутива, тем, модулей и библиотек для быстрой установки следующих локальных сайтов на Drupal;
* установку наборов модулей, которые не были добавлены при установке Drupal;
* переключение режимов работы сайта: 
    * стандартный режим: возможность редактировать сайт + кэширование;
    * режим разработчика: без кэширования, с включенными инструментами программиста и вебмастера;
    * режим максимальной производительности;
* удаление сайта — по частям или полностью, включая файлы сайта, резервные копии, базу данных и настройки Apache.
 
Работа Tiun протестирована в Ubuntu 14.10 и 15.04.

## Необходимое окружение

Для работы Tiun нужны:

- Apache, MySQL и PHP
- apg — генерирование паролей
- drush — Drupal Shell, управление Drupal'ом из командной строки
- php-pear — для обновления drush
- curl — для проверки новых версий библиотек на официальных сайтах
- git — для скачивания библиотек

### Установка окружения в Ubuntu и Debian

Установите нужные программы:

    sudo apt-get install apache2 php5 php-pear mysql-server drush php-pear git

Включить модуль Apache *mod_rewrite*:

    sudo a2enmod rewrite

Добавьте себя в группу *www-data*:

    sudo usermod -aG www-data $(whoami)

Протестируйте:

    echo '<?php phpinfo(); ?>' >> /var/www/info.php
    sudo apachectl restart
    xdg-open http://localhost/info.php

Обновите *drush*:

    sudo pear channel-discover pear.drush.org && sudo pear upgrade drush/drush

## Использование Tiun

### Вариант 1. Скачивание Tiun из репозитория

Вместо `USERNAME` введите свое имя пользователя Bitbucket:

    git init
    git clone https://USERNAME@bitbucket.org/zinadz/tiun.git

### Вариант 2. Установка Tiun из архива

1. Перепишите содержимое архива в любую удобную папку. Например, в `~/tiun`
2. Назначьте файлу `tiun` права на исполнение:

    chmod +x ~/tiun/tiun

## Удобная работа

Чтобы скрипт можно было запустить из любой папки, просто набрав в терминале *tiun*, добавьте в файл `~/.bashrc` строку с путем к папке скрипта:

    PATH=$PATH:~/bin/tiun

Чтобы сразу указать Тиуну, с каким сайтом работать, запустите его с именем домена в качестве аргумента:

    tiun site.dev

### Сайты в домашней папке

Настройте Apache на работу с локальной папкой `/home/USERNAME/Sites`, где *USERNAME* — ваше имя пользователя.

Сначала создайте эту папку:

    mkdir ~/Sites

Добавьте в основной конфиг Apache `/etc/apache2/apache2.conf` директиву `IncludeOptional`, указывающую, где лежат конфиги ваших сайтов:

    sudo sh -c 'echo "IncludeOptional /home/*/Sites/*.conf" >> /etc/apache2/apache2.conf'

Теперь можно создавать сайты как стандартным образом, в папке `/var/www`, так и более простым — в своей домашней папке `~/Sites`.

При это учитывайте, что конфиги сайтов тоже могут храниться как в папке стандартной папке `/etc/apache2/sites-available/`, так и в папке `~/Sites`. Соответственно, инструменты для включения `a2ensite` и выключения `a2dissite` сайтов будут работать только со стандартным путем и не будут работать с сайтами в домашней папке.

### uploadprogress

`uploadprogress` — это библиотека PHP для отслеживания прогресса загрузки файла на сервер. Без этой библиотеки в статусе Drupal `admin/reports/status` может выводиться сообщение об ошибке.

Установите и активируйте `uploadprogress`:

    sudo apt-get install php5-dev
    sudo pecl install uploadprogress
    sudo sh -c 'echo "extension=uploadprogress.so" >> /etc/php5/apache2/php.ini'
    sudo service apache2 restart

### Локальная DNS-зона

Для более удобной работы с локальными сайтами рекомендуем настроить локальную DNS-зону.

Это нужно для того, чтобы при обращении к сайтам на локальной доменной зоне (например, `.loc`) браузер не искал эту несуществующую зону в интернете и не выдавал сообщение об ошибке, а сразу отправлялся на локальный Apache.

Установите облегченный DNS-сервер *dnsmasq*:

    sudo apt-get install dnsmasq

Добавьте локальную зону *.loc* в файл `/etc/dnsmasq.conf`:

    address=/loc/127.0.0.1
    listen-address=127.0.0.1

Перезапустите DNS-сервер:

    sudo /etc/init.d/dnsmasq restart

Протестируйте:

    ping qwerty.loc

### Node.js и npm

`Node.js` — платформа для разработки серверных приложений на языке JavaScript. Напрямую при работе с Drupal она не используется, но она требуется для установки *Sass*, *Gulp* и других крутых штук, применяемых в современной верстке сайтов.

Вместе с Node.js потребуется `npm` — менеджер пакетов, который поддерживает пакеты для *Node.js*, *Gulp*, *Grunt*, *jQuery* и других популярных платформ.

Установим официальный репозиторий и оба приложения — Node.js и npm.

    curl -sL https://deb.nodesource.com/setup | sudo bash -
    sudo apt-get install nodejs

### Sass

`Sass` — это препроцессорный язык с синтаксисом SCSS. Проще говоря, это CSS с расширенными и более удобными возможностями. Но т.к. код SCSS все-таки не чистый CSS и его нельзя напрямую давать браузеру, то он сначала обрабатывается препроцессором, после чего браузеру отдается стандартный, хорошо оптимизированный CSS.

Установите Sass:

    sudo apt-get install ruby-sass

### Gulp

`Gulp` — сборщик кода Sass/SCSS. Он запускается в фоне и, пока вы работаете с шаблонами сайта, следит за изменениями в ваших файлах SCSS и на лету пересобирает их в понятный браузеру и оптимизированный по размеру CSS.

У вас уже должны быть установлены *Node.js*, *NPM* и *Sass*, как описано выше.

Установите Gulp:

    sudo npm install -g gulp

Параметр `-g` означает, что Gulp будет установлен глобально, т.е. в общесистемную папку.

Дальнейшая настройка Gulp делается локально, в папке вашего проекта.
