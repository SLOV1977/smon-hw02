# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Рахманов Александр`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

## Решение.

### Задание 1.

### Установите Zabbix Server с веб-интерфейсом.

```
# На slov-VirtualBox (zabbix server):

sudo -i
apt update
apt install postgresql
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.0+ubuntu22.04_all.deb
dpkg -i zabbix-release_latest_6.0+ubuntu22.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts
su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"'
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"'
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql Zabbix
sed -i 's/# DBPassword=/DBPassword=13579/g' /etc/zabbix/zabbix_server.conf
systemctl restart zabbix-server apache2
systemctl enable zabbix-server apache2

```

![Web-интерфейс Zabbix](https://github.com/SLOV1977/smon-hw02/tree/main/img/smon-hw-02-01.png)

![Web-интерфейс Zabbix](smon-hw-02-01.png)

---

### Задание 2.

### Установите Zabbix Agent на два хоста.

```
# На slov-VirtualBox (zabbix agent):

wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.0+ubuntu22.04_all.deb
dpkg -i zabbix-release_latest_6.0+ubuntu22.04_all.deb
apt update
apt install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
sed -i 's/Server=127.0.0.1/Server=10.0.2.15'/g' /etc/zabbix/zabbix_server.conf
systemctl restart zabbix-agent
systemctl enable zabbix-agent

# На DESKTOP-PVKD5GR (zabbix agent):

sudo -i
apt update
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian12_all.deb
dpkg -i zabbix-release_latest_6.0+debian12_all.deb
apt update
apt install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
sed -i 's/Server=127.0.0.1/Server=172.19.96.1'/g' /etc/zabbix/zabbix_server.conf
systemctl restart zabbix-agent
systemctl enable zabbix-agent

```

![Раздел "Configuration > Hosts"](https://github.com/SLOV1977/smon-hw02/tree/main/img/smon-hw-02-02.png)

![Раздел "Configuration > Hosts"](smon-hw-02-02.png)


![Лог zabbix agent на slov-VirtualBox](https://github.com/SLOV1977/smon-hw02/tree/main/img/smon-hw-02-04.png)

![Лог zabbix agent на slov-VirtualBox](smon-hw-02-04.png)


![Лог zabbix agent на DESKTOP-PVKD5GR](https://github.com/SLOV1977/smon-hw02/tree/main/img/smon-hw-02-03.png)

![Лог zabbix agent на DESKTOP-PVKD5GR](smon-hw-02-03.png)


![Раздел "Monitoring > Latest data" (общий)](https://github.com/SLOV1977/smon-hw02/tree/main/img/smon-hw-02-05.png)

![Раздел "Monitoring > Latest data" (общий)](smon-hw-02-05.png)


![Раздел "Monitoring > Latest data" (slov-VirtualBox)](https://github.com/SLOV1977/smon-hw02/tree/main/img/smon-hw-02-06.png)

![Раздел "Monitoring > Latest data" (slov-VirtualBox)](smon-hw-02-06.png)


![Раздел "Monitoring > Latest data" (DESKTOP-PVKD5GR)](https://github.com/SLOV1977/smon-hw02/tree/main/img/smon-hw-02-07.png)

![Раздел "Monitoring > Latest data" (DESKTOP-PVKD5GR)](smon-hw-02-07.png)

---
