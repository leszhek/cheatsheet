# Установка MinIO

MinIO — это высокопроизводительное решение для хранения объектов, которое часто используется для создания частных облачных хранилищ. Настройка MinIO на Raspberry Pi может быть отличным решением для домашнего или небольшого офисного облачного хранилища. Вот как все можно настроить:

### Шаг 1: Установка MinIO на Raspberry Pi
1. Обновляем систему:
```
$ apt update
$ apt dist-upgrade
```
2. Скачиваем MinIO:
    - MinIO предоставляет двоичные файлы для различных платформ. Для ARM архитектуры, которая используется в Raspberry Pi, можно загрузить соответствующий бинарный файл:
```
$ wget https://dl.min.io/server/minio/release/linux-arm/minio
$ chmod +x minio
```
3. Перемещаем файл в нужное место:
```
$ sudo mv minio /usr/local/bin
```

### Шаг 2: Настройка и ручной запуск MinIO
1. Создаем каталог для хранения данных:
```
$ mkdir ~/minio-data
```
2. Устанавливаем переменные окружения:
```
$ export MINIO_ROOT_USER=minio
$ export MINIO_ROOT_PASSWORD=minio123
```
Замени minio и minio123 на свои уникальные значения.

3. Запускаем сервер MinIO:
```
$ minio server ~/minio-data
```

### Шаг 3: Демонизация MinIO с использованием systemd
Чтобы MinIO автоматически запускался при старте системы, можно создать systemd сервис.

1. Создадим пользователя, от которого будем стартовать сервис:
```
$ sudo useradd -M minio-user
```

2. Создадим файл сервиса systemd:
```
$ sudo vi /etc/systemd/system/minio.service
```

3. Добавим туда следующее содержание:
```
[Unit]
Description=MinIO
Wants=network-online.target
After=network-online.target

[Service]
User=minio-user
Group=minio-user
ExecStart=/usr/local/bin/minio server /home/pi/minio-data

# Установи переменные окружения
Environment="MINIO_ROOT_USER=minio"
Environment="MINIO_ROOT_PASSWORD=minio123"

[Install]
WantedBy=multi-user.target
```
*Не забудь изменить minio и minio123 на свои значения.*

4. Перезагрузим daemon, чтобы systemd увидел новый файл:
```
$ sudo systemctl daemon-reload
```
5. Включи и запусти сервис:
```
$ sudo systemctl enable minio
$ sudo systemctl start minio
```
Теперь можно проверить статус сервиса:
```
$ sudo systemctl status minio
```
И посмотреть на 9000 порт по адресу малинки:
```
http://192.168.1.254:9000
```
