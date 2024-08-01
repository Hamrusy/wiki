---
description: Блокировка портов при помощи iptables
---

# Блокировка портов для BungeeCord

## Защита сервера BungeeCord путем блокировки портов <a href="#zashita-servera-bungeecord-putem-blokirovki-portov-pri-pomoshi-iptables-na-debian-11-i-docker" id="zashita-servera-bungeecord-putem-blokirovki-portov-pri-pomoshi-iptables-na-debian-11-i-docker"></a>

В данной статье мы рассмотрим, как обеспечить безопасность сервера BungeeCord на операционной системе Debian 11, блокируя определенные порты при помощи iptables. Также мы предоставим вариант для сервера, работающего в Docker-контейнере.

### Вариант 1: Блокировка портов с использованием iptables на Debian 11 <a href="#variant-1-blokirovka-portov-s-ispolzovaniem-iptables-na-debian-11" id="variant-1-blokirovka-portov-s-ispolzovaniem-iptables-na-debian-11"></a>

#### Шаг 1: Установка и настройка iptables <a href="#shag-1-ustanovka-i-nastroika-iptables" id="shag-1-ustanovka-i-nastroika-iptables"></a>

Установите и активируйте iptables, выполнив следующие команды в терминале:

```bash
sudo apt update
sudo apt install iptables
sudo iptables -F
```

#### Шаг 2: Блокировка ненужных портов <a href="#shag-2-blokirovka-nenuzhnykh-portov" id="shag-2-blokirovka-nenuzhnykh-portov"></a>

Предположим, что ваш BungeeCord сервер работает на порту 25565. Выполните следующие команды для блокировки всех входящих соединений, кроме необходимых:

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 25565 -j ACCEPT
sudo iptables -A INPUT -p tcp -j DROP
sudo iptables -A OUTPUT -p tcp -j ACCEPT
```

### Вариант 2: Блокировка портов с использованием iptables в Docker-контейнере <a href="#variant-2-blokirovka-portov-s-ispolzovaniem-iptables-v-docker-konteinere" id="variant-2-blokirovka-portov-s-ispolzovaniem-iptables-v-docker-konteinere"></a>

#### Шаг 1: Создание Docker-контейнера с BungeeCord <a href="#shag-1-sozdanie-docker-konteinera-s-bungeecord" id="shag-1-sozdanie-docker-konteinera-s-bungeecord"></a>

Создайте Docker-контейнер с BungeeCord, указав порты, которые необходимо открыть. Например, для порта 25565:

```bash
docker run -d --name bungeecord-server -p 25565:25565 -v /path/to/bungeecord/config:/config your/bungeecord-image
```

#### Шаг 2: Блокировка ненужных портов в Docker-контейнере <a href="#shag-2-blokirovka-nenuzhnykh-portov-v-docker-konteinere" id="shag-2-blokirovka-nenuzhnykh-portov-v-docker-konteinere"></a>

В Docker-контейнере iptables доступен по умолчанию. Выполните следующие команды для блокировки всех входящих соединений, кроме необходимых:

```bash
docker exec -it bungeecord-server bash
iptables -A INPUT -p tcp --dport 25565 -j ACCEPT
iptables -A INPUT -p tcp -j DROP
iptables -A OUTPUT -p tcp -j ACCEPT
exit
```

Теперь ваш сервер BungeeCord должен быть защищен от нежелательных входящих соединений благодаря блокировке ненужных портов при помощи iptables на Debian 11 и в Docker-контейнере.

### Сохранение и восстановление правил iptables <a href="#sokhranenie-i-vosstanovlenie-pravil-iptables" id="sokhranenie-i-vosstanovlenie-pravil-iptables"></a>

После настройки iptables, рекомендуется сохранить правила для последующего использования. В Debian 11 выполните следующие команды:

```bash
sudo apt install iptables-persistent
sudo sh -c 'iptables-save > /etc/iptables/rules.v4'
```

Для Docker-контейнера выполните следующие команды:

```bash
docker exec -it bungeecord-server bash
iptables-save > /config/iptables.rules
exit
```

Теперь правила iptables будут сохранены и применены автоматически при перезагрузке сервера или Docker-контейнера. В случае необходимости восстановления правил выполните следующие команды:

```bash
sudo iptables-restore < /etc/iptables/rules.v4
```

Для Docker-контейнера:

```bash
docker exec -it bungeecord-server bash
iptables-restore < /config/iptables.rules
exit
```

Используя эти рекомендации, вы сможете обеспечить безопасность вашего сервера BungeeCord, блокируя ненужные порты при помощи iptables на Debian 11 и в Docker-контейнере.
