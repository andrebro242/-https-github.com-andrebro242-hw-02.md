# -https-github.com-andrebro242-hw-02.md
Домашнее задания "Система мониторинга Zabbix" Брюхов А.
Решение 1
1. Установил PostgreSQL с помощью следующей команды:
sudo apt install postgresql

2.Скачал и установл репозиторий Zabbix:

wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian11_all.deb
sudo dpkg -i zabbix-release_6.4-1+debian11_all.deb
sudo apt update

3.Установил Zabbix Server, веб-интерфейс и агент:

sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent

4.Создал базу данных:

sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix

5.Импортировал начальную схему и данные в базу данных:

zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

5.Настроил базу данных для Zabbix Server:
Отредактировал файл /etc/zabbix/zabbix_server.conf и установил пароль для БД:
DBPassword=пароль от БД

6.Запустил процессы Zabbix Server и агента:

    sudo systemctl restart zabbix-server zabbix-agent apache2
    sudo systemctl enable zabbix-server zabbix-agent apache2
7.В браузере http://localhost/zabbix
8.Далее настройки

Решение 2

На обеих виртуальных машинах установил Zabbix Agent:

sudo apt update
sudo apt install zabbix-agent

Добавил Zabbix Server в список разрешенных серверов в Zabbix Agent. Отредактировал файл конфигурации /etc/zabbix/zabbix_agentd.conf:

sudo nano /etc/zabbix/zabbix_agentd.conf

изменил следующую строку, добавил IP-адрес Zabbix Server:

Server=IP моего Zabbix Server 

Перезапустил службу Zabbix Agent:

    sudo systemctl restart zabbix-agent

    На сервере Zabbix добавил хосты в разделе Configuration > Hosts.
