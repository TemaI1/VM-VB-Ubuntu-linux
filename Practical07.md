# Скрипты Bash

* Написать скрипт очистки директорий. На вход принимает путь к директории. Если директория существует, то удаляет в ней все файлы с расширениями .bak, .tmp, .backup. Если директории нет, то выводит ошибку.

>mkdir testScript создал директорию

>touch test.txt test2.bak test3.tmp test4.backup создал файлы

    cat > testSc создал файл с содержимым:
    #!/bin/bash
    if [ -d $1 ]
    then
    rm -r *.bak *.tmp *.backup
    echo "files with bak, tmp, backup extensions have been deleted"else
    echo "there is no such directory"
    fi
    directory=$1

>chmod +x testSc добавил возможность исполнения

>bash testSc /home/may2/testScript/ запустил скрипт, удалил файлы с расширениями .bak, .tmp, .backup
