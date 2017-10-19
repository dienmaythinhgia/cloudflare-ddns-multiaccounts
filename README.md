# Cloudflare DDNS-client v1.0.0

DDNS-клиент для сервиса [Cloudflare](https://www.cloudflare.com/).


## Основные возможности
- Работа с несколькими аккаунтами Cloudflare
- Работа с несколькими доменами. Например: domain1.com, domain2.com
- Обновление нескольких DNS записей для домена
- Поддержка IPv4 и IPv6
- Выбор режима обновления: только IPv4, только IPv6, или IPv4 и IPv6 одновременно
- Выбор режима получения текущено IP. С помощью HTTP-запроса или утилиты DIG
- При отсутствии DNS записи, она будет создана
- Выбор режима логирования событий: полный, краткий, отключено
- Автоочистка лог-файла. При превышении заданного размера, файл будет очищен
- Возможность запуска обновления по CRON или GET-запросом (Например с помощью утилиты WGET)
- Возможность добавить несколько URL-сервисов получения текущего IP адреса для HTTP-метода. URL сервиса будет выбран случайным образом
- Возможность указать количество попыток получения текущего IP адреса для HTTP-метода. Если IP получить не удалось, будет выбран другой URL-сервиса


## Как использовать
Основные настройки скрипта находятся в файле `config/config.php`, этот файл необходимо создать. Для этого нужно открыть файл `config/config.php.sample`, настроить в нем параметры и сохранить как `config/config.php`.


В папке `config/entries` должны располагаться файлы с параметрами доменов для обновления их DNS-записей. Таких файлов может быть несколько, можно задавать произвольное имя, главное, чтобы расширение у файлов было `.php`. Например: domain1.com.php

В папке `config/entries` находится файл `entry.php.sample`, как пример для создания такого файла с параметрами. Его необходимо открыть, настроить в нем параметры и сохранить с произвольным именем, например: `config/entries/domain1.com.php`.

Для автоматического запуска обновления IP-адреса можно создать CRON-задачу или вызвать скрипт `update.php` GET-запросом с параметром token. Вместо cron_token указать токен запуска из основного конфиг-файла (находится в самом низу)

*Примеры вызова*

CRON-задача

``*/5 * * * * php /path/to/cloudflare-ddns-multiaccounts/update.php --token="cron_token"``

Вызов GET-запросом

``http://example.com/cloudflare-ddns-multiaccounts/update.php?token=cron_token``


Кратко:
1. Скачать скрипт как ZIP-архив или выполнить: ``git clone https://github.com/prog-it/cloudflare-ddns-multiaccounts.git``
2. Перейти в папку со скриптом: ``cd cloudflare-ddns-multiaccounts``
3. Создать основной конфиг-файл "config/config.php: ``cp config/config.php.sample config/config.php``. Настройки по умолчанию оптимальны, но можно изменить на свои. В файле есть подробные комментарии
4. Создать первый файл с параметрами для обновления DNS записей домена: "config/entries/entry1.php": ``cp config/entries/entry.php.sample config/entries/entry1.php``. Настроить параметры в этом файле, в нем есть подробные комментарии. Таких файлов можно создавать несколько (с разными именами), в каждом свои параметры для обновления DNS записей
5. Создать CRON задачу обновления IP. Вместо cron_token указать токен запуска CRON из конфиг-файла (находится в самом низу)

``*/5 * * * * php /path/to/cloudflare-ddns-multiaccounts/update.php --token="cron_token"``


## Некоторые особенности
- Если для DNS записи (Например: my.computer.example.com) используется несколько IP адресов, то будет обновлен IP только у первой по счету


## Системные требования
- PHP 5.4 и выше
- PHP библиотеки: cURL


Если нашлись какие-либо баги или недоработки, то оставляйте свои заявки в разделе [**Issues**](https://github.com/prog-it/cloudflare-ddns-multiaccounts/issues)

Историю изменений можно посмотреть [здесь](https://github.com/prog-it/cloudflare-ddns-multiaccounts/releases). Текущая версия DDNS-клиента находится в файле: ``README.md``


## Лицензия

DDNS-клиент распространяется под лицензией *MIT*.

