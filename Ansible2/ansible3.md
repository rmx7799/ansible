# Домашнее задание к занятию "08.02 Работа с Playbook"

## Подготовка к выполнению

1. Создайте свой собственный (или используйте старый) публичный репозиторий на github с произвольным именем.
2. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.
3. Подготовьте хосты в соответствии с группами из предподготовленного playbook.

## Основная часть

1. Приготовьте свой собственный inventory файл `prod.yml`.

Ответ:

приготовил.
файл prod.yml ниже.

---

clickhouse:

  hosts:

    clickhouse-01:

      ansible_host: localhost

vector:

  hosts:

    vector-01:

      ansible_host: localhost


2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает [vector](https://vector.dev).

Ответ:
сделал.

3. При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.

Ответ:
сделал.

4. Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, установить vector.

Ответ:
сделал. 

файл site3.yml ниже.
---
- name: Install Vector

  hosts: all
  
  tasks:

      - name: Upload tar.gz Vector from remote URL
        ansible.builtin.get_url:
            url: https://packages.timber.io/vector/latest/vector-x86_64-unknown-linux-gnu.tar.>
            dest: /tmp/vector-x86_64-unknown-linux-gnu.tar.gz
            timeout: 60
            force: true
            validate_certs: false
            mode: 0644
      - name: Create directrory for Vector
        ansible.builtin.file:
            state: directory
            path: /var/lib/vector
            mode: 0644
      - name: Extract Vector in the installation directory
        ansible.builtin.unarchive:
            copy: false
            src: /tmp/vector-x86_64-unknown-linux-gnu.tar.gz
            dest: /usr/local/bin
            extra_opts: [--strip-components=2]



5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.

Ответ:
сделал. 

root@vagrant:/home/vagrant/ansible2# ansible-lint site.yml -vvv

DEBUG    Logging initialized to level 10

DEBUG    Options: Namespace(cache_dir='/root/.cache/ansible-compat/5bc770', 
colored=True, config_file=None, configured=True, cwd=PosixPath('/home/vagrant/ansible2'), display_relative_path=True, enable_list=[], exclude_paths=['.cache', '.git', '.hg', '.svn', '.tox'], extra_vars=None, format='rich', kinds=[{'jinja2': '**/*.j2'}, {'jinja2': '**/*.j2.*'}, {'yaml': '.github/**/*.{yaml,yml}'}, {'text': '**/templates/**/*.*'}, {'execution-environment': '**/execution-environment.yml'}, {'ansible-lint-config': '**/.ansible-lint'}, {'ansible-lint-config': '**/.config/ansible-lint.yml'}, {'ansible-navigator-config': '**/ansible-navigator.{yaml,yml}'}, {'inventory': '**/inventory/**.yml'}, {'requirements': '**/meta/requirements.yml'}, {'galaxy': '**/galaxy.yml'}, {'reno': '**/releasenotes/*/*.{yaml,yml}'}, {'playbook': '**/playbooks/*.{yml,yaml}'}, {'playbook': '**/*playbook*.{yml,yaml}'}, {'role': '**/roles/*/'}, {'tasks': '**/tasks/**/*.{yaml,yml}'}, {'handlers': '**/handlers/*.{yaml,yml}'}, {'vars': '**/{host_vars,group_vars,vars,defaults}/**/*.{yaml,yml}'}, {'test-meta': '**/tests/integration/targets/*/meta/main.{yaml,yml}'}, {'meta': '**/meta/main.{yaml,yml}'}, {'meta-runtime': '**/meta/runtime.{yaml,yml}'}, {'arg_specs': '**/roles/**/meta/argument_specs.{yaml,yml}'}, {'yaml': '.config/molecule/config.{yaml,yml}'}, {'requirements': '**/molecule/*/{collections,requirements}.{yaml,yml}'}, {'yaml': '**/molecule/*/{base,molecule}.{yaml,yml}'}, {'requirements': '**/requirements.yml'}, {'playbook': '**/molecule/*/*.{yaml,yml}'}, {'yaml': '**/{.ansible-lint,.yamllint}'}, {'yaml': '**/*.{yaml,yml}'}, {'yaml': '**/.*.{yaml,yml}'}], lintables=['site.yml'], listrules=False, listtags=False, loop_var_prefix=None, mock_modules=[], mock_roles=[], offline=None, parseable=False, progressive=False, project_dir='.', quiet=0, rules={}, rulesdir=[], rulesdirs=['/usr/local/lib/python3.8/dist-packages/ansiblelint/rules'], skip_action_validation=True, skip_list=[], tags=[], use_default_rules=False, var_naming_pattern=None, verbosity=3, version=False, warn_list=['experimental', 'role-name'], write_list=None)

DEBUG    /home/vagrant/ansible2
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/5bc770/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/5bc770/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/5bc770/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
DEBUG    Loading rules from /usr/local/lib/python3.8/dist-packages/ansiblelint/rules
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/5bc770/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/5bc770/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/5bc770/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
DEBUG    Effective yamllint rules used: {'braces': {'level': 'error', 'forbid': False, 'min-spaces-inside': 0, 'max-spaces-inside': 1, 'min-spaces-inside-empty': -1, 'max-spaces-inside-empty': -1}, 'brackets': {'level': 'error', 'forbid': False, 'min-spaces-inside': 0, 'max-spaces-inside': 0, 'min-spaces-inside-empty': -1, 'max-spaces-inside-empty': -1}, 'colons': {'level': 'error', 'max-spaces-before': 0, 'max-spaces-after': 1}, 'commas': {'level': 'error', 'max-spaces-before': 0, 'min-spaces-after': 1, 'max-spaces-after': 1}, 'comments': {'level': 'warning', 'require-starting-space': True, 'ignore-shebangs': True, 'min-spaces-from-content': 1}, 'comments-indentation': False, 'document-end': False, 'document-start': False, 'empty-lines': {'level': 'error', 'max': 2, 'max-start': 0, 'max-end': 0}, 'empty-values': False, 'hyphens': {'level': 'error', 'max-spaces-after': 1}, 'indentation': {'level': 'error', 'spaces': 'consistent', 'indent-sequences': True, 'check-multi-line-strings': False}, 'key-duplicates': {'level': 'error'}, 'key-ordering': False, 'line-length': {'level': 'error', 'max': 160, 'allow-non-breakable-words': True, 'allow-non-breakable-inline-mappings': False}, 'new-line-at-end-of-file': {'level': 'error'}, 'new-lines': {'level': 'error', 'type': 'unix'}, 'octal-values': False, 'quoted-strings': False, 'trailing-spaces': {'level': 'error'}, 'truthy': {'level': 'warning', 'allowed-values': ['true', 'false'], 'check-keys': True}}
WARNING  Overriding detected file kind 'yaml' with 'playbook' for given positional argument: site.yml

INFO     Executing syntax check on site.yml (0.61s)

WARNING  Listing 1 violation(s) that are fatal

syntax-check: A malformed block was encountered while loading a block

site.yml:11:9 [WARNING]: While constructing a mapping from /home/vagrant/ansible2/site.yml,

line 24, column 9, found a duplicate dict key (name). Using last defined value
only.

ERROR! A malformed block was encountered while loading a block


The error appears to be in '/home/vagrant/ansible2/site.yml': line 11, column 9, but may

be elsewhere in the file depending on the exact syntax problem.


The offending line appears to be:


  tasks:

      - block: test1

        ^ here


Finished with 1 failure(s), 0 warning(s) on 1 files.



6. Попробуйте запустить playbook на этом окружении с флагом `--check`.

Ответ:
сделал. 



root@vagrant:/home/vagrant/ansible2# ansible-playbook -i inventory/prod.yml site.yml --check

[WARNING]: While constructing a mapping from /home/vagrant/ansible2/site.yml, line 24, column 9, found a duplicate

dict key (name). Using last defined value only.

ERROR! A malformed block was encountered while loading a block


The error appears to be in '/home/vagrant/ansible2/site.yml': line 11, column 9, but may

be elsewhere in the file depending on the exact syntax problem.


The offending line appears to be:


  tasks:

      - block: test1

        ^ here




7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.

Ответ:
сделал. 


root@vagrant:/home/vagrant/ansible2# ansible-playbook -i inventory/prod.yml site.yml --diff

[WARNING]: While constructing a mapping from /home/vagrant/ansible2/site.yml, line 24, column 9, found a duplicate

dict key (name). Using last defined value only.

ERROR! A malformed block was encountered while loading a block


The error appears to be in '/home/vagrant/ansible2/site.yml': line 11, column 9, but may

be elsewhere in the file depending on the exact syntax problem.


The offending line appears to be:


  tasks:

      - block: test1

        ^ here

root@vagrant:/home/vagrant/ansible2#




8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.

Ответ:
сделал. 

root@vagrant:/home/vagrant/ansible2# ansible-playbook -i inventory/prod.yml site.yml --diff

[WARNING]: While constructing a mapping from /home/vagrant/ansible2/site.yml, line 24, column 9, found a duplicate

dict key (name). Using last defined value only.

ERROR! A malformed block was encountered while loading a block


The error appears to be in '/home/vagrant/ansible2/site.yml': line 11, column 9, but may

be elsewhere in the file depending on the exact syntax problem.


The offending line appears to be:

  tasks:

      - block: test1

        ^ here

root@vagrant:/home/vagrant/ansible2#





9. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.

Ответ:
ссылка.

https://github.com/rmx7799/ansible/tree/main/Ansible2

10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-02-playbook` на фиксирующий коммит, в ответ предоставьте ссылку на него.

Ответ:
выполнил команды по добавлению tag.

User@DCE-HPE-AIV MINGW64 ~/netology-ansible/ansible/Ansible2 (main)


$  git tag -fa 08-ansible-02-playbook a45f7f3add5f6569fdac845d84c0b8e8a36043bf
Updated tag '08-ansible-02-playbook' (was 9993a29)


User@DCE-HPE-AIV MINGW64 ~/netology-ansible/ansible/Ansible2 (main)

$

User@DCE-HPE-AIV MINGW64 ~/netology-ansible/ansible/Ansible2 (main)

$ git log --pretty=oneline
a45f7f3add5f6569fdac845d84c0b8e8a36043bf (HEAD -> main, tag: 08-ansible-02-playbook, origin/origin, origin/main, origin/HEAD) commit

a832d8458a11ae24471e35c51832de7c0482e2e1 commit

5b806517543e48611533e64b871f6c578852ef50 commit

d073957a4a06328e818d8a7c88bc0b21d6a859cd Add files via upload

916cf53d67e33135862c01a83cccda9008ce170b Create examp.yml

257e59f6e0222fe9e927bb12c86ab92743e55c5a Create examp.yml

e8e9e1d126cadacc6415d44aa508e0c005ad75e6 Create examp.yml

0df083277531d6ca09d74f79b8235c0a8995ffdc Create examp.yml

b6553857edfe42544b4ae0b88baedae5325c3414 Create prod.yml

fae7e86c7ed41290927325a782c6866399dce67f Add files via upload

ee9953e8269f58e5b16304435bed26fcd16d8ee1 Create test

6eb5b4db6a1428294045f08ca1de2a85e170d641 Add files via upload


User@DCE-HPE-AIV MINGW64 ~/netology-ansible/ansible/Ansible2 (main)


---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---