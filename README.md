# Методы и инструменты DevOps.
## ЛР по лекции 4

На корпоративной системе управления виртуализацией создаю две виртуальные машины, из корпоративного шаблона debian 12

<img width="652" height="238" alt="изображение" src="https://github.com/user-attachments/assets/d20b4822-ca77-4bda-9005-18f580808fe1" />

Указав начальные параметры при заказе ВМ, в результате на них автоматически создаётся юзер "ilya" с sudo доступом без пароля и прокинутым ssh ключом. Юзеры подгружаются из папки sudoers.d
```
ilya@podyukov-deb-2:~$ sudo cat /etc/sudoers | grep includedir
@includedir /etc/sudoers.d
```
sudo доступ без пароля даётся юзерам, входящим в группу adm
```
ilya@podyukov-deb-2:~$ sudo cat /etc/sudoers.d/adm 
User_Alias  ROOTGRP = %adm
ROOTGRP ALL=(ALL) NOPASSWD: ALL
%adm            ALL=(ALL)       NOPASSWD: ALL
```
Юзер ilya входит в группу adm => может использовать sudo без пароля
```
ilya@podyukov-deb-2:~$ id ilya
uid=1000(ilya) gid=1000(ilya) groups=1000(ilya),4(adm)
```
Серая сетка автоматически настроена, есть возможность подключения по ssh по ключам

Если бы я сам создавал юзера, я бы делал это через useradd -ms /bin/bash ilya, где -m говорит создать домашнюю директорию, а -s (shell) указывает оболочку, которую будет использовать юзер. Насколько помню, по умолчанию выдаётся sh, поэтому лучше при создании юзера явно указать bash, чтобы потом не лазить в /etc/passwd. sudo редактировал бы через visudo
