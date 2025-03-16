
Ansible Monitoring Stack
Этот проект предоставляет набор ролей Ansible для развертывания мониторингового стека, включающего Prometheus, Node Exporter, Elasticsearch, Kibana и Fluent Bit. Роли предназначены для работы с операционными системами семейства Debian/Ubuntu.

Настройка прокси
Если вам нужно использовать прокси для доступа к заблокированным ресурсам (например, репозиториям Elasticsearch), настройте прокси следующим образом:

Создайте файл proxy.env в корне проекта со следующим содержимым:

# Настройка HTTP/HTTPS прокси
HTTP_PROXY=http://your-proxy-server:port
HTTPS_PROXY=http://your-proxy-server:port

# Если прокси требует аутентификации
HTTP_PROXY_USER=your-proxy-username
HTTP_PROXY_PASSWORD=your-proxy-password

# Исключение использования прокси для определенных хостов
NO_PROXY=localhost,127.0.0.1,internal-host.example.com


Импортируйте переменные прокси перед запуском playbook :

export $(cat proxy.env | xargs)

ansible-playbook -i inventory.yml playbook.yml --ask-become-pass

Описание ролей
1. Prometheus
Разворачивает Prometheus для мониторинга внешних хостов.
Включает создание пользователя, установку бинарных файлов и конфигурацию systemd service файла.
2. Node Exporter
Разворачивает Node Exporter для сбора метрик с хостов.
Автоматически добавляет новые хосты в конфиг Prometheus.
3. Elasticsearch + Kibana
Разворачивает Elasticsearch и Kibana для анализа логов.
Поддерживает использование прокси для доступа к заблокированным репозиториям.
4. Fluent Bit
Разворачивает Fluent Bit для сбора логов из journald и отправки их в Elasticsearch.

Параметры по умолчанию
Все параметры по умолчанию находятся в файлах defaults/main.yml каждой роли. Например:

prometheus_version: Версия Prometheus.
node_exporter_version: Версия Node Exporter.
elasticsearch_version: Версия Elasticsearch.
kibana_version: Версия Kibana.
fluentbit_version: Версия Fluent Bit.
elasticsearch_host: Хост Elasticsearch для Fluent Bit.
Вы можете изменить эти параметры под свои нужды.

Тестирование
Для тестирования можно использовать Vagrant. Создайте файл Vagrantfile для создания виртуальной машины Ubuntu и протестируйте playbook на ней.