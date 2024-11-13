### Д3 1.2. Создать именованный пайп ( named pipe ). Вывести в файл через созданный pipe вывод команды ss -plnt

1. **`mkfifo ss_output`**: Создает именованный пайп с именем ss_output.
2. **`ss -plnt > ss_output &`**: Запускает команду ss и перенаправляет вывод в пайп ss_output (в фоновом режиме).
3. **`cat ss_output > ss_output.txt`**: Читает данные из пайпа ss_output и записывает их в файл ss_output.txt.
4. **`cat ss_output.txt'`**: Выводит содержимое файла ss_output.txt.

```bash
[root@Zero ~]#	mkfifo ss_output
[root@Zero ~]#  ss -plnt > ss_output &
[1] 29165
[root@Zero ~]# cat ss_output > ss_output.txt
[1]+  Done                    ss -plnt > ss_output
[root@Zero ~]# 
[root@Zero ~]# cat ss_output.txt
State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=1119,fd=5))

