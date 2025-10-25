# Подюков Илья. ФИТ-2-2024 НМ. Методы и инструменты DevOps. ЛР по лекции 4
## Подготовка
На корпоративной системе управления виртуализацией создаю две виртуальные машины, из корпоративного шаблона debian 12

<img width="242" height="230" alt="изображение" src="https://github.com/user-attachments/assets/271ea4d8-3355-4395-a59b-a88e3d550d73" />

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

## Установка ansible
Кратко: для установки ansible я сначала создаю виртуальное окружение python, и в нём через pip устанавливаю ansible
```
ilya@msi ~> python3 -m venv ansible_venv
ilya@msi ~> cd ansible_venv/
ilya@msi ~/ansible_venv> source bin/activate.fish
(ansible_venv) ilya@msi ~/ansible_venv>
(ansible_venv) ilya@msi ~/ansible_venv> pip install ansible
# процесс установки пропускаю
Installing collected packages: resolvelib, PyYAML, pycparser, packaging, MarkupSafe, jinja2, cffi, cryptography, ansible-core, ansible
Successfully installed MarkupSafe-3.0.3 PyYAML-6.0.3 ansible-12.1.0 ansible-core-2.19.3 cffi-2.0.0 cryptography-46.0.3 jinja2-3.1.6 packaging-25.0 pycparser-2.23 resolvelib-1.2.1
(ansible_venv) ilya@msi ~/ansible_venv>
(ansible_venv) ilya@msi ~/ansible_venv> ansible --version
ansible [core 2.19.3]
  config file = None
  configured module search path = ['/home/ilya/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/ilya/ansible_venv/lib/python3.13/site-packages/ansible
  ansible collection location = /home/ilya/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/ilya/ansible_venv/bin/ansible
  python version = 3.13.7 (main, Aug 15 2025, 12:34:02) [GCC 15.2.1 20250813] (/home/ilya/ansible_venv/bin/python3)
  jinja version = 3.1.6
  pyyaml version = 6.0.3 (with libyaml v0.2.5)
```

## Подготовка плейбука
Для установки пакета мне понадобится модуль package, для копирования модуль copy. Файл nginx.yml с комментариями в репозитории.

## Подготовка inventory
Чтобы не показывать ip адреса машинок, я принял решение заменить их DNS записями, прописал в /etc/hosts содержимое:
```
<ip1> podyukov-deb-2
<ip2> podyukov-deb-5
```
Файл inventory в репозитории

## Запуск плейбука
Перехожу в папку с данным репозиторием и запускаю плейбук

<img width="1041" height="626" alt="изображение" src="https://github.com/user-attachments/assets/fd6d6945-4394-4580-8723-ceec607d3e29" />

Плейбук успешно раскатился, проверяю результат

<img width="518" height="131" alt="изображение" src="https://github.com/user-attachments/assets/09170a2c-710b-4774-a394-7453ab7540e1" />
