 # 8.2 описание Playbook по заданию

## GROUP VARS
clickhouse_packages - имя пакета установки Clickhouse
clickhouse_version - используемая версия Clickhouse

vector_packages - имя пакета установки Vector
vector_version - используемая версия Vector

## Описание Play 

### Install Clickhouse

 - загрузка пакета с сайта Clickhouse с помощью модуля ansible.builtin.get_url
используются параметры url, dest, timeout, force, validate_certs, mode

 - установка пакета с помощью модуля ansible.builtin.yum , используются параметры notify

 - создание базы данных с помощью модуля ansible.builtin.command, используются параметры register, failed_when, changed_when 

### Install Vector

 - загрузка архива пакета с сайта Vector с помощью модуля ansible.builtin.get_url
используются параметры url, dest, timeout, force, validate_certs, mode

 - создние каталога с помощью модуля ansible.builtin.file , используются параметры state, path, mode

 - распаковка пакета с помощью модуля ansible.builtin.unarchive , используются параметры copy, src, dest, extra_opts 