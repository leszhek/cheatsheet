# Установка и демонизация MLFlow
[Вернуться](./)

Я не настоящй датасатанист, поэтому, использую MLFlow для трекинга АБ-тестов и других экспериментов, которые необходимо сохранить, поэтому разворачивать будем под следущие задачи:
1. Сохранить гипотезу эксперимента, его рассчитанные параметры, базу на которой строился расчет
1. Сохранить документ дизайна эксперимента (в S3)
1. Сохранить фактические показатели и вывод
1. Сохранить в S3-хранилище данные об участниках эксперимента (в CSV) с делением на контрольную и целевую группы

### Шаг 1: Установка MinIO
Заглядываем [сюда](../MinIO/install.md)

### Шаг 2: Установка и подготовка Postgres
1. Установку и настройку сервера опишу как-нибудь потом (но справится можно и по любому туториалу)
1. Создаем отдельную базу для MLFlow
```
create database mlflow;
```
3. Создаем пользователя
```
create user mlflow_user with encrypted password 'your_password';
```
*Не забудьте поменять 'your_password' на что-то более личное*

4. Даем mlflow_user полные привелегии на схему public:
```
GRANT ALL PRIVILEGES ON DATABASE mlflow TO mlflow_user;
GRANT CREATE, USAGE ON SCHEMA public TO mlflow_user;
```
В 90% случаев этого должно хватить. Если не хватает добавляем:
```
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO mlflow_user;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO mlflow_user;

ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON TABLES TO mlflow_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON SEQUENCES TO mlflow_user;
```

5. Добавляем mlflow_user разрешение на вход по паролю:
```
$ sudo vi /etc/postgresql/15/main/pg_hba.conf
```
Ищем секцию с вариантами входа и прописываем:
```
local    all    mlflow_user    password
```

### Шаг 3: Установка MLFlow
1. Создадим юзера от имени которого будем стартовать демон
```
$ sudo useradd -M ml-user
```
2. Создаем каталог mlflow в /opt
```
$ sudo mkdir /opt/mlflow
$ sudo chown ml-user:ml-user /opt/mlflow
$ sudo su ml-user
```
3. Создаем виртуальное окружение и устанавливаем в него необходимый набор
```
$ sudo python -m venv venv
$ source venv/bin/activate
$ pip install mlflow psycopg2-binary boto3
```

### Шаг 4: Запускаем MLFlow сервер вручную
1. Устанавливаем переменные окружения
```
$ export MLFLOW_TRACKING_URI=http://localhost:5000
$ export MLFLOW_SQLALCHEMYSTORE_DATABASE_URI=postgresql+psycopg2://mlflow_user:yourepassword@localhost:5432/mlflow
$ export MLFLOW_S3_ENDPOINT_URL=http://localhost
$ export AWS_ACCESS_KEY_ID=your_minio_access_key
$ export AWS_SECRET_ACCESS_KEY=your_minio_secret_key
```
*Не забудьте поменять все явки и пароли на свои*

2. Запускаем сервер:
```
$ mlflow server --backend-store-uri $MLFLOW_SQLALCHEMYSTORE_DATABASE_URI --default-artifact-root s3://experiments/ --host 0.0.0.0
```
- --backend-store-uri - строка подключения к базе данных
- --default-artifact-root - бакет дефолтного хранилища (должен быть создан)

Если при старте не падаем с ошибкой - все должно работать.

### Шаг 5: Демонизация

Общее описание демонизации можно глянуть [здесь](../demonization_pyapp.md). Но, все равно, опишем наш путь:
1. Создаем сервисный файл:
```
$ sudo vi /etc/systemd/system/mlflow.service
```
2. Наполняем таким содержимым:
```
[Unit]
Description=MLflow Tracking Server
After=minio_default.service

[Service]
User=ml-user
Group=ml-user
Environment="MLFLOW_TRACKING_URI=postgresql://mlflow_user:your_password@localhost/mlflow"
Environment="MLFLOW_TRACKING_URI=postgresql://mlflow_user:your_password@localhost/mlflow"
Environment="MLFLOW_S3_ENDPOINT_URL=http://localhost:9000"
Environment="AWS_ACCESS_KEY_ID=your_minio_access_key"
Environment="AWS_SECRET_ACCESS_KEY=your_minio_secret_key"
ExecStart=/opt/mlflown/venv/bin/mlflow server --backend-store-uri $MLFLOW_TRACKING_URI --default-artifact-root s3://mlflow/ --host 0.0.0.0
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
3. Перезапустим демонов:
```
$ sudo systemctl daemon-reload
```
4. Активируем и запустим сервис:
```
$ sudo systemctl enable mlflow.service
$ sudo systemctl start mlflow.service
$ sudo systemctl status mlflow.service
```

Если все запустилось - наслаждаемся 🙂