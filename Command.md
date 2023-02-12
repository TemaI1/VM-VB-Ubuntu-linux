# Command

### Установка гостевых дополнений VB в Ubuntu

>sudo apt update – обновить список пакетов

>sudo apt install perl gcc make – установить пакеты

### Работа с файлами

>ls — список файлов

>pwd — текущая директория

>mkdir — создание каталога

>cp — копирование

>rm — удаление

>mv — переименование/перенос

>touch — создание пустого файла

>cat — вывод файла, склейка, создание

### Хранение данных о пользователях

>/etc/passwd – список пользователей

>/etc/group – группы пользователей

>/etc/shadow – пароли пользователей

### Управление пользователями

>useradd – создание пользователя

>adduser – создание пользователя (скрипт)

>usermod – изменение пользователя

>userdel – удаление пользователя

>passwd – изменение пароля

>chage – изменение свойств пароля

>groupadd – создание группы

>groupdel – удаление группы

### Механизм sudo и su

>su – переключение пользователя

>sudo su – переключение на суперпользователя (root)

>sudo – выполнение команды с правами суперпользователя

>/etc/sudoers – конфигурация sudo

>visudo – редактирование

### Изменение владельца и группы владельца файлов

- chown – изменение владельца и группы

> chown -R – рекурсивно

> chown testuser:testgroup

> chown testuser

- chgrp – изменение группы

>chgrp -R –рекурсивно

>chgrp testgroup

### Изменение прав доступа

- chmod — change mode, изменение прав доступа

>chmod -R testdir/ – рекурсивно

>chmod u=rwx,g=rx,o=r testfile – символьная форма

>chmod 751 testfile – числовая форма

>chmod a+x – изменение для всех

>chmod 4755 – изменение специальных битов

### Подключение дополнительного репозитория

>/etc/apt/sources.list – добавление репозитория в основной файл

>/etc/apt/sources.list.d/*.list – добавление файла с адресом репозитория

>deb http://адрес_репозитория версия_дистрибутива ветки

>deb http://nginx.org/packages/ubuntu jammy nginx

>sudo apt-key add repo.key – добавление ключа репозитория

### Утилита apt

>apt search package_name — поиск пакета по названию и
описанию

>apt list package_name — поиск пакета по имени

>apt show package_name — посмотреть информацию о пакете

>apt install package_name -y — установить пакет

>apt remove package_name — удалить пакет, при этом сохранятся файлы с настройками

>apt purge package_name — полностью удалить пакет, включая конфигурационные файлы

>apt upgrade — обновить все установленные пакеты

>apt update — обновить информацию о пакетах в репозиториях, указанных в настройках

### Утилита dpkg

>dpkg -l — просмотр списка пакетов

>dpkg -i package_name — установить пакет или группу пакетов

>dpkg -r package_name — удалить пакет или группу пакетов

### Snap-пакеты

>snap search package_name — поиск пакета

>snap install package_name — установка пакета

>snap refresh package_name — обновление пакета

>snap remove package_name — удаление пакета

>snap list — просмотр установленных пакетов

### Утилита crontab

>crontab - l вывести содержимое текущего файла расписания

>crontab -r удаление текущего файла расписания:

>crontab -e редактирование текущего файла расписания

>sudo crontab -u username – работа с файлом расписания другого пользователя

### Сетевые интерфейсы и команда ip

> ip a – список всех интерфейсов

> ip -s a – показ статистики

> ip -c -s a – включение подсветки

> ip a show enp0s3 – данные по одному интерфейсу

> ip link show enp0s3 – данные уровня L2 (link)

> ip r – просмотр информации о маршрутах

### Сокеты и порты

> ss – socket stat

> ss -ntlp – TCP-сокеты в состоянии LISTEN

> ss -ntulp – TCP и UDP-сокеты в состоянии LISTEN

> ss -tulpan – Все TCP и UDP-сокеты

### Netplan

> /etc/netplan/*.yaml – конфигурационные файлы

> netplan try – тестирование и применение конфигурации

> netplan apply – применение конфигурации

### Диагностика сети

> ping 8.8.8.8 – доступность хоста (ICMP протокол)

> ping ya.ru – проверка DNS и доступности

> host -t a yandex.ru – проверка DNS

> host -t a yandex.ru 8.8.8.8 – другой DNS-сервер

> dig @8.8.8.8 google.com – подробная информация по DNS

> tracepath ya.ru – просмотр маршрута прохождения пакетов

> traceroute ya.ru – альтернатива

> mtr ya.ru – постоянный мониторинг доступности хостов

### Правила фильтрации

- Просмотр таблицы

> iptables -L -nv

> iptables -L -nv -t nat

- Политика по умолчанию

> iptables -P INPUT DROP

- Добавление правил

> iptables -A INPUT -p tcp --dport 80 -j ACCEPT

> iptables -I INPUT -p tcp --dport 80 -j ACCEPT

> iptables -A INPUT -p tcp -s 192.168.0.100 --dport 80 -j DROP

- Удаление правил

> iptables -D INPUT 3

> iptables -D INPUT -p tcp --dport 80 -j ACCEPT

- Сброс правил

> iptables -F

### Пример конфигурации сервера

###### SSH allow
iptables -A INPUT -p tcp --dport=22 -j ACCEPT
###### HTTP, HTTPS allow
iptables -A INPUT -p tcp -m multiport --dport 80,443 -j ACCEPT
###### loopback allow
iptables -A INPUT -i lo -j ACCEPT
###### ICMP
iptables -A INPUT -p icmp -j ACCEPT
###### established connections allow
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
###### policy drop
iptables -P INPUT DROP

### Перенаправление портов

- Редирект с 80 на 8080 порт (TCP):

> iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

- Проверка:

> iptables -L -nv -t nat

### Сохранение конфигурации iptables

- Сохранение и восстановление из файла

> iptables-save > iptables.rules

> iptables-restore < iptables.rules

- Сервис netfilter-persistent

> apt install iptables-persistent netfilter-persistent

> netfilter-persistent save

> Конфигурация в /etc/iptables

### Консольные утилиты для веб

- Получить URL в консоли:

> curl -L https://ya.ru/

> wget https://yastatic.net/jquery/2.1.4/jquery.min.js

### Конфигурация Nginx

> Установка: sudo apt install nginx

> Тестирование конфигурации: sudo nginx -t

> Применить: sudo systemctl reload nginx

> Конфигурация: /etc/nginx/*

> Основной файл: /etc/nginx/nginx.conf

> Блоки: server {}

> Директивы: server_name site.ru;

> Переменные: $uri

> Документация: http://nginx.org/ru/docs/

### Конфигурация Apache 

> Установка: sudo apt install apache2

> Тестирование конфигурации: sudo apachectl -t

> Применить: sudo systemctl reload apache2

> Конфигурация: /etc/apache2/*

> Основной файл: /etc/apache2/apache2.conf

> Блоки: <VirtualHost></VirtualHost>

> Директивы: ServerName site.ru

> Документация: https://httpd.apache.org/docs/2.4/en/

### Установка MySQL и первые шаги

> Установка: apt install mysql-server-8.0

> Заходим в консоль MySQL: sudo mysql

> Переходим в системную БД mysql: use mysql;

> Получаем список пользователей: SELECT * FROM user\G

> Создаём новую базу данных: CREATE DATABASE gb;

> Создаём таблицу: CREATE TABLE test(i INT);

> Создадим записи в таблице: INSERT INTO test (i) VALUES (1),(2),(3),(4);

> Сделаем выборку из таблицы: SELECT * FROM test;

### Запуск тестового контейнера

> Установка Docker: apt install docker.io

> Проверка: sudo docker

> Создание и запуск контейнера: docker run hello-world

### Базовые операции

> docker ps – просмотр активных контейнеров

> docker ps -a – просмотр всех контейнеров

> docker images – список образов

> docker search nginx – поиск образа

> docker pull nginx – скачивание образа

> docker start|restart|stop nginx – операции с контейнером

> docker rm 9cbf7c3230d0 – удаление контейнера

> docker rmi hello-world – удаление образа

> docker logs nginx1 – просмотр логов контейнера

### Работа внутри контейнера

> Заходим: docker exec -ti nginx1 bash

> Смотрим настройки: ls -al /etc/

> Версия базового дистрибутива: cat /etc/os-release

> Настройки nginx: ls -al /etc/nginx/

> Директория сайта: ls -al /usr/share/nginx/html

### Использование Docker Compose

> apt install docker-compose – установка

> Проверка yml: apt install yamllint

> docker-compose up -d – создание и запуск

> docker-compose ps – список контейнеров

> docker-compose down – остановить и удалить

> docker-compose stop – остановить

> docker-compose start – запустить

> docker-compose rm – удалить остановленные

### Перенаправление потоков ввода-вывода

> program < file – перенаправление ввода из файла file

> program > file – перенаправление вывода (STDOUT) в файл file (запись с начала файла)

> program >> file – перенаправление вывода (STDOUT) в файл file в режиме дополнения файла

> program 2> file – перенаправление ошибок (STDERR) в файл file (запись с начала файла)

> program 2>> file – перенаправление ошибок (STDERR) в файл file в режиме дополнения файла

> program > file 2>&1 – перенаправление вывода (STDOUT) и ошибок (STDERR) в файл file
(запись с начала файла)

### Переменные и их классификация

- Переменные окружения

> $PATH

> $UID

> $PWD

- Пользовательские переменные

> var1=test

> echo $var1

- Специальные переменные

> $1…$9

> $?

### Создаём первый скрипт на bash

>cat > testscript
#!/bin/bash

>directory=$1
hidden_count=$(ls -A $directory | grep '^\.' | wc -l)

>echo “Hidden files in $directory found: $hidden_count”

### Методы запуска скрипта

> Относительный путь: ./testscript

> Абсолютный путь: /home/db/test/testscript

> Команда (должен быть в $PATH): testscript

> Через команду bash: bash testscript

### Варианты условий

- Операции сравнения строк

> = или == возвращает true (истина), если строки равны

> != возвращает true (истина), если строки не равны

> -z возвращает true (истина), если строка пуста

> -n возвращает true (истина), если строка не пуста

- Операции проверки файлов

> -e возвращает true (истина), если файл существует (exists)

> -d возвращает true (истина), если каталог существует (directory)

- Операции сравнения целых чисел (наиболее используемые)

> -eq возвращает true (истина), если числа равны (equals)

> -ne возвращает true (истина), если числа не равны (not equal)

