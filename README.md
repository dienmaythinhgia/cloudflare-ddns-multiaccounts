# Cloudflare DDNS-client v1.1.9

DDNS client for  [Cloudflare](https://www.cloudflare.com/).


## Key features
Work with multiple Cloudflare accounts
Work with multiple domains. For example: domain1.com, domain2.com
Updating multiple DNS records for a domain
Configure settings for each DNS record:
Time to Live (TTL)
Request Proxy Mode
IPv4 and IPv6 Support
Select update mode:
IPv4 only
IPv6 only
IPv4 and IPv6 at the same time
Selecting the mode for obtaining the current IP:
HTTP request
DNS lookup (dig)
If there is no DNS record, it will be created.
Choosing an event logging mode:
Full
Brief
Disconnected
Auto clean log file. If the specified size is exceeded, the file will be cleared
Ability to start updates by CRON or GET request (for example, using the WGET utility)
The ability to add multiple URL services to obtain the current IP address for the HTTP method. Service URL will be randomly selected
The ability to specify the number of attempts to obtain the current IP address for the HTTP method. If the IP could not be obtained, a different URL service will be selected


## КHow to use
The main script settings are in the file config/config.php, this file must be created. To do this, open the file ``config/config.php.sample``, configure the settings in it and save as ``config/config.php.``

The folder ``config/entries``should contain files with domain parameters for updating their DNS records. There can be several such files, you can specify an arbitrary name, the main thing is that the extension has files ``.php``. For example: ``domain1.com.php``

The folder ``config/entries`` contains a file entry.php.sample, as an example for creating such a file with parameters. It must be open, set up parameters and save with a random name, for example: ``config/entries/domain1.com.php.``

When calling the script, ``update.php`` you can add the entry parameter. This option allows you to update only one entry from a folder ``config/entries``, and not all at once. If you call the script without this parameter, all records that are in the folder will be updated ``config/entries``

To automatically start updating the IP address, you can create a CRON task or call the script with a ``update.php`` GET request with the token (and entry) parameter. Instead of startup_token, specify the launch token from the main config file (located at the very bottom)

Call examples to update all records

CRON task

``*/5 * * * * php /path/to/cloudflare-ddns-multiaccounts/update.php --token="startup_token"``

Вызов GET-запросом

``http://example.com/cloudflare-ddns-multiaccounts/update.php?token=startup_token``

*Примеры вызова для обновления одной записи "example.com"*. Для этого в папке `config/entries` должен существовать файл `example.com.php`

CRON-задача

``*/5 * * * * php /path/to/cloudflare-ddns-multiaccounts/update.php --token="startup_token" --entry="example.com"``

GET request call

``http://example.com/cloudflare-ddns-multiaccounts/update.php?token=startup_token&entry=example.com``

Также в папке `server` находится скрипт для получения текущего IP-адреса с помощью HTTP-запроса. Этот скрипт можно разместить на своем сервере и добавить URL этого скрипта в основной конфиг-файл. 
Например: ``http://example.com/ip.php``. Если вызвать скрипт с параметром ``?raw``, то он вернет только IP-адрес, например: ``http://example.com/ip.php?raw``.


Кратко:
1. Скачать скрипт как ZIP-архив или выполнить: ``git clone https://github.com/prog-it/cloudflare-ddns-multiaccounts.git``
2. Перейти в папку со скриптом: ``cd cloudflare-ddns-multiaccounts``
3. Создать основной конфиг-файл "config/config.php: ``cp config/config.php.sample config/config.php``. Настройки по умолчанию оптимальны, но можно изменить на свои. В файле есть подробные комментарии
4. Создать первый файл с параметрами для обновления DNS записей домена: "config/entries/entry1.php": ``cp config/entries/entry.php.sample config/entries/entry1.php``. Настроить параметры в этом файле, в нем есть подробные комментарии. Таких файлов можно создавать несколько (с разными именами), в каждом свои параметры для обновления DNS записей
5. Создать CRON задачу обновления IP. Вместо startup_token указать токен запуска CRON из конфиг-файла (находится в самом низу)

``*/5 * * * * php /path/to/cloudflare-ddns-multiaccounts/update.php --token="startup_token"``


## Некоторые особенности
- Если для DNS записи (Например: my.computer.example.com) используется несколько IP адресов, то будет обновлен IP только у первой по счету


## Системные требования
- PHP 5.4 и выше
- PHP библиотеки: cURL, php-mod-tokenizer (PHP7 - php7-mod-tokenizer, PHP5 - php5-mod-tokenizer)
- Утилита DNS lookup (dig), если выбран способ получения IP - "dig"


Если нашлись какие-либо баги или недоработки, то оставляйте свои заявки в разделе [**Issues**](https://github.com/prog-it/cloudflare-ddns-multiaccounts/issues)

Историю изменений можно посмотреть [здесь](https://github.com/prog-it/cloudflare-ddns-multiaccounts/releases). Текущая версия DDNS-клиента находится в файле: ``README.md``

