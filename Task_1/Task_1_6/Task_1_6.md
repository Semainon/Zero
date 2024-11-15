### Д3 1.6. Требуется переместить ядро или переименовать его. Перезагрузить систему.
Восстановить систему двумя способами.


### Первый способ. Kоманды:
1. **`uname -r`**: Выводит текущую версию ядра, в данном случае, 5.4.17-2136.310.7.1.el8uek.x86_64.
2. **`ls /boot`**: Смотрим содержимое каталога boot, где хранятся:
- сжатые образы ядра: файлы вида 'vmlinuz-', которые представляют собой сжатые образы ядра, 
- образы инициализации: файлы вида "initrd.img-", которые используются для инициализации системы во время загрузки;
- конфигурационные файлы, которые содержат настройки и конфигурации для загрузки системы.
3. **`sudo mv /boot/vmlinuz-5.4.17-2136.310.7.1.el8uek.x86_64 /boot/vmlinuz-zero`**: 
   - Переименовываем текущее ядро в `vmlinuz-zero`.
4. **`sudo mv /boot/initramfs-5.4.17-2136.310.7.1.el8uek.x86_64.img /boot/initramfs-zero.img`**: 
   - Переименовываем файл инициализации (initramfs) в `initramfs-zero.img`, чтобы он соответствовал новому имени ядра.
5. **`sudo nano /boot/loader/entries/06088db1377a47b2bcd19b4355d5886f-5.4.17-2136.310.7.1.el8uek.x86_64.conf`**: 
   - Открываем конфигурационный файл загрузчика для редактирования, чтобы обновить пути к ядру и initramfs.
6. **`sudo systemctl restart sshd`**: 
   - Перезапускаем службу SSH, чтобы убедиться, что все изменения конфигурации вступили в силу и служба работает корректно.
7. **`sudo systemctl reboot`**: 
   - Перезагружаем систему, чтобы применить изменения и загрузить новое ядро.
8. **`ssh root@**.***.***.**`**: 
   - Подключаемся к системе по SSH после перезагрузки. Если всё прошло успешно, мы должны увидеть сообщение о подключении.
9. **`Возвращаем исходные имена ядра и initramfs, редактируем конфиг, etc.`**: 
   
```bash
[root@Zero ~]# uname -r
5.4.17-2136.310.7.1.el8uek.x86_64
[root@Zero ~]# ls /boot
System.map-4.18.0-372.26.1.0.1.el8_6.x86_64   grub2                                                    symvers-4.18.0-372.26.1.0.1.el8_6.x86_64.gz
System.map-5.4.17-2136.310.7.1.el8uek.x86_64  initramfs-0-rescue-06088db1377a47b2bcd19b4355d5886f.img  symvers-5.4.17-2136.310.7.1.el8uek.x86_64.gz
config-4.18.0-372.26.1.0.1.el8_6.x86_64       initramfs-4.18.0-372.26.1.0.1.el8_6.x86_64.img           vmlinuz-0-rescue-06088db1377a47b2bcd19b4355d5886f
config-5.4.17-2136.310.7.1.el8uek.x86_64      initramfs-5.4.17-2136.310.7.1.el8uek.x86_64.img          vmlinuz-4.18.0-372.26.1.0.1.el8_6.x86_64
efi                                           loader                                                   vmlinuz-5.4.17-2136.310.7.1.el8uek.x86_64
[root@Zero ~]# sudo mv /boot/vmlinuz-5.4.17-2136.310.7.1.el8uek.x86_64 /boot/vmlinuz-zero
[root@Zero ~]# sudo mv /boot/initramfs-5.4.17-2136.310.7.1.el8uek.x86_64.img /boot/initramfs-zero.img
[root@Zero ~]# ls /boot/loader/entries/
06088db1377a47b2bcd19b4355d5886f-0-rescue.conf                          06088db1377a47b2bcd19b4355d5886f-5.4.17-2136.310.7.1.el8uek.x86_64.conf
06088db1377a47b2bcd19b4355d5886f-4.18.0-372.26.1.0.1.el8_6.x86_64.conf
[root@Zero ~]# sudo nano /boot/loader/entries/06088db1377a47b2bcd19b4355d5886f-5.4.17-2136.310.7.1.el8uek.x86_64.conf
[root@Zero ~]# sudo systemctl restart sshd 
[root@Zero ~]# sudo systemctl reboot
Connection to **.***.***.** closed by remote host.
Connection to **.***.***.** closed.
Trick@MBP-Zero ~ % ssh root@**.***.***.**
[root@Zero ~]# sudo mv /boot/vmlinuz-zero /boot/vmlinuz-5.4.17-2136.310.7.1.el8uek.x86_64
[root@Zero ~]# sudo mv /boot/initramfs-zero.img /boot/initramfs-5.4.17-2136.310.7.1.el8uek.x86_64.img
[root@Zero ~]# ls /boot/loader/entries/
06088db1377a47b2bcd19b4355d5886f-0-rescue.conf  06088db1377a47b2bcd19b4355d5886f-4.18.0-372.26.1.0.1.el8_6.x86_64.conf  06088db1377a47b2bcd19b4355d5886f-5.4.17-2136.310.7.1.el8uek.x86_64.conf
[root@Zero ~]# sudo nano /boot/loader/entries/06088db1377a47b2bcd19b4355d5886f-5.4.17-2136.310.7.1.el8uek.x86_64.conf
[root@Zero ~]# sudo systemctl restart sshd
root@Zero ~]# sudo systemctl reboot
Connection to **.***.***.** closed by remote host.
Connection to **.***.***.** closed.
Trick@MBP-Zero ~ % ssh root@**.***.***.**
Activate the web console with: systemctl enable --now cockpit.socket

Last login: Fri Nov 15 21:54:14 2024 from **.***.***.**
[root@Zero ~]# 
```


### Второй способ восстановления: Перед переиенованием в веб-консоли был добавленн снапшот. После переименования – исходное состояние восстановлено из снапшота. Примечание: в контексте учебной задачи данный способ является самым простым, в реальных условиях – имеет ряд ограничений. 

