# WebLogHW
Домашнее задание по теме: "Основы сбора и хранения логов"
Задание:
Поднимаем две машины — web и log.
На web поднимаем nginx.
Настраиваем центральный лог-сервер на любой системе по выбору:
journald;
rsyslog;
elk.
Настраиваем аудит, который будет отслеживать изменения конфигураций nginx.
Все критичные логи с web должны собираться и локально и удаленно.
Все логи с nginx должны уходить на удаленный сервер (локально только критичные).
Логи аудита должны также уходить на удаленную систему.
В первую очередь устанавливаем VirtualBox, Ansible и Vagrant
Установка VirtualBox:
apt install VirtualBox
Установка Ansible:
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
Установка Vagrant:
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
После чего создаём директорию нашего задания.
В ней мы дальше создаём Vagrantfile и директории с файлами.
В первую очередь пишем playbook и inventory файл.
После написания мы можем поднимать наши машины командой:
vagrant up
Запускаем provision:
vagrant provision
И наблюдаем выполнение. Если у нас всё выполнилось без ошибок то пристпаем к проверке:
Выполняем curl на web
curl https://localhost
Иитируем ошибку nginx:
mv /var/www/html/index.nginx-debian.html /tmp/
Меняем конфиг nginx:
nano /etc/nginx/nginx.conf
Я поменял значени строки types_hash_max_size 2048 на 4096;
Проверяем измения в /var/log/rsyslog/web/
Если все шаги выполнились и отобразились на log, то задание выполнено!
