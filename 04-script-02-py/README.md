# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | Будет ошибка, т.к. разный тип значений  |
| Как получить для переменной `c` значение 12?  | `c = str(a) + b`  |
| Как получить для переменной `c` значение 3?  | `c = a + int(b)`  |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3
import os
bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```python
#!/usr/bin/env python3
import os

bash_command = ["cd C:/Users/fulla/netology_devops/testgit", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
    elif result.find('new file') != -1:
        prepare_result = result.replace('\tnew file:   ', '')
        print(prepare_result)
        #break
```

### Вывод скрипта при запуске при тестировании:
```
err.log
README.md
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
#!/usr/bin/env python3
import os
import sys

cmd = os.getcwd()
if len(sys.argv)>=2:
    cmd = sys.argv[1]
bash_command = ["cd " + cmd, "ls -a | grep .git"]
result_git = os.popen(' && '.join(bash_command)).read()
if len(result_git)==0:
    print('В данном каталоге не найден гит репозиторий')
    exit()
bash_command = ["cd "+cmd, "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
    elif result.find('new file') != -1:
        prepare_result = result.replace('\tnew file:   ', '')
        print(prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
В данном каталоге не найден гит репозиторий
или
err.log
README.md
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
#!/usr/bin/env python3
import datetime
import socket
import time

hosts = {'drive.google.com':'0.0.0.0','mail.google.com':'0.0.0.0','google.com':'0.0.0.0'}
while 1==1:
    for host in hosts:
        ip = socket.gethostbyname(host)
        oldip = hosts[host]
        d = datetime.datetime.now()
        if oldip=='0.0.0.0':
            hosts[host] = ip
            print(d.strftime('%Y-%m-%d %H:%M:%S') + ' ' + host + ' - ' + ip)
        elif ip != oldip:
            print(d.strftime('%Y-%m-%d %H:%M:%S')+' [ERROR] '+host+' IP mismatch: '+oldip+' '+ip)
        else:
            print(d.strftime('%Y-%m-%d %H:%M:%S') + ' ' + host + ' - ' + ip)
    time.sleep(10)
```

### Вывод скрипта при запуске при тестировании:
```
...
2021-12-22 12:33:09 drive.google.com - 209.85.233.194
2021-12-22 12:33:09 mail.google.com - 142.251.1.83
2021-12-22 12:33:09 google.com - 173.194.222.102
...
2021-12-22 12:35:50 drive.google.com - 209.85.233.194
2021-12-22 12:35:50 mail.google.com - 142.251.1.83
2021-12-22 12:35:50 google.com - 173.194.222.102
2021-12-22 12:36:00 drive.google.com - 209.85.233.194
2021-12-22 12:36:00 mail.google.com - 142.251.1.83
2021-12-22 12:36:00 [ERROR] google.com IP mismatch: 173.194.222.102 173.194.222.139
2021-12-22 12:36:10 drive.google.com - 209.85.233.194
2021-12-22 12:36:10 mail.google.com - 142.251.1.83
2021-12-22 12:36:10 [ERROR] google.com IP mismatch: 173.194.222.102 173.194.222.139
...
```
