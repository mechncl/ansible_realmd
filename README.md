### Ansible-роль для доменной авторизации на RedHad-based и Debian-based системах посредством realmd и sssd.

#### Vars:

**kerberos_user**: user - пользователь с правами на ввод в домен 

**kerberos_user_password**: password - пароль kerberos_user

**realm_domain**: domain.com - Домен Active Directory

**krb5_default_realm**: DOMAIN.COM Домен Active Directory для конфигурационного файла kerberos. Обязательно в верхнем регистре.

**realm_ad_ou**: OU=Servers_OU,DC=domain,DC=com - OU, в которой будет находится хост.

**sssd_access_filter**: (memberOf=cn=admins,ou=GROUPS,dc=domain,dc=com) - Access filter для ssd. [Подробнее в документации  sssd](https://sssd.io/docs/design_pages/active_directory_access_control.html)

#### Templates:

**krb5.conf.j2** Конфигурационный файл kerberos

**mkhomedir.j2** Настройка автоматического создзания домашней директории для пользователя в более старых debian-based дистрибутивах (в актуальных используется pam-auth-update)

**realmd.conf.j2** Конфигурационный файл realmd

**sssd.conf.j2** Конфигурационный файл sssd - именно в нем можно изменить основные настройки, такие как кеширование паролей, default shell, путь к создаваемым домашним директориям, access-filter и.т.д.
[Подробнее о конфигурационном файле realmd.comf] (https://manpages.debian.org/testing/sssd-common/sssd.conf.5.en.html)

#### roles/domain_join/tasks/main.yml
  
В блоке "Добавление админской группы в sudoers" указывается группа из Active Directory, в которую входят администраторы:
line: "%linux-admins ALL=(ALL) ALL"
