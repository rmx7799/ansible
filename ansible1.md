# Домашнее задание к занятию "08.01 Введение в Ansible"

## Подготовка к выполнению
1. Установите ansible версии 2.10 или выше.
2. Создайте свой собственный публичный репозиторий на github с произвольным именем.
3. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.

## Основная часть
1. Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте какое значение имеет факт `some_fact` для указанного хоста при выполнении playbook'a.

Ответ:

root@vagrant:/home/vagrant/ansible1# ls
group_vars  inventory  site.yml
root@vagrant:/home/vagrant/ansible1# ansible-playbook -i inventory/test.yml site.yml

PLAY [Print os facts] ********************************************************************************

TASK [Gathering Facts] *******************************************************************************
ok: [localhost]

TASK [Print OS] **************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ************************************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP *******************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible1#



2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.

Ответ:

root@vagrant:/home/vagrant/ansible1# cat group_vars/all/examp.yml
---
  some_fact: 'all default fact'
root@vagrant:/home/vagrant/ansible1#
root@vagrant:/home/vagrant/ansible1# ansible-playbook -i inventory/test.yml site.yml

PLAY [Print os facts] ********************************************************************************

TASK [Gathering Facts] *******************************************************************************
ok: [localhost]

TASK [Print OS] **************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP *******************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible1#


3. Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.

Ответ:
создал контейнер centos7 и поправил конфиг prod.yml для ubuntu

root@vagrant:/home/vagrant/ansible1# docker ps

CONTAINER ID   IMAGE                 COMMAND            CREATED             STATUS             PORTS     NAMES

62b64e9c5ce8   pycontribs/centos:7   "sleep 60000000"   About an hour ago   Up About an hour             centos7

root@vagrant:/home/vagrant/ansible1#



4. Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.

Ответ:

root@vagrant:/home/vagrant/ansible1# ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] ********************************************************************************

TASK [Gathering Facts] *******************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] **************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP *******************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible1#

5. Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились следующие значения: для `deb` - 'deb default fact', для `el` - 'el default fact'.

Ответ:

root@vagrant:/home/vagrant/ansible1# cat group_vars/deb/examp.yml ;echo ""
---
  some_fact: "deb default fact"

root@vagrant:/home/vagrant/ansible1# cat group_vars/el/examp.yml ;echo ""
---
  some_fact: "el default fact"

root@vagrant:/home/vagrant/ansible1# cat group_vars/all/examp.yml ;echo ""
---
  some_fact: 'all default fact'

root@vagrant:/home/vagrant/ansible1#


6.  Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.

Ответ:

root@vagrant:/home/vagrant/ansible1# ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] ********************************************************************************

TASK [Gathering Facts] *******************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] **************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *******************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible1#


7. При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.

Ответ:

Зафишровал с паролем Ansible.


root@vagrant:/home/vagrant/ansible1# ansible-vault encrypt group_vars/deb/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful
root@vagrant:/home/vagrant/ansible1#
root@vagrant:/home/vagrant/ansible1# ansible-vault encrypt group_vars/el/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful
root@vagrant:/home/vagrant/ansible1#
root@vagrant:/home/vagrant/ansible1#



8. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.

Ответ:

root@vagrant:/home/vagrant/ansible1# ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ********************************************************************************

TASK [Gathering Facts] *******************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] **************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *******************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible1#



9. Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.

Ответ:

подходит "local"

10. В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.

Ответ:

root@vagrant:/home/vagrant/ansible1# cat inventory/prod.yml
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: local

  local:
    hosts:
      localhost:
        ansible_connection: local
root@vagrant:/home/vagrant/ansible1#


11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь что факты `some_fact` для каждого из хостов определены из верных `group_vars`.

Ответ:

добавил директорию local в group_vars и создал конфиг examp.yml

root@vagrant:/home/vagrant/ansible1# ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ********************************************************************************

TASK [Gathering Facts] *******************************************************************************
ok: [ubuntu]
ok: [localhost]
ok: [centos7]

TASK [Print OS] **************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "local default fact"
}

PLAY RECAP *******************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible1#


12. Заполните `README.md` ответами на вопросы. Сделайте `git push` в ветку `master`. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым `playbook` и заполненным `README.md`.

Ответ:
ссылка https://github.com/rmx7799/ansible/tree/main/ansible1/ansible1.md



## Необязательная часть

1. При помощи `ansible-vault` расшифруйте все зашифрованные файлы с переменными.
2. Зашифруйте отдельное значение `PaSSw0rd` для переменной `some_fact` паролем `netology`. Добавьте полученное значение в `group_vars/all/exmp.yml`.
3. Запустите `playbook`, убедитесь, что для нужных хостов применился новый `fact`.
4. Добавьте новую группу хостов `fedora`, самостоятельно придумайте для неё переменную. В качестве образа можно использовать [этот](https://hub.docker.com/r/pycontribs/fedora).
5. Напишите скрипт на bash: автоматизируйте поднятие необходимых контейнеров, запуск ansible-playbook и остановку контейнеров.
6. Все изменения должны быть зафиксированы и отправлены в вашей личный репозиторий.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---