# Подключение сторонних репозиториев, ручная установка пакетов

* Подключить дополнительный репозиторий на выбор: Docker, Nginx, Oracle MySQL. Установить любой пакет из этого репозитория.

>http://nginx.org/ru/linux_packages.html работа с nginx следуя инструкциям.

>sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring установил пакеты, необходимые для подключения apt-репозитория

>curl https://nginx.org/keys/nginx_signing.key | gpg —dearmor \
| sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null импортировал официальный ключ, используемый apt для проверки подлинности пакетов

>gpg —dry-run —quiet —no-keyring —import —import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg проверил, верный ли ключ был загружен

>echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
| sudo tee /etc/apt/sources.list.d/nginx.list подключил apt-репозиторий для стабильной версии nginx

>echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
| sudo tee /etc/apt/preferences.d/99nginx настроил закрепление, для использования пакетов из репозитория вместо распространяемых в дистрибутиве

>sudo apt update обновил список пакетов

>sudo apt install nginx установил nginx 

* Установить и удалить deb-пакет с помощью dpkg.

>wget https://download.virtualbox.org/virtualbox/7.0.6/virtualbox-7.0_7.0.6-155176~Ubuntu~jammy_amd64.deb скачал virtualbox

>sudo dpkg -i virtualbox-7.0_7.0.6-155176~Ubuntu~jammy_amd64.deb установил скачанный пакет

>sudo apt -f install установил зависимости

>rm -r virtualbox-7.0_7.0.6-155176~Ubuntu~jammy_amd64.deb удалил скачанный файл deb

>sudo dpkg -r virtualbox-7.0 удалил пакет

* Установить и удалить snap-пакет.

>sudo snap install gimp установил графический редактор GIMP

>snap remove gimp удалил snap пакет

* Добавить задачу для выполнения каждые 3 минуты (создание директории, запись в файл).

>crontab -e открыл crontab

>*/3 * * * *  mkdir testCron добавил задачу, создания директории

>*/3 * * * *  cd /home/may/testCron  && touch testFileCron добавил задачу, перехода в директорию, создания там файла 

>*/3 * * * *  cd /home/may/testCron  && echo "hello" >> testFileCron добавил задачу, перехода в директорию, добавления текста в файл

* Подключить PPA-репозиторий на выбор. Установить из него пакет. Удалить PPA из системы.

>sudo add-apt-repository ppa:atareao/telegram подключил PPA-репозиторий

>sudo apt update обновил списки пакетов

>sudo apt install telegram установил telegram

>sudo add-apt-repository -r ppa:atareao/telegram удалил PPA-репозиторий

* Создать задачу резервного копирования (tar) домашнего каталога пользователя. Реализовать с использованием пользовательских crontab-файлов.

>crontab -e открыл crontab

>30 8 * * 1 tar -zcf /var/backups/home.tgz /home/ добавил задачу, резервного копирования
