# Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"

## 1. Работа c HTTP через телнет.
- Подключитесь утилитой телнет к сайту stackoverflow.com
`telnet stackoverflow.com 80`
- отправьте HTTP запрос
```bash
GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]
```
- В ответе укажите полученный HTTP код, что он означает?

Результат:

    `vagrant@vagrant:~$ telnet stackoverflow.com 80
    Trying 151.101.193.69...
    Connected to stackoverflow.com.
    Escape character is '^]'.
    GET /questions HTTP/1.0
    HOST: stackoverflow.com

    HTTP/1.1 301 Moved Permanently
    cache-control: no-cache, no-store, must-revalidate
    location: https://stackoverflow.com/questions
    x-request-guid: 2ee65083-4bc7-44ad-8bfe-5fc4d601ecd1
    feature-policy: microphone 'none'; speaker 'none'
    content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
    Accept-Ranges: bytes
    Date: Sun, 05 Dec 2021 10:27:43 GMT
    Via: 1.1 varnish
    Connection: close
    X-Served-By: cache-fra19160-FRA
    X-Cache: MISS
    X-Cache-Hits: 0
    X-Timer: S1638700063.494519,VS0,VE93
    Vary: Fastly-SSL
    X-DNS-Prefetch-Control: off
    Set-Cookie: prov=07644e29-d26a-61f5-3b17-951f195a586d; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly
    
    Connection closed by foreign host.`
    
Ответ сервера: `HTTP/1.1 301 Moved Permanently` - код перенаправления протокола передачи гипертекста (HTTP) показывает, что запрошенный ресурс был перемещён в URL, указанный в заголовке Location.
## 2. Повторите задание 1 в браузере, используя консоль разработчика F12.
- откройте вкладку `Network`
- отправьте запрос http://stackoverflow.com
- найдите первый ответ HTTP сервера, откройте вкладку `Headers`
- укажите в ответе полученный HTTP код.
- проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
- приложите скриншот консоли браузера в ответ.

Результат:

Status Code: `307 Internal Redirect`
    
Самый долгий запрос: 
    
    `Request URL: https://stackoverflow.com/
    Request Method: GET
    Status Code: 200 
    Remote Address: 151.101.129.69:443
    Referrer Policy: strict-origin-when-cross-origin`
    
![stackoverflow](https://user-images.githubusercontent.com/92984527/144743131-3591cb0a-d829-4325-89c3-d00198db869c.png)

## 3. Какой IP адрес у вас в интернете?
https://whoer.net/ru
`176.50.168.66`
## 4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой `whois`
    `vagrant@vagrant:~$ whois -h whois.ripe.net 176.50.168.66
    ...
    % Information related to '176.50.128.0/18AS12389'

    route:          176.50.128.0/18
    descr:          JSC Rostelecom regional branch "Siberia"
    remarks:        ALTAY branch
    origin:         AS12389
    mnt-by:         NSOELSV-NCC
    mnt-by:         ROSTELECOM-MNT
    created:        2019-04-30T02:07:32Z
    last-modified:  2019-04-30T02:07:32Z
    source:         RIPE # Filtered`
## 5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой `traceroute`
Не показывает каке AS.

    `vagrant@vagrant:~$ traceroute -An 8.8.8.8
    traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
     1  10.0.2.2 [*]  0.473 ms  0.272 ms  0.165 ms
     2  * * *
     3  * * *
     4  * * *
     5  * * *
     6  * * *
     7  * * *
     8  * * *
     9  * * *
    10  * * *
    11  * * *
    12  * * *
    13  * * *
    14  * * *
    15  * * *
    16  * * *
    17  * * *
    18  * * *
    19  * * *
    20  * * *
    21  * * *
    22  * * *
    23  * * *
    24  * * *
    25  * * *
    26  * * *
    27  * * *
    28  * * *
    29  * * *
    30  * * *`
## 6. Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка - delay?
    `                           My traceroute  [v0.93]
    vagrant (10.0.2.15)                                2021-12-05T11:43:08+0000
    Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                       Packets               Pings
     Host                            Loss%   Snt   Last   Avg  Best  Wrst StDev
     1. AS???    10.0.2.2             0.0%    19    1.5   2.6   0.8  11.4   2.8
     2. AS???    192.168.1.1          0.0%    19    5.1   5.6   3.5  19.8   3.6
     3. (waiting for reply)
     4. AS12389  213.228.107.12       5.3%    19   33.6  36.9  32.9  50.5   5.2
     5. AS12389  87.226.181.89        0.0%    19   77.1  78.4  76.6  82.0   1.7
     6. AS15169  74.125.52.232       88.9%    19  215.5 146.5  77.5 215.5  97.6
        AS15169  209.85.254.20
     7. AS15169  108.170.250.34       0.0%    19   38.4  33.1  25.4  77.3  12.9
        AS15169  74.125.52.232
     8. AS15169  142.251.49.24        0.0%    19   25.5  29.5  17.8 106.5  19.4
        AS15169  108.170.250.34
     9. AS15169  209.85.254.20        0.0%    18   44.7  45.2  40.5  93.9  12.6
        AS15169  142.251.49.24
    10. AS15169  142.250.56.219       0.0%    18   28.1  45.6  28.1  92.7  12.8
        AS15169  209.85.254.20
        AS15169  142.251.49.24
    11. AS12389  213.228.116.69       5.6%    18   39.6 114.7  39.1 1258. 294.8
        AS15169  142.250.56.219
    12. AS12389  213.228.116.69       5.6%    18  653.1 584.7  23.9 664.8 146.3
        AS15169  142.250.56.219
    13. AS12389  213.228.116.69      94.1%    18  568.9 568.9 568.9 568.9   0.0
    14. (waiting for reply)
    15. (waiting for reply)
    16. (waiting for reply)
    17. (waiting for reply)`
    
По среднему `Avg`:

**`12. AS12389  213.228.116.69       5.6%    18  653.1 584.7  23.9 664.8 146.3`**
 
По максимальному `Wrst`:

**`11. AS12389  213.228.116.69       5.6%    18   39.6 114.7  39.1 1258. 294.8`**
## 7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой `dig`
    `vagrant@vagrant:~$ dig +trace @8.8.8.8 dns.google
    ...
    dns.google.             900     IN      A       8.8.8.8
    dns.google.             900     IN      A       8.8.4.4
    ...`
## 8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой `dig`
`dns.google.`
    
    `vagrant@vagrant:~$ dig -x 8.8.8.8

    ; <<>> DiG 9.16.1-Ubuntu <<>> -x 8.8.8.8
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 52647
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 65494
    ;; QUESTION SECTION:
    ;8.8.8.8.in-addr.arpa.          IN      PTR

    ;; ANSWER SECTION:
    8.8.8.8.in-addr.arpa.   3368    IN      PTR     dns.google.

    ;; Query time: 0 msec
    ;; SERVER: 127.0.0.53#53(127.0.0.53)
    ;; WHEN: Sun Dec 05 12:03:27 UTC 2021
    ;; MSG SIZE  rcvd: 73`

    `vagrant@vagrant:~$ dig -x 8.8.4.4

    ; <<>> DiG 9.16.1-Ubuntu <<>> -x 8.8.4.4
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 65140
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 65494
    ;; QUESTION SECTION:
    ;4.4.8.8.in-addr.arpa.          IN      PTR

    ;; ANSWER SECTION:
    4.4.8.8.in-addr.arpa.   54365   IN      PTR     dns.google.

    ;; Query time: 36 msec
    ;; SERVER: 127.0.0.53#53(127.0.0.53)
    ;; WHEN: Sun Dec 05 12:03:37 UTC 2021
    ;; MSG SIZE  rcvd: 73`
