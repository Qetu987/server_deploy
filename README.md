# 🚀 Развёртывание Django с помощью Docker на сервере
🐍 Развёртывание Django-проекта с помощью Docker, PostgreSQL и Nginx на сервер Ubuntu. Полная инструкция: установка зависимостей, запуск контейнеров, настройка Nginx, миграции и создание суперпользователя. Подходит для тестирования на сервере и staging сред.

## 👤 Добавление пользователя в группу `sudo` и переключение на него

🔧 **Команды:**

1️⃣ Добавляем пользователя в группу `sudo`:

```bash
usermod -aG sudo username
```

2️⃣ Создаем группу с именем пользователя (если еще не существует):

```bash
group username
```

3️⃣ Переключаемся на пользователя:

```bash
su username
```

📝 **Примечания:**
- 🔁 Замените `username` на имя нужного пользователя.
- ⚠️ Ошибка про существующую группу — не критична.

---

## 💻 Установка необходимого ПО

🛠️ Здесь устанавливаем всё, что нужно для работы с Docker и веб-сервером Nginx.

📦 **Команды:**

```bash
sudo apt install nginx
sudo apt install docker
sudo apt install docker-compose
```

---

## ⏬ Загрузка проекта

📁 Скачиваем проект из репозитория GitHub в домашнюю директорию пользователя.

🏠 **Переход в домашнюю директорию:**

```bash
cd ~
```

🔄 **Клонирование репозитория:**

```bash
git clone https://link...
```

📝 **Примечания:**
- 🔑 Сгенерируйте SSH-ключ, если хотите использовать `git@...`
- 🌐 Выбирайте `HTTPS` в выпадающем меню "Code"
- 📤 Загрузите `credentials` через файлообменник, если нужно

---

## 🐳 Запуск Docker-контейнера

🚀 Поднимаем контейнер и инициализируем проект.

📁 **Переходим в директорию с `docker-compose.yml` и запускаем:**

```bash
sudo docker-compose up --build -d
```

🧱 **Затем: миграции, статика, суперюзер и выход из контейнера:**

```bash
sudo docker exec -it name_of_container bash
python manage.py migrate
python manage.py collectstatic --noinput
python manage.py createsuperuser
exit
```

---

## 🌐 Настройка Nginx

### ⚙️ 1. Конфигурация веб-сервера

📂 Переходим в директорию и редактируем файл:

```bash
cd /etc/nginx/sites-available/
sudo nano default
```

🧹 Удаляем пример из файла (можно с помощью `Alt + T`) и прописываем настройки вашего проекта:

```nginx
server {
    listen 80;
    server_name ip_server or host_name;
    access_log  /var/log/nginx/example.log;

    location /static/ {
        alias /home/username/project_path/staticfiles/;
        expires 30d;
    }

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $server_name;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

---

### 🔐 2. Настройка прав доступа

👤 Пользователю `www-data` (от имени которого работает Nginx) нужно дать права на чтение/запись в нужные каталоги.

🛠 **Изменение прав на статику:**

```bash
sudo chown -R www-data:www-data /home/username/project_path/staticfiles
sudo chmod -R 755 /home/username/project_path/staticfiles
sudo systemctl reload nginx
```

👁 **Права на просмотр всех родительских директорий:**

```bash
sudo chmod o+x /home
sudo chmod o+x /home/username
sudo chmod o+x /home/username/project_path
sudo systemctl reload nginx
```
