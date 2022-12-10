# Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"

## 1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.
![Bitwarden](https://user-images.githubusercontent.com/92984527/145699393-6c00647a-82ce-4ffe-aa70-0299569e2757.png)
## 2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.
![BitwardenG](https://user-images.githubusercontent.com/92984527/145699775-838c2b78-4839-4025-8f1b-7f3581f61cf6.png)

![BitwardenG2](https://user-images.githubusercontent.com/92984527/145699766-49db815c-6b22-4177-ab4b-d7e72a9e1ded.png)
## 3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
    `vagrant@vagrant:~$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \-keyout /etc/ssl/private/apache-selfsigned.key \-out /etc/ssl/certs/apache-selfsigned.crt \-subj "/C=RU/ST=Moscow/L=Moscow/O=Company Name/OU=Org/CN=test.com"
    Generating a RSA private key
    ..........................................................................................+++++
    ...+++++
    writing new private key to '/etc/ssl/private/apache-selfsigned.key'
    -----
    vagrant@vagrant:~$ sudo vim /etc/apache2/sites-available/test.com.conf
    vagrant@vagrant:~$ sudo mkdir /var/www/test
    vagrant@vagrant:~$ sudo vim /var/www/test/index.html
    vagrant@vagrant:~$ sudo a2ensite test.com.conf
    Enabling site test.com.
    To activate the new configuration, you need to run:
      systemctl reload apache2
    vagrant@vagrant:~$ sudo apache2ctl configtest
    Syntax OK
    vagrant@vagrant:~$ sudo systemctl reload apache2
    vagrant@vagrant:~$ curl https://test.com`
## 4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).
    `vagrant@vagrant:~$ git clone --depth 1 https://github.com/drwetter/testssl.sh.git
    Cloning into 'testssl.sh'...
    remote: Enumerating objects: 100, done.
    remote: Counting objects: 100% (100/100), done.
    remote: Compressing objects: 100% (93/93), done.
    Receiving objects: 100% (100/100), 8.55 MiB | 254.00 KiB/s, done.
    remote: Total 100 (delta 14), reused 36 (delta 6), pack-reused 0
    Resolving deltas: 100% (14/14), done.
    vagrant@vagrant:~$ cd testssl.sh
    vagrant@vagrant:~/testssl.sh$ ./testssl.sh -U --sneaky https://www.google.com/

    ###########################################################
        testssl.sh       3.1dev from https://testssl.sh/dev/
        (2201a28 2021-12-13 18:24:34 -- )

          This program is free software. Distribution and
                 modification under GPLv2 permitted.
          USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

           Please file bugs @ https://testssl.sh/bugs/

    ###########################################################

     Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
     on vagrant:./bin/openssl.Linux.x86_64
     (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")


     Start 2021-12-14 06:16:36        -->> 216.58.210.132:443 (www.google.com) <<--

     Further IP addresses:   2a00:1450:4026:805::2004
     rDNS (216.58.210.132):  mad06s09-in-f132.1e100.net.
                             hem08s06-in-f4.1e100.net.
                             mad06s09-in-f4.1e100.net.
     Service detected:       HTTP


     Testing vulnerabilities

     Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
     CCS (CVE-2014-0224)                       not vulnerable (OK)
     Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
     ROBOT...`
## 5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
    `vagrant@vagrant:~$ ssh-keygen -C "fullautopilot@gmail.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/vagrant/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/vagrant/.ssh/id_rsa
    Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:... fullautopilot@gmail.com
    The key's randomart image is:
    +---[RSA 3072]----+
    |o.++             |
    |. ++.            |
    | o.B=            |
    |  *BE...         |
    |  .%=.o.S        |
    |  oooo =..       |
    |  . ++.  oo      |
    |   *o + +...     |
    |  ..+o ..o..     |
    +----[SHA256]-----+
    vagrant@vagrant:~$ cat ~/.ssh/id_rsa.pub
    ssh-rsa ...
    ...
    fullautopilot@gmail.com
    vagrant@vagrant:~$ ssh -T git@github.com
    Enter passphrase for key '/home/vagrant/.ssh/id_rsa':
    Hi korotkov-dmitry! You've successfully authenticated, but GitHub does not provide shell access.` 
## 6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.
    `vagrant@vagrant:~$ mv ~/.ssh/id_rsa ~/.ssh/id_rsa_git
    vagrant@vagrant:~$ cat ~/.ssh/config
    Host githab
      HostName github.com
      IdentityFile ~/.ssh/id_rsa_git
      User git
    vagrant@vagrant:~$ ssh githab
    Enter passphrase for key '/home/vagrant/.ssh/id_rsa_git':
    PTY allocation request failed on channel 0
    Hi korotkov-dmitry! You've successfully authenticated, but GitHub does not provide shell access.
    Connection to github.com closed.`
## 7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
    `vagrant@vagrant:~$ sudo tcpdump -c 100 -w file.pcap
    tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
    100 packets captured
    112 packets received by filter
    0 packets dropped by kernel`
![Wireshark](https://user-images.githubusercontent.com/92984527/145980516-ece2fdb1-8303-4c79-9acb-949fba32b72a.png)

