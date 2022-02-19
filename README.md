# Yandex Cloud
## Yandex Cloud CLI

### 1. Команды CLI для управления Yandex Cloud Functions

#### Сервисный аккаунт
- Создать сервисный аккаунт "service-account-for-cf"

```
export SERVICE_ACCOUNT=$(yc iam service-account create --name service-account-for-cf \
--description "service account for cloud functions" \
--format json | jq -r .)
```

- Список сервисных эккаунтов

```
yc iam service-account list
```

- Запись ID сервисного эккаунта в переменную SERVICE_ACCOUNT_ID

```
echo "export SERVICE_ACCOUNT_ID=<ID>" >> ~/.bashrc && . ~/.bashrc
echo $SERVICE_ACCOUNT_ID
```
- Добавить вновь созданному сервисному аккаунту роль editor:

```
echo "export FOLDER_ID=$(yc config get folder-id)" >> ~/.bashrc && . ~/.bashrc 
echo $FOLDER_ID

yc resource-manager folder add-access-binding $FOLDER_ID \
--subject serviceAccount:$SERVICE_ACCOUNT_ID \
--role editor
```

#### Yandex Cloud Functions

- Создать функцию с именем my-first-function
```
yc serverless function create --name my-first-function 
```
- Список функций

```
yc serverless function list
```
- Информация о функции my-first-function

```
yc serverless function version list --function-name my-first-function 
```

- Вызов функции по FUNCTION ID

```
yc serverless function invoke <FUNCTION ID>
```

- По умолчанию функция создаётся непубличной. Чтобы сделать функцию my-first-function публичной, вызовите следующую команду:
```
yc serverless function allow-unauthenticated-invoke my-first-function 
```

- После этого вы можете вызвать её в браузере. Получите параметр http_invoke_url для функции my-first-function:
```
yc serverless function get my-first-function 
```
