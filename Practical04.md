# Настройка сети в Linux. Работа с IPtables

* Настроить статическую конфигурацию в (без DHCP) Ubuntu через ip и netplan. Настроить IP, маршрут по умолчанию и DNS-сервера (1.1.1.1 и 8.8.8.8). Проверить работоспособность сети.

>cd /etc/netplan/ открыл директорию netplan

>sudo nano 01-network-manager-all.yaml открыл конфигурационный файл

>изменил конфигурационный файл:

        network: 
            version: 2      версия YAML
            renderer: networkd      менеджер сети (networkd или NetworkManager)
            ethernets:              настройка сетевых адаптеров ethernet
                enp0s3:             интерфейс сети
                dhcp4: no           будет ли получать сетевой адаптер IP-адрес автоматически
                addresses: [192.168.0.8/24, 192.168.0.237/24]  задает IP-адреса через запятую, настройка сети (IP-адрес, шлюз, сервер DNS)
                gateway4: 192.168.0.1   настройка сети (IP-адрес, шлюз, сервер DNS)
                nameservers:        настройка серверов имен, настройка сети (IP-адрес, шлюз, сервер DNS)
                    addresses: 
                        8.8.8.8 
                        1.1.1.1 

* Настроить правила iptables для доступности сервисов на TCP-портах 22, 80 и 443. Также сервер должен иметь возможность устанавливать подключения к серверу обновлений. Остальные подключения запретить.

>sudo iptables –nvL посмотрел таблицу 

>sudo iptables -A INPUT -i lo -j ACCEPT добавил правило

>sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT добавил правило

>sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT открыл порт 22 SSH

>sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT открыл порт 80 HTTP

>sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT открыл порт 443 HTTPS

>sudo iptables -P INPUT DROP поменял политику ACCEPT на DROP

* Запретить любой входящий трафик с IP 3.4.5.6.

>sudo iptables -I INPUT -s 3.4.5.6 -j DROP

* Запросы на порт 8090 перенаправлять на порт 80 (на этом же сервере).

>sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

* Разрешить подключение по SSH только из сети 192.168.0.0/24.

>sudo iptables -I INPUT -p tcp --dport 22 -s 192.168.0.0/24 -j ACCEPT

