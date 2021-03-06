# Сделать собственную сборку ядра

доустановить компилятор и различные зависимости  
`yum install -y ncurses-devel make gcc bc bison flex elfutils-libelf-devel openssl-devel grub2`  

обновить систему  
`yum update`  

посмотреть текущую версию ядра  
`cat /proc/version`  
`Linux version 3.10.0-957.21.3.el7.x86_64 (mockbuild@kbuilder.bsys.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC) ) #1 SMP Tue Jun 18 16:35:19 UTC 2019`  

скачать последнюю longterm версию ядра:  
`wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.61.tar.xz`  

распаковать новое ядро:  
`tar -xvf linux-4.19.61.tar.xz`  

переместить распакованное новое ядро:  
`mv linux-4.19.61/ /usr/src/ && cd /usr/src/linux-4.19.61/`  

скопировать старый конфиг ядра:  
`cp /boot/config-3.10.0-957.21.3.el7.x86_64 /usr/src/linux-4.19.61/.config`  

сконфигурировать конфиг нового ядра (включаем поддержку ФС reiserfs):  
`cd /usr/src/linux-4.19.61/ && make menuconfig`  

собираем ядро:  
`make -j 4` (компиляция ядра в 4 потока)       
`make modules_install && make install` (устанавливает новое ядро и модули)  

указываем загрузчику с какого ядра загружаться:  
`grubby --set-default /boot/vmlinuz-4.19.61`

проверяем результат:  
`cat /proc/version`  

`Linux version 4.19.61 (root@localhost.localdomain) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC)) #1 SMP Fri Jul 26 15:15:34 UTC 2019`  

`uname -mrs`  
`Linux 4.19.61 x86_64`  

проверяем что модуль reiserfs появился в системе:  
`ls /lib/modules/$(uname -r)/kernel/fs | grep reiser`  
`reiserfs`

[конфиг нового ядра](./.config)  
[yum.log](./yum.log)  

