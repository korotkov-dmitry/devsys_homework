# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис
```
    { "info" : "Sample JSON output from our service\t",
        "elements" : [
            { "name" : "first",
            "type" : "server",
            "ip" : "7175"
            },
            { "name" : "second",
            "type" : "proxy",
            "ip" : "71.78.22.43"
            }
        ]
    }
```
## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
#!/usr/bin/env python3
import datetime
import socket
import time
import json
import yaml
import os

cdlog = os.getcwd() + "/log/"
hosts = {'drive.google.com':'0.0.0.0','mail.google.com':'0.0.0.0','google.com':'0.0.0.0'}
while True:
    for host in hosts:
        error = False
        ip = socket.gethostbyname(host)
        oldip = hosts[host]
        d = datetime.datetime.now()
        if oldip == '0.0.0.0':
            hosts[host] = ip
        elif ip != oldip:
            error = True
        if error:
            print(d.strftime('%Y-%m-%d %H:%M:%S') + ' [ERROR] ' + host + ' IP mismatch: ' + oldip + ' ' + ip)
        else:
            print(d.strftime('%Y-%m-%d %H:%M:%S') + ' ' + host + ' - ' + ip)
        with open(cdlog + host + ".json", 'w') as jsf:
            json_data = json.dumps({host: ip})
            jsf.write(json_data)
        with open(cdlog + host + ".yaml", 'w') as ymf:
            yaml_data = yaml.dump([{host: ip}])
            ymf.write(yaml_data)
    time.sleep(10)
```

### Вывод скрипта при запуске при тестировании:
```
2021-12-26 12:43:24 drive.google.com - 209.85.233.194
2021-12-26 12:43:24 mail.google.com - 216.58.210.133
2021-12-26 12:43:24 google.com - 216.58.209.206
2021-12-26 12:43:34 drive.google.com - 209.85.233.194
2021-12-26 12:43:34 mail.google.com - 216.58.210.133
2021-12-26 12:43:34 google.com - 216.58.209.206
...
2021-12-26 12:46:14 drive.google.com - 209.85.233.194
2021-12-26 12:46:14 mail.google.com - 216.58.210.133
2021-12-26 12:46:14 google.com - 216.58.209.206
2021-12-26 12:46:24 [ERROR] drive.google.com IP mismatch: 209.85.233.194 173.194.222.194
2021-12-26 12:46:24 mail.google.com - 216.58.210.133
2021-12-26 12:46:24 google.com - 216.58.209.206
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{"drive.google.com": "173.194.222.194"}
{"mail.google.com": "216.58.210.133"}
{"google.com": "216.58.209.206"}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
- drive.google.com: 209.85.233.194
- mail.google.com: 216.58.210.133
- google.com: 216.58.209.206
```
