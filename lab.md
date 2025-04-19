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
попытки зайти с неправильным паролем, доступа больше нет
![alt text](screen/3-2.png)
ура доступ есть, в статусе видно прошлый бан
![alt text](screen/3-3.png)

### настройка portknocking
![alt text](screen/4-1.png)
![alt text](screen/4-2.png)

**файл knockd.conf**

поменяла порт на нужный и ```seq_timeout``` увеличила, остальное оставила
![alt text](screen/4-3.png)

**проверяем, что порт явно закрыт**
![alt text](screen/4-6.png)
![alt text](screen/4-7.png)

**стучимся и заходим, при выходе закрываем**
![alt text](screen/4-4.png)
![alt text](screen/4-5.png)

### настройка двухфактурной аутентификации
```bash
sudo apt install libpam-google-authenticator -y
google-authenticator
```
сканим qr и соглашаемся на нужные штучки
![alt text](screen/5-1.png)
 
добавим в файл ```/etc/pam.d/sshd``` строчку:
![alt text](screen/5-2.png)
и в файл ```/etc/ssh/sshd_config ```:
![alt text](screen/5-3.png)
перезапускаем командой ```sudo systemctl restart sshd``` 

**теперь при входе нужен одноразовый пароль из Google Authenticator**
![alt text](screen/5-4.png)

### настройка GeoIP-блокировки

устанавливаем нужные библиотечки
```bash 
sudo apt install xtables-addons-common
sudo apt install xtables-addons-common libtext-csv-xs-perl
```
загружаем базу данных стран
```bash
sudo /usr/libexec/xtables-addons/xt_geoip_dl
sudo mkdir -p /usr/share/xt_geoip
sudo /usr/libexec/xtables-addons/xt_geoip_build -D /usr/share/xt_geoip
```
![alt text](screen/6-1.png)
разрешаем только РФ и сохраняем
![alt text](screen/6-2.png)

**проверяем, что блокировка работает**
![alt text](screen/6-4.png)

**при подключении под proxy сервером другой страны - никакого вывода, подключение невозможно**
![alt text](screen/6-3.png)