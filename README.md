# Домашнее задание к занятию "Резервное копирование" - `Важничев Георгий`


### Задание 1

1. `Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup`
2. `Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)`
3. `Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.`
4. `На проверку направить скриншот с командой и результатом ее выполнения`

#### Решение: 
```bash
rsync -a --checksum --verbose --delete --progress --exclude '.*' /home/jora/ /tmp/backup
```
`результатом ее выполнения`
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.1.png)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.2.png)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.3.png)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.4.png)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.5.png)

### Задание 2

1. `Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.`
2. `Резервная копия должна быть полностью зеркальной`
3. `Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции`
4. `Резервная копия размещается локально, в директории /tmp/backup`
5. `На проверку направить файл crontab и скриншот с результатом работы утилиты.`

#### Решение:
#### crontab:
[crontab](https://github.com/vajnichev/10-03-hw/blob/main/crontab)

###Результат работы утилиты

![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.6.png)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.7.png)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.8.png)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.9.png)

####Работы скрипта и утилиты
#### скрипт:
[backup.sh](https://github.com/vajnichev/10-03-hw/blob/main/backup.sh)

```bash
 #!/bin/bash
copy=$1
paste=$2

if [ ! -d $copy ]; then
	echo -e "\033[31m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Unsuccessfully! | Source directory does not exist. | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
elif [ ! -d $paste ]; then
	echo -e "\033[31m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Unsuccessfully! | Destination directory does not exist. | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
else
	rsync -a --checksum --verbose --delete --progress --exclude '.*' $copy $paste >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
  echo -e "\033[32m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Successfully! | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
fi

```
`crontab`
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.10.png)

#### rsync-cron-2025-08-02
[rsync-cron-2025-08-02.log](https://github.com/vajnichev/10-03-hw/blob/main/rsync-cron-2025-08-02.log)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.11.png)
![png](https://github.com/vajnichev/10-03-hw/blob/main/img/10.3.12.png)
