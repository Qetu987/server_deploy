# deploy django with docker to server

## 👤 Добавление пользователя в группу `sudo` и переключение на него

Эта инструкция поможет добавить пользователя в группу `sudo`, создать группу с его именем (если нужно) и переключиться на него.

### 📌 Команды:

#### 1. Добавляем пользователя в группу sudo
```bash

usermod -aG sudo username
```
#### 2. Создаем группу с именем пользователя (если еще не существует)
```bash
group username
```
#### 3. Переключаемся на пользователя
```bash
su username
```

### 📝 Примечания:
- Замените `username` на имя нужного пользователя.
- Выполняйте команды от имени администратора (например, через `sudo` или от root-пользователя).
- Если группа уже существует, команда `group username` просто вернет ошибку, которую можно игнорировать.


## 💻 Установка нужного ПО

Даниый раздел содержит основные програмы, которые нужны для работы проэкта 

предпологается что у вас проект работает используя docker

### 📌 Команды:

```bash

sudo apt install nginx
```
```bash

sudo apt install docker
```
```bash

sudo apt install docker-compose
```

## ⏬ Загрузка проекта

Далее загружаем репозиторий с гита в корневой каталог пользователя

### 🏠 переходим в корневой каталог:
```bash
cd ~
```
### ⚙️ качаем репозиторий
```bash
git clone https://link...
```
### 📝 Примечания:
- Сгенерируйте ssh ключ для авторизации в git если хотите качать через ssh.
- В выпадающем меню *code* выберайте https.
- Загрузите креденшиналсы вручную через файлообменник в ваш проект.

## ⚙️ Запуск докер контейнера

Для запуска контейнера, мы должны перейти в директорию с файлом **docker-compose** после чего поднимаем контейнер
```bash
sudo docker-compose up --build -d
```
Далее проводим миграци с бд, собираем статику и создаем суперпользователя при необходимости. Для этого заходим в консоль докера и выполняем внутри команды.
```bash
sudo docker exec -it name_of_container bash
```
```bash
python manage.py migrate

python manage.py collectstatic --noinput

python manage.py createsuperuser

```






