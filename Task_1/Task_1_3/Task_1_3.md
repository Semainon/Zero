### Д3 1.3. При помощи именованного пайпа заархивировать всё, что в него отправляем.Например, содержимое файла /var/log/messages.На выходе получить архив tar или любой другой

### Kоманды получения имени интерфейса, подлюченного к Интернет и его IP-адреса

1. **`mkfifo arch`**: Создает именованный пайп с именем arch.
2. **`tar -cvf archive.tar -C /var/log messages < arch &`**: Используем tar для создания архива с именем archive.tar:
	-c — создать новый архив/v — выводить информацию о процессе/f archive.tar — указать имя выходного файла архива. Архивированные данные будут поступать в именованный пайп arch. -C /var/log — сменить директорию на /var/log, чтобы архивировать файл messages.
3. **`cat /var/log/messages > arch`**: Отправка содержимого файла /var/log/messages в именованный пайп.


```bash
[root@Zero ~]# mkfifo arch
[root@Zero ~]# tar -cvf archive.tar -C /var/log messages < arch &
[1] 106069
[root@Zero ~]# cat /var/log/messages > arch
messages
[1]+  Done                    tar -cvf archive.tar -C /var/log messages < arch
[root@Zero ~]# tar -tvf archive.tar
-rw------- root/root    114946 2024-11-15 00:00 messages
[root@Zero ~]# ls -l
total 128
prw-r--r--. 1 root root      0 Nov 15 00:26 arch
-rw-r--r--. 1 root root 122880 Nov 15 00:26 archive.tar
-rw-r--r--. 1 root root    312 Nov 14 23:41 poem.txt
prw-r--r--. 1 root root      0 Nov 13 14:23 ss_output
-rw-r--r--. 1 root root    154 Nov 13 14:23 ss_output.txt
prw-r--r--. 1 root root      0 Nov 14 11:27 unixtime


