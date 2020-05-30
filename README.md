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

GET request call

``http://example.com/cloudflare-ddns-multiaccounts/update.php?token=startup_token``

*Call examples to update a single record "example.com" . To do this, a file config/entriesmust exist in the folder`example.com.php`

CRON task

``*/5 * * * * php /path/to/cloudflare-ddns-multiaccounts/update.php --token="startup_token" --entry="example.com"``

GET request call

``http://example.com/cloudflare-ddns-multiaccounts/update.php?token=startup_token&entry=example.com``

Also in the folder ``serveris`` a script to get the current IP address using an HTTP request. This script can be placed on your server and add the URL of this script to the main config file. For example: ``http://example.com/ip.php.`` If you call the script with a parameter ``?raw``, it will return only the IP-address, for example:``http://example.com/ip.php?raw``.


#Briefly:
1. Download the script as a ZIP archive or run: ``git clone https://github.com/prog-it/cloudflare-ddns-multiaccounts.git``
2.Go to the script folder: ``cd cloudflare-ddns-multiaccounts``
3. Create the main config file "config/config.php: ``cp config/config.php.sample config/config.php``. The default settings are optimal, but can be changed to your own. There are detailed comments in the file
4. Create the first file with the settings for DNS update domain records: "config/entries/entry1.php": ``cp config/entries/entry.php.sample config/entries/entry1.php``. Set the parameters in this file, it has detailed comments. You can create several such files (with different names), each with its own parameters for updating DNS records
5. Create a CRON IP update task. Instead of startup_token, specify the CRON launch token from the config file (located at the very bottom)

``*/5 * * * * php /path/to/cloudflare-ddns-multiaccounts/update.php --token="startup_token"``


##Some features
If several IP addresses are used for DNS records (for example: my.computer.example.com), then only the first one will be updated with IP


##System requirements
-PHP 5.4 and higher
-PHP libraries: cURL, php-mod-tokenizer (PHP7 - php7-mod-tokenizer, PHP5 - php5-mod-tokenizer)
-DNS lookup (dig) utility, if the method of obtaining IP is chosen - "dig"


If you find any bugs or flaws, then leave your applications in the [Issues](https://github.com/prog-it/cloudflare-ddns-multiaccounts/issues)section

The history of changes can be found [here](https://github.com/prog-it/cloudflare-ddns-multiaccounts/releases). The current version of the DDNS client is in the file: ``README.md``

