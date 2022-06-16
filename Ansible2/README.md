# 8.2 описание Playbook по заданию

## GROUP VARS
all - сбор информации о хостах . some_fact: 'all default fact'


## Описание Play 

### Install Vector

 - загрузка архива пакета с сайта Vector с помощью модуля ansible.builtin.get_url
используются параметры url, dest, timeout, force, validate_certs, mode

 - создние каталога с помощью модуля ansible.builtin.file , используются параметры state, path, mode


 - распаковка пакета с помощью модуля ansible.builtin.unarchive , используются параметры copy, src, dest, extra_opts 