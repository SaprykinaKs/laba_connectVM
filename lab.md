### вход по паролю и логину

![alt text](screen/1-1.png)

### вход по ssh-key

сгенерила ключ и перенесла на вм 

```ssh-keygen -t rsa -b 4096```
![alt text](screen/2-1.png)
![alt text](screen/2-2.png)

### настройка fail2ban
![alt text](screen/3-1.png)

скопировала jail.conf в jail.local для локальных настроек, поменяла только порт ```port = 2220``` и потыкала числа:
```bash
bantime = 600 # "bantime" is the number of seconds that a host is banned
findtime = 600 # A host is banned if it has generated "maxretry" during the last "findtime"
maxretry = 5  # "maxretry" is the number of failures before a host get banned
```

### настройка portknocking
![alt text](screen/4-1.png)
![alt text](screen/4-2.png)

**файл knockd.conf**

поменяла порт на нужный и ```seq_timeout``` увеличила, остальное оставила
![alt text](screen/4-3.png)
 
**проверяем, что порт явно закрыт**
![alt text](screen/4-6.png)

**стучимся и заходим, при выходе закрываем**
![alt text](screen/4-4.png)
![alt text](screen/4-5.png)

